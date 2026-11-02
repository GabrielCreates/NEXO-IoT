# Arquitetura técnica

**Diagrama:** `diagramas/arquitetura.mmd`

| Camada | Elementos | Situação |
|---|---|---|
| Física | HC-SR04, botões, display, LED e ESP32 | Proposta |
| Comunicação | Wi-Fi 2,4 GHz (WPA2/WPA3 Personal) e TLS | Proposta |
| Aplicação | API REST com Spring Boot | API prevista no NEXO; endpoints específicos do totem são propostos |
| Processamento | Validação de dados (Bean Validation), regras dos cenários e idempotência | Proposta |
| Persistência | Oracle | Banco previsto no NEXO; tabelas específicas do totem são propostas |
| Apresentação | Display do totem e aplicativo Flutter para acompanhamento do progresso | Totem: proposta; aplicativo: previsto |

## Firmware (ESP32) e máquina de estados

O funcionamento do firmware será organizado em estados:

`OCIOSO` → `PRESENÇA` → `MENU` → `CENÁRIO` → `AGUARDA_ENVIO` → `RESULTADO` → (`MENU` ou `OCIOSO`).

A sessão será encerrada quando ocorrer uma das seguintes condições:

- 45 segundos sem interação do visitante;
- 10 segundos sem detecção de presença pelo sensor.

Após o encerramento, o totem retornará ao modo atrativo, aguardando a aproximação de um novo visitante.

## Decisão de projeto (display)

O totem utilizará firmware próprio no ESP32 e um display TFT para apresentar as missões, alternativas e resultados. O aplicativo Flutter continuará sendo utilizado como experiência individual e para acompanhamento do progresso do usuário.

Por isso, cada cenário deverá ter uma versão resumida, adequada à tela do totem e disponibilizada pela API, mantendo as informações necessárias para apresentar as alternativas e suas respectivas explicações educativas.

## Endpoints propostos para validação com a equipe de backend

| Método | Rota | Finalidade |
|---|---|---|
| GET | `/api/totens/{totemId}/missoes` | Lista resumida de missões e cenários para exibição no display |
| POST | `/api/totens/{totemId}/decisoes` | Envia a decisão do visitante e recebe a consequência e o conteúdo de Mito ou Fato |
| POST | `/api/totens/{totemId}/heartbeat` | Envia informações técnicas para monitoramento do funcionamento do totem |

**Observação:** as rotas acima representam uma proposta de integração e ainda precisam ser validadas com a equipe responsável pelo backend.

## Modelo de dados proposto (Oracle)

As seguintes tabelas são sugeridas para armazenar as informações relacionadas ao totem:

- `TOTEM (id, local, data_instalacao, status)`
- `SESSAO_TOTEM (id_aleatorio, totem_id, inicio, fim)`
- `DECISAO_TOTEM (event_id, sessao_id, missao_id, escolha, tempo_resposta_ms, criado_em)`
- `TOTEM_HEARTBEAT (totem_id, criado_em, rssi, uptime_s, fw_version, fila_pendente)`

Esse modelo é uma proposta inicial e poderá ser ajustado conforme a definição da estrutura do banco de dados e a integração com a API.

## Relação com o restante do NEXO

Os cenários, as alternativas e as decisões do totem poderão reutilizar o modelo de dados e as regras previstos para a API do NEXO.

O funcionamento do totem não dependerá do aplicativo Flutter nem da prova de conceito desenvolvida em .NET. A comunicação com o backend será realizada diretamente pela API REST, utilizando o ESP32.