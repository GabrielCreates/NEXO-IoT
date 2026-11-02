# Arquitetura do sistema NEXO

```mermaid

flowchart TD
    A([Início: totem em modo atrativo]) --> B[Visitante se aproxima]
    B --> C[HC-SR04 mede distância a cada 200 ms]
    C --> D{Presença detectada?<br/>menos de 80 cm<br/>em 3 leituras seguidas}
    D -- Não --> C
    D -- Sim --> E[ESP32 recebe o sinal e abre sessão<br/>gera session_id aleatório]
    E --> F{Conexão Wi-Fi e API disponíveis?}
    F -- Sim --> G[Ativa interface online<br/>LED verde]
    F -- Não --> H[Modo offline: missões em cache<br/>LED amarelo]
    G --> I[Visitante pressiona Iniciar]
    H --> I
    I --> J[Seleção da área/missão<br/>Saúde, Agricultura, Indústria, Pesquisa, Energia]
    J --> K[Exibe cenário e alternativas A e B]
    K --> L[Visitante pressiona A ou B]
    L --> M[ESP32 monta JSON com event_id<br/>missao_id, escolha, tempo_resposta_ms]
    M --> N{Modo online?}
    N -- Não --> O[Resultado local do cache<br/>evento vai para fila na flash]
    N -- Sim --> P[POST HTTPS para API REST Spring Boot]
    P --> Q{Comunicação realizada com sucesso?}
    Q -- Não: timeout ou 5xx --> R[Até 3 tentativas com espera crescente]
    R --> S{Sucesso?}
    S -- Não --> O
    S -- Sim --> T
    Q -- Sim --> T{Dados válidos?}
    T -- Não: 400 --> U[API rejeita e não grava<br/>totem avisa e volta ao cenário]
    T -- Sim --> V[API processa: aplica regra do cenário]
    V --> W[Registra decisão anônima no Oracle<br/>idempotente por event_id]
    W --> X[API devolve consequência,<br/>justificativa e Mito ou Fato]
    X --> Y[Totem exibe explicação educativa]
    O --> Y
    U --> K
    Y --> Z{Nova missão?}
    Z -- Sim --> J
    Z -- Não, 45 s sem ação ou visitante saiu --> AA[Encerra sessão e apaga session_id]
    AA --> AB([Fim: volta ao modo atrativo])
    O -.-> AC[Quando a conexão volta: reenvia fila em lote]
    AD[A cada 60 s: heartbeat<br/>uptime, RSSI, firmware, fila] -.-> P
