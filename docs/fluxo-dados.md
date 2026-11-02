# Fluxo de dados

## Passo a passo

1. **Sensor → ESP32:** o HC-SR04 envia pulsos ultrassônicos, e o ESP32 mede o tempo de retorno do eco a cada 200 ms.
2. **Interpretação local:** a mediana de 3 leituras consecutivas inferiores a 80 cm indica presença. As distâncias brutas permanecem no ESP32 e não são transmitidas.
3. **Sessão:** o ESP32 gera um `session_id` aleatório, sem relação direta com a identidade do visitante, e ativa a interface do totem. A detecção de presença apenas desperta a tela. A sessão só é registrada na API após o envio de uma decisão, evitando contabilizar objetos parados como visitantes.
4. **Conteúdo:** o ESP32 solicita as missões à API por meio de `GET .../missoes` ou utiliza o conteúdo armazenado no cache local quando está offline.
5. **Decisão:** o visitante seleciona uma das opções, A ou B, e o ESP32 registra o tempo de resposta em `tempo_resposta_ms`.
6. **Envio:** o ESP32 envia a decisão à API por meio de `POST .../decisoes`, utilizando o JSON apresentado abaixo.
7. **API:** o servidor valida os dados, aplica as regras do cenário, registra a decisão no Oracle e verifica a duplicidade do evento por meio do identificador `event_id`. Em seguida, retorna o resultado.
8. **Apresentação:** o totem exibe a consequência da escolha, a justificativa educativa e o conteúdo de Mito ou Fato.
9. **Fim:** a sessão é encerrada quando o visitante sai ou após 45 segundos de inatividade. Também é encerrada quando não há presença detectada por 10 segundos. O `session_id` é removido da memória do ESP32.
10. **Processamento paralelo:** o ESP32 envia um heartbeat a cada 60 segundos, contendo informações sobre o funcionamento do equipamento.

## Exemplo de payload (proposto)

```json
{
  "event_id": "7c9e6679-7425-40de-944b-e07fc1f90ae7",
  "session_id": "b1c2d3e4-aaaa-bbbb-cccc-111122223333",
  "missao_id": "saude-001",
  "escolha": "A",
  "tempo_resposta_ms": 8200,
  "timestamp": "2026-11-01T14:30:00Z"
}
```

**Exemplo de resposta da API:**

```json
{
  "correta": false,
  "consequencia": "...",
  "justificativa": "...",
  "mito_ou_fato": {}
}
```

## Matriz de dados

| Dado | Coletado? | Transmitido? | Armazenado? | Uso |
|---|---|---|---|---|
| Distância bruta do sensor | Sim, localmente | Não | Não | Detectar presença |
| Presença (indicador booleano) | Sim | Não diretamente | Contagem agregada | Ativar o totem e contabilizar ativações |
| `session_id` aleatório | Sim | Sim | Sim | Agrupar decisões de uma mesma sessão |
| `missao_id`, escolha e tempo de resposta | Sim | Sim | Sim | Analisar decisões e indicadores agregados de aprendizagem |
| RSSI, uptime, firmware e tamanho da fila | Sim | Sim, via heartbeat | Sim, com retenção limitada | Monitorar o funcionamento do equipamento |
| Imagem, voz, biometria, nome e e-mail | Não | Não | Não | Não se aplica |

## Validação

**No ESP32 (cliente):** verificar as faixas de leitura do sensor, a validade do botão selecionado e o tamanho do JSON antes do envio.

**Na API (servidor):** verificar a presença dos campos obrigatórios, se `escolha` corresponde a A ou B, se `missao_id` existe, se `tempo_resposta_ms` está dentro de uma faixa plausível, se `event_id` possui formato UUID válido e se o timestamp está dentro da tolerância definida pelo servidor.