# Proposta de IoT: Totem Educativo Inteligente do NEXO

> **Status: proposta conceitual.** Nenhum dos componentes ou recursos descritos nesta seção foi construído ou testado pelo grupo.

## 1. Onde IoT e Redes entram no NEXO

A aplicação de IoT está na camada de **acesso físico** ao jogo, por meio de um totem instalado em escolas, laboratórios, feiras e exposições.

O aplicativo mobile, desenvolvido em Flutter, continuará oferecendo uma experiência individual. Já o totem permitirá que visitantes interajam com o NEXO durante eventos presenciais, sem precisar instalar aplicativos ou realizar cadastro.

## 2. Necessidade atendida

1. Atrair e engajar visitantes em ambientes presenciais, sem exigir download ou cadastro.
2. Facilitar o acesso de professores e organizadores às missões educativas.
3. Monitorar o funcionamento do equipamento e acompanhar sua utilização por meio de dados agregados, sem identificação direta dos visitantes.
4. Oferecer uma interação simples e adequada ao uso contínuo por diferentes visitantes, utilizando botões físicos A/B.

## 3. Sensores e dispositivos

(Ver detalhes em `componentes.md`.)

- **HC-SR04:** mede a distância para detectar a aproximação de visitantes.
- **ESP32:** realiza a leitura do sensor e dos botões, controla o display e o LED e gerencia a comunicação Wi-Fi, o HTTPS, o cache local e a fila de eventos.
- **Display TFT:** apresenta os cenários, as alternativas e os resultados das escolhas.
- **Três botões físicos (Iniciar, A e B):** permitem iniciar a experiência e registrar as decisões dos visitantes.
- **LED de status:** indica as condições de funcionamento: verde para online, amarelo para offline e vermelho para falha.

## 4. Instalação

A proposta considera um totem com aproximadamente 1,2 m de altura, construído em MDF ou acrílico.

O sensor ficará instalado na parte frontal, a aproximadamente 1 m do chão. O display será posicionado na altura dos olhos, entre aproximadamente 1,1 m e 1,3 m. Os botões ficarão abaixo do display.

O ESP32 e a fonte de alimentação serão instalados dentro da estrutura, em compartimento ventilado e próximo a uma tomada.

## 5. Quem utiliza as informações

| Usuário | Informação utilizada | Finalidade |
|---|---|---|
| Visitante | Cenário, consequência e conteúdo de Mito ou Fato | Aprender por meio da interação |
| Professor ou organizador | Status do totem e dados agregados de utilização | Acompanhar a atividade |
| Equipe NEXO | Estatísticas agregadas por missão | Avaliar e melhorar os conteúdos |

## Benefícios

| Benefício | Como a arquitetura contribui |
|---|---|
| Ativação automática | HC-SR04 combinado à regra de três leituras inferiores a 80 cm, processadas no ESP32 |
| Interação física acessível | Botões A/B conectados aos GPIOs, com tratamento de debounce |
| Encerramento automático da sessão | Encerramento após 10 segundos sem presença ou 45 segundos sem atividade |
| Registro de interações | En