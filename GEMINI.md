# Guia do Projeto FastMCP

## Visão Geral do Projeto
FastMCP é um framework Python abrangente (Python ≥3.10) para construir servidores e clientes [Model Context Protocol (MCP)](https://modelcontextprotocol.io/). Ele fornece uma API Pythonic de alto nível para expor ferramentas, recursos e prompts para LLMs, enquanto gerencia as complexidades subjacentes do protocolo, negociação de transporte e validação.

### Pilares Principais
- **Servidores:** Transformam funções Python em ferramentas, recursos e prompts compatíveis com MCP.
- **Aplicativos:** Fornecem interfaces de usuário interativas renderizadas diretamente nas conversas do LLM.
- **Clientes:** Conectam a qualquer servidor MCP (local ou remoto, programático ou via CLI).

## Principais Tecnologias
- **Linguagem:** Python 3.10+
- **Gerenciamento de Pacotes:** [uv](https://docs.astral.sh/uv/)
- **Build Backend:** [Hatchling](https://hatch.pypa.io/)
- **Linting e Formatação:** [Ruff](https://beta.ruff.rs/docs/), [Prettier](https://prettier.io/)
- **Verificação de Tipos:** [ty](https://github.com/jakekaplan/ty)
- **Testes:** [Pytest](https://docs.pytest.org/)
- **Análise Estática:** [prek](https://github.com/jakekaplan/prek)
- **Controle de Tamanho de Arquivos:** [loq](https://github.com/jakekaplan/loq)
- **Documentação:** [Mintlify](https://mintlify.com/)

## Fluxo de Desenvolvimento

### Sequência Obrigatória
Antes de realizar um commit ou abrir um PR, sempre execute estes comandos em sequência:
1. `uv sync` - Instala e sincroniza as dependências.
2. `uv run pytest -n auto` - Executa toda a suíte de testes em paralelo.
3. `uv run prek run --all-files` - Executa todas as verificações estáticas (Ruff, Prettier, ty).

### Comandos Comuns (via `just`)
- `just build`: Executa `uv sync`.
- `just test`: Compila e executa `pytest -xvs tests`.
- `just typecheck`: Executa `uv run ty check`.
- `just docs`: Serve a documentação localmente usando Mintlify.
- `just api-ref-all`: Gera a documentação de referência da API.

## Estrutura do Repositório
- `fastmcp_slim/fastmcp/`: Código-fonte da biblioteca principal.
    - `server/`: Implementação do servidor, autenticação e middleware.
    - `client/`: SDK do cliente e autenticação.
    - `tools/`, `resources/`, `prompts/`: Definições dos objetos principais do MCP.
    - `cli/`: Comandos da CLI.
    - `utilities/`: Utilitários compartilhados (incluindo `FastMCPComponent`).
- `tests/`: Suíte do Pytest (testes unitários e de integração).
- `docs/`: Código-fonte da documentação (framework Mintlify).
- `examples/`: Exemplos de uso e demonstrações.

## Convenções de Desenvolvimento

### Regras Gerais
- **Estilo Python:** Use anotações de tipo completas. Priorize a clareza e a facilidade de leitura ao invés de código "esperto" ou complexo.
- **Atribuição de Agente:** Se você é um agente de IA, você DEVE se identificar nos commits e PRs (ex: "🤖 Generated with Claude Code").
- **Verificação de Identidade:** Use a propriedade `.key` do `FastMCPComponent` para identidade canônica. Não use fallbacks ad-hoc de nome ou URI para verificações de identidade.
- **Docstrings:** Use Markdown para docstrings (não RST). As docstrings do FastMCP são automaticamente compiladas em MDX.
- **Tamanho dos Arquivos:** Imposto pelo `loq`. Se um arquivo exceder os limites, pode ser necessário refatorá-lo ou atualizar o `loq.toml` com a aprovação de um mantenedor.
- **Exportações de Módulo:** Evite importações excessivas no `__init__.py`. Reexporte apenas os tipos fundamentais para o namespace de nível superior `fastmcp.*`.

### Testes
- Todos os novos recursos e correções de bugs DEVEM incluir testes.
- Os testes em `tests/integration_tests` são marcados automaticamente com `integration`.
- O tempo limite padrão (timeout) para os testes é de 5 segundos.
- Use fixtures do arquivo `tests/conftest.py` (ex: `fastmcp_server`, `tool_server`) para ambientes de teste consistentes.

### Documentação
- Os recursos não existem a menos que estejam documentados em `docs/`.
- Novos arquivos devem ser adicionados ao `docs/docs.json`.
- Ao atualizar configurações em `settings.py`, atualize também `docs/more/settings.mdx`.
- Evite modificar manualmente arquivos gerados automaticamente em `docs/python-sdk/` ou `docs/public/schemas/`.

### Releases
- Apenas crie releases quando explicitamente solicitado por um mantenedor.
- **Crítico:** Os títulos de release DEVEM seguir o formato `v<versão>: <trocadilho>`, onde o trocadilho ("pun") se relaciona com o tema da release.
- Formato da tag: `v<versão>` (ex: `v3.2.0`).
- Use `gh release create ... --generate-notes`.

### Diretrizes de PR
- As mensagens do PR devem se concentrar no "por que" e incluir um exemplo claro de código.
- Evite listas exaustivas de alterações ou linguagem de marketing verbosa.
- Sempre leia os comentários do bot de revisão (CodeRabbit, etc.) antes de aprovar ou finalizar um PR.
- Respeite as marcações `[DNM]` ou `[DRAFT]` nos títulos/labels dos PRs.