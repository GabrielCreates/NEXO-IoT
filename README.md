
# NEXO — Totem Educativo Inteligente
### NEXO — Aprenda. Decida. Descubra.

**Disciplina:** Disruptive Architectures: IoT, IoB & Generative AI — 1º Sprint  
**Natureza da entrega:** Proposta conceitual

> Esta entrega apresenta uma proposta conceitual de aplicação de IoT ao projeto NEXO. Nenhum totem foi construído, nenhum sensor foi testado e a integração ESP32 ↔ API ainda não foi implementada nesta etapa.

## Equipe

- Gabriel Rocha de Souza — RM 567023
- Gabriel Stuani — RM 566682
- Luis Fillipe Rodrigues — RM 567918
- Luiz Felipe Kichimoto — RM 567726
- Guilherme Soares — RM 568227

## 1. Apresentação do NEXO

O NEXO é um projeto educacional que busca ensinar a sociedade sobre as aplicações civis da tecnologia nuclear por meio de um jogo educativo.

O projeto apresenta quatro funcionalidades principais:

- **Mapa de Missões:** exploração de áreas como Saúde, Agricultura, Indústria, Pesquisa e Energia.
- **Laboratório de Decisões:** apresentação de cenários com alternativas A e B.
- **Mito ou Fato:** esclarecimento de informações relacionadas à tecnologia nuclear.
- **Perfil de Progresso:** acompanhamento das missões e atividades realizadas.

## 2. Problema

A tecnologia nuclear costuma ser associada principalmente a acidentes e armas. Entretanto, existem aplicações civis importantes em áreas como diagnóstico e tratamento médico, conservação de alimentos, ensaios industriais, pesquisa científica e geração de energia.

A falta de conhecimento sobre essas aplicações dificulta a compreensão de seus benefícios, limitações e cuidados.

## 3. Objetivo

Promover o conhecimento sobre as aplicações civis da tecnologia nuclear, permitindo que o público compreenda:

- Onde a tecnologia nuclear pode ser utilizada;
- Quais são seus benefícios e limitações;
- Quando sua utilização faz sentido;
- Quais cuidados devem ser considerados;
- Como diferenciar informações científicas de desinformação.

O jogo não apresenta a tecnologia nuclear como uma solução sempre superior. Em determinados cenários, outras alternativas podem ser mais adequadas.

## 4. Público-alvo

**Principal:** estudantes e jovens interessados em ciência e tecnologia.

**Público de apoio:** professores, instituições de ensino e divulgadores científicos.

## 5. Aplicação de IoT: Totem NEXO

O Totem NEXO é uma proposta de equipamento físico para escolas, laboratórios, feiras de ciências e exposições.

Ele funcionaria como uma extensão física do jogo educativo, permitindo que visitantes interajam com as missões por meio de sensores, display e botões físicos.

### Funcionalidades propostas

- Detectar a aproximação de visitantes;
- Ativar automaticamente a interface de interação;
- Apresentar missões educativas;
- Receber decisões por botões físicos A/B;
- Exibir consequências, justificativas e conteúdos Mito ou Fato;
- Registrar interações de forma anônima;
- Enviar informações técnicas do equipamento para monitoramento remoto.

### Por que não seria apenas um tablet?

| Capacidade | Tablet | Totem NEXO |
|---|---|---|
| Detectar aproximação | Não necessariamente | Sensor de presença |
| Ativação automática | Não | Sim |
| Reinício automático | Não necessariamente | Sessão por presença e timeout |
| Interação em eventos | Tela touchscreen | Botões físicos A/B |
| Monitoramento remoto | Não necessariamente | Heartbeat para API |
| Dados por equipamento | Não necessariamente | Eventos associados ao totem_id |

## 6. Funcionamento proposto

1. O visitante se aproxima do equipamento.
2. O sensor HC-SR04 identifica a proximidade.
3. O ESP32 confirma a presença.
4. A tela sai do modo atrativo.
5. O visitante pressiona Iniciar.
6. Seleciona uma área ou missão.
7. O totem apresenta um cenário e alternativas A/B.
8. O visitante realiza sua escolha por botão físico.
9. O ESP32 envia a decisão por HTTPS à API.
10. A API valida a informação, registra a interação e retorna consequência, justificativa e conteúdo Mito ou Fato.
11. O totem apresenta a explicação educativa.
12. A sessão é encerrada após a saída do visitante ou período de inatividade.

Detalhamento:
- [Proposta IoT](docs/proposta-iot.md)
- [Fluxo de dados](docs/fluxo-dados.md)

## 7. Componentes e sensores

| Componente | Função |
|---|---|
| ESP32 DevKit | Processamento e comunicação Wi-Fi |
| HC-SR04 ou HC-SR04P | Detecção de proximidade |
| Display TFT SPI 2,8" | Apresentação das missões |
| 3 botões físicos | Iniciar, A e B |
| LED de status | Indicação de funcionamento |
| Fonte 5 V | Alimentação do equipamento |

Componentes descartados e respectivas justificativas:
- [Componentes](docs/componentes.md)

## 8. Arquitetura

A arquitetura proposta possui seis camadas:

1. **Física:** ESP32, sensor, display e botões.
2. **Comunicação:** Wi-Fi.
3. **Aplicação:** API REST.
4. **Processamento:** Java e Spring Boot.
5. **Persistência:** Oracle.
6. **Apresentação:** display do totem e aplicativo NEXO.

Documentação:
- [Arquitetura](docs/arquitetura.md)
- [Diagrama de arquitetura](diagramas/arquitetura.mmd)

