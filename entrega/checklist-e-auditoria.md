# Checklist final e auditoria

## Ações manuais pendentes do grupo (a entrega NÃO está 100% concluída sem elas)
- [ ] Gravar e publicar o vídeo (YouTube, Não listado) e colar o link em `entrega/links.txt`
- [ ] Criar o repositório no GitHub, subir estes arquivos e colar o link
- [X] Conferir todos os links de `docs/referencias.md` (não foram abertos/validados neste ambiente)
- [X] Confirmar com o time de backend se os endpoints/tabelas propostos são viáveis
- [X] Confirmar o que já está implementado (app, API, Oracle, .NET, Docker/Azure) e ajustar a tabela "Tecnologias previstas" do README
- [ ] Exportar os diagramas Mermaid como PNG (ex.: mermaid.live) para o vídeo
- [ ] Conferir se o README renderiza o Mermaid no GitHub
- [ ] Abrir o ZIP e verificar que nenhum arquivo está vazio/corrompido; enviar antes do prazo
- [ ] Decidir a divisão de falas do vídeo (a do roteiro é sugestão)

## Estrutura do ZIP
```
NEXO_Sprint1_IoT.zip
├── README.md
├── docs/ (8 arquivos .md)
├── diagramas/ (fluxograma.mmd, arquitetura.mmd)
├── video/roteiro-video.md
└── entrega/ (links.txt, checklist-e-auditoria.md)
```

## Auditoria pelos critérios (autoavaliação, não nota garantida)
| Critério | Pontos | Evidência | Ponto fraco |
|---|---|---|---|
| Aplicação de IoT/Redes | 50 | Totem ligado ao jogo; componentes justificados; protocolo escolhido com trade-off; falhas tratadas; fluxograma | Sem protótipo/testes; valores de alcance e consumo não medidos |
| Apresentação | 20 | Roteiro por tempo, com tela e transição | Depende da execução do grupo |
| Organização e documentação | 20 | Repo estruturado, tabelas, referências | Links não validados; docs longas |
| Inovação e originalidade | 10 | Sessão por presença, botões físicos, offline, IoB anônimo | Totem em si é ideia comum em museus; diferencial está na integração com decisões A/B |

## Revisão crítica / possíveis contradições
1. **Display TFT vs. app Flutter:** o totem usa firmware próprio no ESP32, não o Flutter. Conteúdo precisa de versão resumida; o grupo deve aceitar essa decisão.
2. **Cache offline** só mostra missões já gravadas; avaliar se isso agrada ao professor.
3. **HC-SR04** detecta objetos, não pessoas; presença pode ser falsa (ex.: cadeira).
4. **ECHO 5 V** no ESP32 (3,3 V) é risco de hardware se ignorado.
5. **Rede de eventos** com portal cativo pode impedir Wi-Fi; plano B: hotspot/roteador próprio.
6. **LGPD:** se o público incluir menores e houver cadastro futuro, validar com a instituição.
7. **Origem do conteúdo:** os textos educativos devem ser revisados por fonte confiável (ex.: IAEA/CNEN) antes de aparecerem no totem.
