# Privacidade e segurança

## Princípios

- **Minimização de dados:** são transmitidas apenas as informações necessárias para o funcionamento das experiências educativas e para a melhoria dos conteúdos.
- **Privacidade desde a concepção:** o projeto não prevê login, coleta de nome, utilização de câmera, microfone ou dados biométricos.
- **Sessão temporária:** cada experiência utiliza um `session_id` aleatório, sem associação direta à identidade do visitante. O identificador é descartado ao final da sessão e não é utilizado para relacionar visitas diferentes.
- **Agregação:** os relatórios apresentam informações agrupadas, com previsão de um mínimo de 5 sessões por grupo, como medida para reduzir o risco de reidentificação.
- **Transparência:** será exibido um aviso no totem informando: "Este totem registra escolhas de forma anônima para melhorar o conteúdo."

## LGPD

A proposta busca evitar a coleta de dados pessoais, utilizando informações relacionadas às escolhas e interações dos visitantes.

Caso sejam incluídos recursos de cadastro no futuro, como identificação de usuários ou vinculação a turmas, será necessário avaliar a base legal aplicável, a finalidade do tratamento e a transparência das informações fornecidas aos titulares.

No caso de crianças e adolescentes, deverão ser observadas as disposições específicas da Lei Geral de Proteção de Dados (Lei nº 13.709/2018), especialmente o artigo 14.

**As medidas deverão ser avaliadas e validadas com a orientação da instituição antes de qualquer implementação.**

## Segurança

| Risco | Mitigação prevista |
|---|---|
| Interceptação de dados | HTTPS com TLS 1.2 ou superior e validação de certificados |
| Dispositivo falso | Token individual por totem, com possibilidade de revogação |
| Reenvio ou duplicação de eventos | Identificador `event_id` para garantir idempotência e validação de timestamp |
| Injeção de dados inválidos | Bean Validation e consultas parametrizadas |
| Abuso ou excesso de requisições | Limitação de requisições (*rate limit*) por `totem_id` |
| Rede de eventos insegura | Utilização de rede dedicada, protegida por WPA2 ou WPA3 |
| Vazamento de credenciais | Não armazenar segredos no GitHub; utilizar NVS no dispositivo e variáveis de ambiente no servidor |
| Firmware desatualizado | Identificação da versão do firmware por heartbeat e previsão de atualização OTA assinada em versões futuras |
| Roubo físico do equipamento | Utilização de caixa lacrada e possibilidade de revogação do token do dispositivo |

## Honestidade técnica

Todas as medidas de privacidade e segurança descritas nesta seção são **planejadas**. Nenhuma delas foi implementada ou testada pelo grupo até o momento. Sua adoção dependerá das etapas futuras de desenvolvimento e validação.