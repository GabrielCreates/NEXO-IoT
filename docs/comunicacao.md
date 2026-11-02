# Comunicação e redes

## Decisão: Wi-Fi + HTTPS + REST/JSON

- **Wi-Fi:** o ESP32 possui comunicação sem fio compatível com os padrões 802.11 b/g/n na faixa de 2,4 GHz. Essa tecnologia permite a conexão do totem a redes locais disponíveis no ambiente.
- **HTTPS/REST:** permite reutilizar a API Spring Boot prevista para o projeto. O modelo de requisição e resposta combina com o funcionamento da aplicação: o totem envia uma decisão e recebe a explicação correspondente. Também facilita os testes e a depuração com ferramentas como curl e Postman.
- **JSON:** formato utilizado na troca de dados entre o ESP32 e a API, com suporte direto no ecossistema Spring.

## Por que não MQTT neste momento

| Critério | HTTP/REST | MQTT |
|---|---|---|
| Fluxo principal (decisão → resposta educativa) | Adequado ao modelo de requisição e resposta | Exige configuração de tópicos para solicitação e resposta |
| Infraestrutura adicional | Não exige um broker separado | Exige um broker, como o Mosquitto |
| Frequência de eventos | Adequado ao baixo volume previsto, de alguns eventos por minuto | Apresenta vantagens em cenários com muitos dispositivos e mensagens |
| Conhecimento da equipe | Aproveita a experiência com Spring Boot | Exige o aprendizado de uma nova tecnologia |

**Limitação aceita:** o HTTP possui maior sobrecarga de cabeçalho e não oferece comunicação push nativa no modelo utilizado pelo projeto.

**Possibilidade de evolução:** adoção de MQTT para telemetria e envio de comandos remotos em uma frota maior de totens, caso essa necessidade seja identificada.

## Comportamento em falhas

| Situação | Reação prevista |
|---|---|
| Sem internet ou queda do Wi-Fi | LED amarelo; funcionamento offline com 2 a 3 missões armazenadas em cache no LittleFS; eventos são adicionados à fila para envio posterior; reconexão automática com backoff |
| Falha na API (timeout ou erro 5xx) | Até 3 tentativas, com intervalos de 1 s, 2 s e 4 s. Após as tentativas, o totem apresenta o resultado local e mantém o evento na fila |
| Reinicialização do ESP32 | O equipamento retorna ao estado OCIOSO. A fila de eventos permanece armazenada na memória flash, enquanto a sessão em andamento é descartada |
| Dados inválidos | A API responde com código HTTP 400, não grava o evento e o totem solicita uma nova escolha |
| Evento duplicado após reenvio | O identificador único `event_id` (UUID) permite identificar reenvios e garantir a idempotência da operação |
| Falha do HC-SR04 (leituras fora de faixa ou timeout) | LED vermelho; como alternativa, o botão Iniciar permite ativar o totem manualmente. A falha é registrada no heartbeat |
| Rede com portal cativo ou isolamento de clientes | O ESP32 pode não conseguir estabelecer comunicação com a API. Como alternativa, pode ser utilizado um roteador ou hotspot dedicado ao totem. Sem conexão, o funcionamento offline é mantido |
| Relógio incorreto | O ESP32 tenta sincronizar o horário por NTP quando há conexão. Sem sincronização, os eventos utilizam tempo relativo, e o servidor registra o horário de recebimento |

## Segurança da comunicação

A comunicação entre o ESP32 e a API utilizará HTTPS com TLS, com certificados raiz disponibilizados no firmware. Cada dispositivo terá um token próprio, armazenado no NVS, com previsão de rotação e revogação vinculadas ao `totem_id`.

Também estão previstas medidas como limitação de requisições (*rate limit*) na API, utilização de Wi-Fi com WPA2 ou WPA3 em uma rede separada e exclusão de credenciais do repositório do projeto.

Mais detalhes sobre os cuidados com os dados e a proteção da aplicação estão descritos em `privacidade-e-seguranca.md`.