# Arquitetura do sistema NEXO

```mermaid

flowchart LR
    subgraph FISICA[Camada física - Totem]
      S1[HC-SR04<br/>presença]
      B1[Botões Iniciar / A / B]
      D1[Display TFT 2,8 pol.]
      L1[LED de status]
      E1[ESP32<br/>firmware + cache + fila]
      S1 --> E1
      B1 --> E1
      E1 --> D1
      E1 --> L1
    end
    subgraph COM[Camada de comunicação]
      W[Wi-Fi 2,4 GHz<br/>WPA2/WPA3 + TLS]
    end
    subgraph APP[Aplicação e processamento]
      API[API REST<br/>Java Spring Boot]
      V[Validação e regras<br/>do cenário]
    end
    subgraph DB[Persistência]
      O[(Oracle)]
    end
    subgraph USO[Apresentação e uso]
      M[App Flutter NEXO]
      P[Painel de professores<br/>futuro]
    end
    E1 -- HTTPS/JSON --> W --> API
    API --> V --> O
    API -- resposta educativa --> E1
    O -.-> M
    O -.-> P
