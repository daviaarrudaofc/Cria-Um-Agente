# Agente de terminal com LangChain

Este projeto cria um agente para automatizar tarefas pelo terminal. Ele consegue:

- consultar um banco SQLite local;
- buscar memoria em documentos enviados;
- criar documentos `.docx`;
- usar ferramentas via LangChain.

## Por que essa base

Para comecar, esta base usa LangChain com um provedor configuravel. O padrao do `.env.example` esta como xAI/Grok porque a API e compativel com OpenAI e funciona bem para um agente de terminal.

Voce tambem pode usar OpenAI trocando `MODEL_PROVIDER=openai` e preenchendo `OPENAI_API_KEY`.

## Instalar

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
```

Depois edite `.env` e coloque sua `XAI_API_KEY`, ou configure OpenAI se preferir.

## Rodar

```powershell
python agent.py
```

Comandos dentro do agente:

- `/ingest` indexa arquivos colocados em `data/inbox`;
- `/sair` encerra;
- qualquer outra mensagem vai para o agente.

## Documentos enviados

Coloque arquivos em `data/inbox`. A versao inicial suporta `.txt`, `.md`, `.pdf`, `.docx` e `.csv`.

Com `DOCUMENT_MEMORY_MODE=keyword`, o agente pesquisa localmente nos arquivos da pasta, sem gastar tokens de embedding. Nesse modo, `/ingest` apenas confere os documentos encontrados.

Se quiser memoria vetorial depois, use `DOCUMENT_MEMORY_MODE=vector`, preencha `OPENAI_API_KEY` e rode `/ingest`. A memoria vetorial fica em `data/vectorstore`.

## Usar Grok/xAI

No `.env`:

```env
MODEL_PROVIDER=xai
XAI_API_KEY=sua_chave_aqui
XAI_BASE_URL=https://api.x.ai/v1
XAI_MODEL=grok-4.3
DOCUMENT_MEMORY_MODE=keyword
```

A disponibilidade de creditos gratis e limites depende da sua conta na xAI. Quando os creditos acabarem, as chamadas do agente vao falhar ate a cota resetar ou voce adicionar credito.

## Banco de dados

Por padrao o projeto cria `data/agent.db` com tabelas de exemplo. Para trocar por outro banco depois, podemos substituir as ferramentas SQLite por SQLAlchemy ou pelo toolkit SQL do LangChain.