## 9. Comunicação e protocolos

A comunicação proposta utiliza:

- Wi-Fi 2,4 GHz;
- HTTPS;
- API REST;
- JSON.

A proposta reaproveita a API Spring Boot prevista no projeto NEXO.

O MQTT foi avaliado, mas não adotado nesta etapa. Sua utilização poderá ser considerada em futuras evoluções.

- [Comunicação](docs/comunicacao.md)

## 10. Processamento dos dados

As distâncias brutas medidas pelo sensor permanecem no ESP32 e são descartadas.

Os dados transmitidos são limitados a:

- `totem_id`;
- `session_id` aleatório;
- `missao_id`;
- escolha A/B;
- tempo de resposta;
- informações técnicas do equipamento.

- [Fluxo de dados](docs/fluxo-dados.md)

## 11. Benefícios

A proposta permite:

- Ativação automática do equipamento;
- Interação física acessível;
- Reinício automático das sessões;
- Registro anônimo das decisões;
- Monitoramento remoto;
- Análise agregada de utilização;
- Melhorias futuras no conteúdo educativo.

Os benefícios estão relacionados aos componentes e funcionalidades apresentados na documentação:

- [Proposta IoT](docs/proposta-iot.md)

## 12. Segurança e privacidade

A proposta considera:

- Comunicação protegida por TLS;
- Token individual por dispositivo;
- Validação dos dados no servidor;
- Idempotência para evitar duplicação de eventos;
- Ausência de câmera, microfone e biometria;
- Não coleta de dados pessoais;
- Sessões aleatórias;
- Agregação mínima de dados nos relatórios.

- [Privacidade e segurança](docs/privacidade-e-seguranca.md)

## 13. IoB e Generative AI

### IoB — Internet of Behaviors

A proposta considera o uso de informações agregadas e anônimas para compreender a experiência educativa:

- Missões mais acessadas;
- Áreas de maior interesse;
- Abandono por etapa;
- Tempo por missão;
- Frequência das escolhas A/B.

### Generative AI

A utilização de IA generativa é considerada uma possibilidade futura, fora do escopo inicial.

Poderá auxiliar na apresentação de explicações complementares e na adaptação da linguagem, utilizando uma base de conteúdo previamente curada.

- [IoB e Generative AI](docs/iob-e-genai.md)

## 14. Fluxograma

O fluxograma representa o funcionamento proposto do equipamento, desde a detecção de presença até o encerramento da sessão.

Arquivo editável:
- [Fluxograma Mermaid](diagramas/fluxograma.mmd)

## 15. Tecnologias previstas

| Tecnologia | Situação |
|---|---|
| Flutter | Aplicativo mobile previsto no projeto NEXO |
| Java + Spring Boot | API REST prevista |
| Oracle | Banco de dados previsto |
| .NET | Prova de conceito independente |
| Docker + VM Azure | Infraestrutura prevista para API |
| ESP32 | Proposta conceitual do totem |
| HC-SR04 | Sensor de proximidade proposto |
| Display TFT | Interface física proposta |
| HTTPS/REST | Comunicação conceitual entre totem e API |
| MQTT | Avaliado, não adotado nesta etapa |
| IA generativa | Possibilidade futura |

## 16. Limitações

- O equipamento ainda não foi construído nem testado.
- Alcance, consumo e estabilidade dependem de validação prática.
- Os endpoints específicos do totem ainda não existem na API.
- As tabelas relacionadas ao totem são propostas.
- Redes com portal cativo ou isolamento de clientes podem impedir a comunicação.
- O HC-SR04 detecta objetos, não identifica especificamente pessoas.
- Superfícies que absorvem som e ruídos podem afetar as medições.
- O display pequeno limita a quantidade de texto.
- O modo offline depende de conteúdo previamente armazenado.
- O conteúdo em cache não é atualizado enquanto o equipamento estiver offline.

## 17. Próximos passos

Como evolução futura, propõe-se:

1. Construção de um protótipo de bancada;
2. Validação do sensor de proximidade;
3. Teste dos botões físicos;
4. Implementação da interface no display;
5. Desenvolvimento dos endpoints específicos;
6. Teste de envio de eventos;
7. Validação da comunicação HTTPS;
8. Avaliação da experiência dos visitantes.

## 18. Vídeo de apresentação

[Assistir ao vídeo de apresentação do NEXO — IoT, IoB & Generative AI](https://www.youtube.com/watch?v=ZRO7rws0h0Y)

## 19. Estrutura do repositório

```text
nexo-iot/
├── README.md
├── docs/
│   ├── proposta-iot.md
│   ├── componentes.md
│   ├── arquitetura.md
│   ├── comunicacao.md
│   ├── fluxo-dados.md
│   ├── privacidade-e-seguranca.md
│   ├── iob-e-genai.md
│   └── referencias.md
├── diagramas/
│   ├── fluxograma.mmd
│   └── arquitetura.mmd
├── slide-nexo (arquivo de apresentação.pdf)
└── entrega/
    ├── checklist-e-auditoria.md
    └── links.txt
```

## 20. Conclusão

O Totem NEXO propõe uma aplicação de IoT que amplia a experiência educativa do projeto por meio de interação física, detecção de presença, registro anônimo de decisões e monitoramento remoto.

A proposta utiliza uma arquitetura compatível com as tecnologias previstas no projeto, mantendo atenção à privacidade, segurança e limitações técnicas.

O próximo passo é transformar a proposta conceitual em um protótipo de bancada, permitindo validar os componentes, a interação física e a comunicação com a API.

---

**NEXO — Aprenda. Decida. Descubra.**
