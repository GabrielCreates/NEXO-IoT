# IoB e Generative AI

## IoB (Internet of Behavior): proposta inicial, com dados agregados

| Pergunta | Dado utilizado | Aplicação |
|---|---|---|
| Quais missões são mais acessadas? | Contagem por `missao_id` | Identificar conteúdos de maior interesse e priorizar novas missões |
| Quais áreas despertam mais interesse? | Contagem por área | Orientar a criação de novos conteúdos |
| Em quais etapas há maior abandono? | Quantidade de sessões que não avançam em cada etapa | Identificar pontos que podem precisar de melhorias |
| Quanto tempo dura cada experiência? | `tempo_resposta_ms` | Avaliar o ritmo das atividades e ajustar a duração dos cenários |
| Quais alternativas são mais escolhidas? | Distribuição das escolhas A/B | Identificar padrões de resposta e orientar melhorias no conteúdo de Mito ou Fato |

A análise será realizada com dados agregados, sem identificação direta dos visitantes e sem utilização de biometria. Os resultados serão apresentados em relatórios estatísticos, sem acompanhamento individual.

## Generative AI: possibilidade futura (fora da proposta inicial)

A utilização de Inteligência Artificial Generativa poderá ser considerada em versões futuras do projeto, com possibilidades como:

- Produção de explicações complementares e adaptação da linguagem para diferentes níveis de ensino, como Ensino Fundamental e Ensino Médio.
- Elaboração de rascunhos para novas missões, sempre com revisão humana e validação de especialistas antes da publicação.
- Sistema de perguntas e respostas sobre aplicações civis da tecnologia nuclear.

**Como limitar erros e riscos:**

A utilização futura da IA deverá contar com uma base curada de documentos confiáveis, consultada por meio de RAG (*Retrieval-Augmented Generation*). As respostas deverão se limitar às informações disponíveis nessa base, apresentar as fontes utilizadas e recusar perguntas quando não houver conteúdo suficiente para respondê-las.

Também estão previstas a revisão humana dos conteúdos gerados antes da publicação, a definição de temas fora do escopo (como instruções sensíveis) e a utilização de registros de prompts sem dados pessoais.

Como o totem poderá funcionar offline e não contará necessariamente com moderação presencial, qualquer integração com IA deverá ocorrer em uma versão controlada, com limites de utilização e mecanismos de segurança definidos.