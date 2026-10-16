# AI Agents na Vida Real: Arquiteturas, Segurança e Por Onde Começar

Material da palestra de **Ahirton Lopes, PhD** no **23º Latinoware** (16/10/2026, Foz do Iguaçu).

Os agentes de IA estão deixando de ser chatbots e passando a usar ferramentas, acessar APIs e agir sozinhos. A palestra mostra como eles funcionam em produção: o laço modelo → ferramenta → observação, MCP, memória, multiagente, o ecossistema Google (ADK, Gemini) e Microsoft (Foundry, Agent Framework), e o que muda em segurança, observabilidade e governança quando o erro deixa de ser *dizer* algo errado e passa a ser *fazer* algo errado.

O exemplo é um case **fictício**: a **Cooperativa Grão Tríplice**, inventada, do oeste do Paraná. Nomes, cooperados, romaneios e valores são falsos.

## Conteúdo

| Arquivo | O que é |
|---|---|
| `Palestra_Ahirton_Latinoware.pdf` | Slides da palestra |
| `demo/demo_latinoware.ipynb` | Notebook da demo com Google ADK + MCP + Gemini, pronto para rodar no Colab |
| `demo/servidor_grao_triplice.py` | O servidor MCP da cooperativa, para rodar sozinho (o notebook também o grava) |

## A demo

Um assistente feito com **Google ADK** usa as ferramentas de um sistema interno exposto por um **servidor MCP**. Cada cenário termina com **o ponto que o teste prova**.

| Cenário | O que mostra |
|---|---|
| 1 | O agente descobre as ferramentas pelo MCP, inclusive uma que ele nunca deveria ter |
| 2 | Uso normal: o caminho feliz não prova segurança |
| 3 | **Injeção indireta:** a instrução chega dentro de um dado (o campo de observações de um romaneio) e o agente faz um adiantamento para o cadastro errado |
| 4 | **Defesa em camadas:** menor privilégio (`tool_filter`), identidade fixa e limite em código (`before_tool_callback`) |
| 5 | A mesma defesa testada **sem o modelo**, de forma determinística |
| 6 | A trilha: só o registro do sistema prova o que o agente fez |

### Como rodar

1. Gere uma API key gratuita no [Google AI Studio](https://aistudio.google.com/apikey).
2. **No Colab:** abra o notebook, crie o secret `GOOGLE_API_KEY` (ícone de chave na barra lateral) e libere o acesso do notebook.
   **Local:** `pip install "google-adk>=2.11,<3" "mcp>=2.3,<3" jupyter`, `export GOOGLE_API_KEY=...` e abra o notebook.
3. Rode a célula do `%pip install` e, no Colab, **reinicie a sessão** (Ambiente de execução > Reiniciar sessão). O Colab já vem com um `google-adk` antigo, sem `McpToolset`.
4. Execute as células **uma a uma**, a partir da configuração. Não use "Executar tudo": o plano gratuito do AI Studio tem limite de requisições por minuto.

Para usar a Vertex AI no seu projeto GCP, troque `USE_VERTEX_AI = True` e ajuste `VERTEX_PROJECT_ID`. Os modelos Gemini 3.x ficam no endpoint `global`.

Os modelos não são determinísticos: o ataque do Cenário 3 depende do modelo e do fraseado do pedido, e pode não se repetir a cada execução. É justamente por isso que o controle fica no código, e não no prompt. O Cenário 5 testa a política sem depender do modelo.

## Referências

**Segurança de agentes**
- Simon Willison, [The lethal trifecta for AI agents](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/) (jun/2025)
- OWASP GenAI Security Project, [Top 10 for Agentic Applications](https://genai.owasp.org/) (dez/2025)
- Invariant Labs, [GitHub MCP exploited: accessing private repositories via MCP](https://invariantlabs.ai/blog/mcp-github-vulnerability) (mai/2025), prova de conceito de pesquisadores
- Koi Security via The Hacker News, [First malicious MCP server found](https://thehackernews.com/2025/09/first-malicious-mcp-server-found.html) (set/2025), caso `postmark-mcp`
- Fortune, [AI coding tool Replit wiped database, called it a "catastrophic failure"](https://fortune.com/2025/07/23/ai-coding-tool-replit-wiped-database-called-it-a-catastrophic-failure) (jul/2025), relato do cliente SaaStr
- Gartner, press release de 25/06/2025: "Over 40% of agentic AI projects will be canceled by end of 2027"

**Protocolos e frameworks**
- Model Context Protocol, [The 2026-07-28 Specification](https://blog.modelcontextprotocol.io/posts/2026-07-28/)
- A2A Protocol, [A New Chapter for A2A: Joining the Agentic AI Foundation](https://a2a-protocol.org/latest/blog/2026/08/27/a-new-chapter-for-a2a-joining-the-agentic-ai-foundation/) (ago/2026)
- Google, [Agent Development Kit 2.0](https://adk.dev/2.0/) e [documentação](https://adk.dev)
- Google Cloud, [Gemini Enterprise Agent Platform: release notes](https://docs.cloud.google.com/gemini-enterprise-agent-platform/release-notes)
- Microsoft, [Inside Microsoft Agent Framework 1.0](https://techcommunity.microsoft.com/blog/azuredevcommunityblog/the-future-of-agentic-ai-inside-microsoft-agent-framework-1-0/4510698)
- Google AI for Developers, [Gemini API pricing](https://ai.google.dev/gemini-api/docs/pricing)

Os nomes de produtos e as versões refletem o estado em **16/10/2026** e mudam rápido.

## Licença e atribuição

O uso é livre, **desde que o trabalho original seja citado**.

| O quê | Licença | Como citar |
|---|---|---|
| Código (`demo/`) | [Apache-2.0](LICENSE) | manter o arquivo [NOTICE](NOTICE) e os avisos de copyright nas cópias e obras derivadas |
| Slides e textos (PDF, README, notebook em prosa) | [CC BY 4.0](LICENSE-CONTEUDO) | dar crédito, indicar se houve alterações e linkar para este repositório |

Sugestão de citação: *Ahirton Lopes, "AI Agents na Vida Real: Arquiteturas, Segurança e Por Onde Começar", 23º Latinoware, 2026. https://github.com/ahirtonlopes/latinoware-2026*

O case da Cooperativa Grão Tríplice e todos os dados são fictícios.

---

**Ahirton Lopes, PhD** · [github.com/ahirtonlopes](https://github.com/ahirtonlopes)
