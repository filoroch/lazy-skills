---
name: scaffold-project
description: >-
  Use this skill when scaffolding new fullstack projects, or adding new features/CRUDs 
  to existing Angular (.NET backend) applications following the established architecture.
---

# Scaffold Project

## Objetivo
Padronizar a criação e evolução dos projetos fullstack do Filipe, mantendo Angular e .NET sob uma única skill orquestradora.

## Templates (fonte de verdade: GitHub)

Os repositórios no **GitHub** são a fonte final de verdade sobre os templates. Paths locais são
apenas cache de trabalho — nunca a referência.

| Template | Repositório | Entrega |
|---|---|---|
| Backend | `github.com/filoroch/dotnet-initialr` | template dotnet `filoroch-scaffold-solution` |
| Frontend | `github.com/filoroch/angular-initialr` | Angular standalone + presets |

- Raiz de cache: `TEMPLATE_ROOT=${LAZY_SKILLS_TEMPLATES_DIR:-$HOME/.cache/lazy-skills/templates}`
- Pastas usadas nos comandos abaixo: `DIR_BACKEND` e `DIR_FRONTEND` (resultado da resolução).

### Passo 0 — Resolução do template (rode antes de qualquer scaffold)

1. Se `LAZY_SKILLS_TEMPLATES_DIR` estiver definido, use `<dir>/dotnet-initialr` e
   `<dir>/angular-initialr` direto, sem rede.
2. Se `$TEMPLATE_ROOT/<nome>` já existir como clone git → `git -C <dir> pull --ff-only` e use.
3. Se `~/Repositories/templates/<nome>` existir como clone git, **limpo** e igual a `origin/main`
   (confira após `git fetch`) → reutilize-o, evitando rebaixar o que já está na máquina.
4. Caso contrário clone do GitHub: `gh repo clone filoroch/<nome> $TEMPLATE_ROOT/<nome>`.
5. Se o clone falhar (repo inexistente, sem acesso ou offline) → use `~/Repositories/templates/<nome>`
   e **avise o usuário** de que a cópia local não veio da fonte de verdade.
6. Working tree suja ou commits locais não pushados **não** são fonte de verdade: use o clone do
   cache e informe o usuário antes de continuar.

## Passos para Execução do Scaffold

1. **Resolva os templates** conforme o Passo 0 acima; mantenha `DIR_BACKEND`/`DIR_FRONTEND` em
   vista para os passos seguintes.
2. **Leia as Diretrizes de Arquitetura**:
   - Para Frontend: Consulte [angular-architecture.md](./references/angular-architecture.md).
   - Para Backend: Consulte [dotnet-architecture.md](./references/dotnet-architecture.md).
3. **Analise o projeto existente**: Detecte a estrutura e as convenções já presentes antes de adicionar arquivos.
4. **Reutilize os templates, na ordem de preferência**:
   - **Backend**: use o template engine — `dotnet new install "$DIR_BACKEND"` e
     `dotnet new filoroch-scaffold-solution --name <Raiz> --provider efcore --driver postgresql`.
     O template engine faz o rename automático de namespaces/projetos; **evite cópia manual de `src/`**.
     - *Contorno (known-issue)*: se o install falhar com `MV012 ... 'template-assets/efcore' ... não existe`,
     o `template.json` do template tem source fantasma — corrija o `template.json` (remova a source inexistente)
     ou copie manualmente seguindo o checklist abaixo.
   - **Checklist de cópia manual** (obrigatório, senão sobram restos do template):
     - excluir `bin/`, `obj/`, `node_modules/`, `dist/` antes de copiar;
     - renomear o prefixo do template (`Filoroch.Template` → nome do projeto) em `.sln`, `.csproj`,
       namespaces **e nos `Dockerfile` de cada App** (são esquecidos com facilidade);
     - substituir placeholders `__TEMPLATE_*__` no `appsettings.json` (provider/driver).
   - **Frontend**: copie `$DIR_FRONTEND` sem `node_modules`/`dist`, rode
     `node scripts/init-template.mjs` (nome/prefixo) e aplique o preset de UI
     (`node scripts/use-preset.mjs spartan-tailwind` para Tailwind + Spartan NG; `keen-bootstrap` é
     o default). Depois `pnpm install` (pnpm, não npm). Clone git já vem limpo — o checklist de
     limpeza só é obrigatório no fallback local.
   - **Geração incremental**: `pnpm exec ng generate` no frontend (`ng` não existe globalmente);
     no backend, `dotnet script "$DIR_BACKEND/tools/scaffold/main.csx" -- --entity X --context Y`.
5. **Execute a operação solicitada**, que pode ser:
   - **Fullstack**: Criar CRUD completo coordenando frontend e backend (mantenha os contratos coerentes).
   - **Frontend**: Criar contexto, página, componente, service, model, rota, guard, interceptor.
   - **Backend**: Criar contexto, entidade, repository, command, query, request/response, endpoint, service.
6. **Valide com gates executáveis** (não só inspeção visual):
   - Backend: `dotnet build <sln>` com **0 erros** e `dotnet test` verdes.
   - Frontend: `pnpm exec ng build` (ou `pnpm build`) sem erros.
   - Antes de concluir, procure sobras do template:
     `grep -r "Filoroch\|__TEMPLATE_" --include="*.cs" --include="*.csproj" --include="*.sln" --include="Dockerfile" --include="*.json" <dir>`
     — resultado deve ser vazio.

> **Regras Restritas**:
> - Não adicione abstrações, pastas ou tecnologias que não estejam definidas no padrão referenciado.
> - Não crie pastas opcionais vazias.
> - Não crie testes E2E automaticamente (use a skill e2e-testing).
> - Se uma convenção não estiver definida, pergunte; não a invente silenciosamente.
> - Segredos (connection string Neon, JWT signing key) vão em `dotnet user-secrets` ou env — nunca commitados.
> - O GitHub é a fonte de verdade dos templates: alteração em `dotnet-initialr`/`angular-initialr`
>   só vale depois de commitada e pushada no repo correspondente. Nunca dependa de uma mudança que
>   exista só na sua máquina — se houver diferença local, informe o usuário.
