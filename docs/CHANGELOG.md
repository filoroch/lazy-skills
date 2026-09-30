# Changelog

## [Unreleased]

## [0.1.0] - 2026-09-30

### Changed
- **scaffold-project**: GitHub passou a ser a fonte final de verdade sobre os templates.
  - Nova seção **Templates (fonte de verdade: GitHub)** com os repositórios
    `filoroch/dotnet-initialr` (backend) e `filoroch/angular-initialr` (frontend); os paths de
    `~/Repositories/templates/...` deixaram de ser a referência e viraram apenas cache local.
  - Novo **Passo 0 — Resolução do template**: `LAZY_SKILLS_TEMPLATES_DIR` → cache
    `~/.cache/lazy-skills/templates` (`git pull --ff-only`) → reuso do clone local limpo e
    sincronizado → `gh repo clone` → fallback local **com aviso ao usuário**.
  - Comandos do scaffold agora usam os diretórios resolvidos (`$DIR_BACKEND`, `$DIR_FRONTEND`):
    `dotnet new install`, cópia do template Angular, `init-template.mjs`/`use-preset.mjs` e
    `dotnet script tools/scaffold/main.csx`.
  - Regra restritiva: alteração em template só vale depois de commitada e pushada no repo
    correspondente; divergência local deve ser informada ao usuário.
- **scaffold-project**: validação do scaffold trocada por gates executáveis (`dotnet build` com
  0 erros, `dotnet test`, `pnpm exec ng build`) + varredura de sobras do template
  (`Filoroch`/`__TEMPLATE_`).

### Added
- **aspnet-testing**: skill para executar e corrigir testes unitários ASP.NET, validar endpoints
  relevantes com curl e relatar os resultados.
- **code-review**: arquivo `references/principles.md` com as regras detalhadas de arquitetura,
  DRY, KISS e responsabilidades.
- **e2e-testing**: arquivo `references/playwright-guidelines.md` com as estratégias e regras de
  cobertura para Playwright.
- **scaffold-project**: arquivos `references/angular-architecture.md` e `references/dotnet-architecture.md`
  detalhando a infraestrutura e convenções.
- **scaffold-project**: documentação do scaffold de autenticação multi-tenant (Identity, JWT,
  dupla trava de multi-tenancy, seed e login) e das regras de segredos/persistência.
- **AGENTS.md**: regra global de Changelog Maintenance para que o agente mantenha
  `docs/CHANGELOG.md` atualizado em todas as iterações.
- **scaffold-project**: checklist de cópia manual do template (exclusão de `bin/`/`obj/`/
  `node_modules/`/`dist/`, rename do prefixo em `.sln`/`.csproj`/namespaces/`Dockerfile` e
  substituição dos placeholders `__TEMPLATE_*__`).
