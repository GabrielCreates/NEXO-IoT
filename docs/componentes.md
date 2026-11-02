# Componentes, sensores e dispositivos

**Observação:** os valores e as características técnicas apresentados nesta seção foram baseados em fichas técnicas e documentações dos fabricantes (ver `referencias.md`). Eles ainda precisam ser confirmados durante uma eventual montagem e realização de testes. Nenhum dos componentes foi testado pelo grupo até o momento.

| Componente | Função | Local de instalação | Conexão | Informação produzida | Justificativa da escolha | Contribuição ao NEXO |
|---|---|---|---|---|---|---|
| **ESP32 DevKit (ESP32-WROOM-32)** | Realiza a leitura do sensor e dos botões, controla o display e o LED, além de gerenciar a comunicação, o cache e a fila de eventos. | Dentro da estrutura do totem. | Alimentação de 5 V via USB, GPIO e Wi-Fi de 2,4 GHz. | Eventos de aproximação, decisões e informações de telemetria. | Possui Wi-Fi integrado, baixo custo e suporte a comunicação HTTP e recursos de segurança TLS. | Atua como unidade central de controle do totem e realiza a comunicação com a API. |
| **HC-SR04 ou HC-SR04P (compatível com 3,3 V)** | Mede distâncias por ultrassom. O HC-SR04 possui faixa nominal de medição de 2 a 400 cm e ângulo de medição aproximado de 15°. | Na parte frontal do totem, a aproximadamente 1 m do chão. | Pinos TRIG e ECHO conectados aos GPIOs. Alimentação conforme a versão do sensor. | Medidas de distância, processadas localmente pelo ESP32. | Possui baixo custo e permite detectar aproximações sem contato físico. O HC-SR04 convencional possui saída ECHO de 5 V, exigindo adaptação para o ESP32. | Permite ativar a interface do totem quando um objeto se aproxima. |
| **Display TFT SPI de 2,8 polegadas (ILI9341)** | Exibe a tela inicial, os cenários, as alternativas A e B e os resultados das decisões. | Na parte frontal do totem, na altura adequada para visualização. | Comunicação SPI com o ESP32. | Conteúdo visual apresentado ao visitante. | Dispensa o uso de um tablet e permite o controle direto pelo ESP32. | Apresenta as informações e explicações educativas do NEXO. |
| **3 botões (Iniciar, A e B)** | Permitem iniciar a interação e registrar a alternativa escolhida pelo visitante. | Abaixo do display. | GPIO com resistores de pull-up e tratamento de debounce. | Eventos de acionamento dos botões e escolhas realizadas. | São simples, acessíveis e adequados para utilização em eventos. | Permitem que o visitante interaja com os cenários e registre suas decisões. |
| **LED RGB ou conjunto de 3 LEDs** | Indica o estado de funcionamento: verde para online, amarelo para offline e vermelho para falhas. | Na parte superior ou frontal do totem. | GPIO com resistores adequados. | Estado operacional do equipamento. | Permite identificar visualmente a situação do totem sem abrir sua estrutura. | Facilita o acompanhamento do funcionamento durante eventos. |
| **Fonte de alimentação de 5 V / 2 A** | Fornece energia para os componentes do totem. | Dentro da estrutura, com ventilação e isolamento adequados. | Conexão à rede elétrica por meio de uma fonte apropriada. | Alimentação elétrica dos componentes. | A capacidade prevista oferece uma margem inicial para o funcionamento do ESP32, do sensor e do display. O dimensionamento definitivo depende da medição do consumo. | Permite o funcionamento contínuo do equipamento durante os eventos. |

## Componentes avaliados e descartados

| Item | Motivo |
|---|---|
| Sensor PIR | Detecta movimento, mas não mede distância. Por isso, não foi escolhido para a estratégia de detecção de aproximação proposta. |
| Câmera e reconhecimento facial | Não são necessários para a interação e aumentariam a coleta de dados pessoais, contrariando o princípio de minimização de dados. |
| Buzzer | Não apresenta uma contribuição educativa clara para a proposta e pode gerar ruído desnecessário em ambientes de eventos. |
| DHT22 e sensores ambientais | Não contribuem diretamente para o objetivo educativo do totem. |
| RFID/NFC | Poderia exigir identificação do visitante, o que não é necessário para a interação proposta. |
| Broker MQTT dedicado | Foi considerado desnecessário nesta etapa, pois a comunicação proposta utiliza requisições HTTPS diretamente com a API REST. |

## Alimentação, montagem e limitações

- **Alimentação:** está prevista uma fonte de 5 V / 2 A. O consumo real dos componentes deverá ser medido durante a montagem.
- **Instalação do sensor:** o HC-SR04 deverá ficar em uma posição sem obstruções e distante de superfícies que possam provocar reflexões indesejadas do ultrassom.
- **Detecção de aproximação:** o limite inicial proposto é de 80 cm, podendo ser ajustado após testes.
- **Limitações do HC-SR04:** o sensor pode apresentar leituras imprecisas devido a ruídos, superfícies macias, obstáculos e ângulos inadequados. Como forma de reduzir essas interferências, propõe-se utilizar a mediana de três leituras e um intervalo mínimo entre detecções.
- **Falsas detecções:** objetos parados, como cadeiras, também podem ser detectados. Por isso, a aproximação apenas ativará a interface. A interação somente será contabilizada após o visitante pressionar o botão Iniciar e registrar uma decisão.
- **Segurança elétrica:** a saída ECHO do HC-SR04 convencional pode fornecer 5 V, enquanto os GPIOs do ESP32 operam com lógica de 3,3 V. Portanto, é obrigatório utilizar um divisor de tensão ou outro circuito de adaptação compatível. Como alternativa, poderá ser utilizado um modelo HC-SR04P com saída compatível com 3,3 V, desde que suas especificações sejam confirmadas na documentação do fabricante.

**Nota:** todos os componentes, valores e procedimentos descritos representam uma proposta para o totem IoT. A montagem, os testes elétricos e a validação do funcionamento ainda não foram realizados pelo grupo.