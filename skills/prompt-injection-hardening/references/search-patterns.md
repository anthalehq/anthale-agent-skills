# Search Patterns

Use `rg` first. It is the fastest way to locate prompts, tools, retrieval, memory, and dangerous sinks.

These patterns are intentionally polyglot. They should work across TypeScript and Python codebases. For Python-heavy repos, use the Python-focused narrowing patterns near the end before reading files blindly.

## Prompt Construction

```bash
rg -n --glob '!node_modules' --glob '!.next' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' --glob '!site-packages' '(system|developer|instructions|system_prompt|prompt|PromptTemplate|ChatPromptTemplate|messages|SystemMessage|HumanMessage|AIMessage|chat\.completions|responses\.create|guard_openai_client|guard_chat_model|AnthaleLangchainMiddleware)'
```

## Tools and Dangerous Execution

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' --glob '!site-packages' '(tool|tool_call|bind_tools|function|schema|execute|exec|spawn|subprocess|Popen|os\.system|eval|shell|bash|sql|query|cursor\.execute|sqlite3|sqlalchemy|psycopg)'
```

## Controls and Approval Gates

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' --glob '!site-packages' '(authz|authorize|authorization|permission|allowlist|approval|guard|validator|validate|model_validate|BaseModel|pydantic|policy|policy_id|policyId|scope)'
```

## Retrieval and Ingestion

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' --glob '!site-packages' '(retriev|as_retriever|vector|vectorstore|embed|chunk|rerank|context|loader|PyPDF|BeautifulSoup|ingest|corpus|faiss|chroma|pinecone|qdrant)'
```

## Memory and Summaries

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' --glob '!site-packages' '(memory|ConversationBufferMemory|summary|summari|scratchpad|profile|episodic|checkpoint|store|state graph|StateGraph|history compression)'
```

## MCP and Dynamic Tool Metadata

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' --glob '!site-packages' '(mcp|FastMCP|tool description|server metadata|dynamic tool|tool registry|capabilities|server instructions)'
```

## Browser, Fetch, and External Content

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' --glob '!site-packages' '(fetch|axios|httpx|aiohttp|requests|scrape|playwright|selenium|browser|webhook|email|html|markdown)'
```

## What to Do Next

- Read the highest-signal matches first.
- Follow the flow from untrusted source to transform to sink.
- Once you identify a framework, load the matching `framework-*.md` reference or `attack-surfaces.md` instead of continuing blind repo-wide reads.
