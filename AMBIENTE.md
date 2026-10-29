# AMBIENTE.md — Configuração de Desenvolvimento (RACKOONA-W32R)

> Gerado em 29/10/2026 pela arqueologia + configuração do sistema. Fonte de verdade do ambiente.
> Espelho em notas: `C:\CodexProjetos\CodexVault\Ambiente de Desenvolvimento.md`.

## 1. Arqueologia do sistema (estado encontrado)

| Campo | Valor |
|---|---|
| SO | Windows Server 2022 Standard, build 20348, VM QEMU/KVM |
| Instalação | 27/10/2026 (sistema novo, 2 dias antes desta configuração) |
| CPU / RAM / Disco | AMD64 ~2GHz / 64 GB / 360 GB (331 GB livres) |
| Locale | pt-BR, UTC-3 Brasília |
| Já existia | Chocolatey 2.7.4, Git 2.56 + git-lfs, PowerShell 7.6.9, 7-Zip, Notepad++, WinRAR, Chrome/Edge/Firefox ESR, FileZilla, ownCloud, ZCode 3.16.3, OpenAI Codex CLI |
| Inexistente | Python, Node.js, npm, gh, Obsidian, qualquer gerenciador de versão; winget (normal em Server); `LongPathsEnabled=0`; git sem identidade global |

## 2. O que foi instalado e configurado

| Ferramenta | Versão | Caminho | Como |
|---|---|---|---|
| Python | 3.15.2 | `C:\Python315` | `choco install python3` |
| pip | 26.2.2 | `C:\Python315\Scripts` | junto ao Python |
| uv | 0.13.6 | `C:\Python315\Scripts\uv.exe` | `pip install uv` |
| Node.js LTS | 26.13.0 | `C:\Program Files\nodejs` | `choco install nodejs-lts` |
| npm | 11.20.0 | prefix `%APPDATA%\npm` | junto ao Node |
| GitHub CLI | 2.103.3 | `C:\Program Files\GitHub CLI` | `choco install gh` |
| Obsidian | 1.14.5 | `%LOCALAPPDATA%\Programs\Obsidian` | `choco install obsidian` |
| Graphify | 0.9.97 (+`graphify-mcp`) | `C:\Users\Administrator\.local\bin` | `uv tool install --python 3.13 graphifyy` |
| LongPaths | habilitado (=1) | `HKLM:\...\FileSystem` | registro |
| Identidade git | ✅ configurada | `~/.gitconfig` | `Douglas Penada Arruda <douglaspenadearruda97@gmail.com>` + `init.defaultBranch=main` |

- **Python 3.13 gerenciado pelo uv** existe apenas dentro do ambiente do Graphify (não polui o sistema).
- **Vault Obsidian** criado e registrado: `C:\CodexProjetos\CodexVault` (5 notas linkadas + grafo).
- **Grafo do vault**: `CodexVault\graphify-out\` → `graph.json`, `graph.html` (interativo), `GRAPH_REPORT.md` (25 nós / 27 arestas / 6 comunidades).
- PATH do sistema atualizado (choco + instaladores + `uv tool update-shell`): **reabra shells** para `python`, `node`, `graphify` funcionarem sem caminho absoluto.

## 3. Decisões e porquês

1. **Graphify em Python 3.13 isolado (uv), não no 3.15 global** — `tree-sitter<0.26` e `pyyaml`
   não têm wheels para 3.15 (recém-lançado); a instalação direta quebrou na compilação.
   Ambiente isolado = global intacto + ferramenta funcionando.
2. **Pacote PyPI correto é `graphifyy` (yy)** — `graphify` não existe no PyPI.
3. **Tudo via Chocolatey** quando possível — um dono por ferramenta, upgrade/remoção centralizados.
4. **LongPaths antes do primeiro `npm install`** — prevenção de erro de path >260 chars.
5. **Corepack não existe no Node 26** — removido da distribuição oficial. Se precisar de
   pnpm/yarn: `npm install -g pnpm` (não `corepack enable`).

## 4. Uso rápido

```powershell
# Python com uv (recomendado para projetos)
cd projeto; uv venv; uv pip install -r requirements.txt; uv run main.py

# Node
npm init -y; npm install

# Grafo de conhecimento de um projeto
cd C:\CodexProjetos\projeto
graphify update .          # constrói/reconstrói graphify-out/graph.json (sem LLM)
graphify explain "main.py" # explica nó e vizinhos
start graphify-out/graph.html   # grafo interativo no navegador

# Obsidian: start obsidian://open?vault=CodexVault (vault já registrado; Ctrl+G = graph view)
```

## 5. Pendências — críticas ZERO ✅ (29/10/2026)

Restam apenas opcionais (dono: usuário):
1. VS Code (`choco install vscode -y`) — já há ZCode + Notepad++.
2. `graphify install --platform codex` — skill do Graphify no Codex CLI.
3. `GEMINI_API_KEY`/`GOOGLE_API_KEY` — extração semântica (LLM) do Graphify.
4. Fechar issue de teste #1 e definir descrição do repo `master` — cosmético.
5. Decisão de visibilidade do repo `master` (hoje público; conteúdo sem segredos).

## 6. Repos GitHub (dono: douglaspenadearruda97)

| Repo | Visibilidade | Papel |
|---|---|---|
| [`master`](https://github.com/douglaspenadearruda97/master) | público | Config/documentação do servidor (este AMBIENTE.md) |
| [`codex-vault`](https://github.com/douglaspenadearruda97/codex-vault) | privado | Vault Obsidian: notas + grafos de conhecimento |
| [`projeto1`](https://github.com/douglaspenadearruda97/projeto1) | privado | Primeiro projeto real (Python 3.13 via uv, calculadora, grafo de código) |
