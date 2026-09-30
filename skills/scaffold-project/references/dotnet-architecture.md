# .NET Architecture & Conventions

Use o template do repositório **`github.com/filoroch/dotnet-initialr`** (template
`filoroch-scaffold-solution`) como referência principal para a estrutura e convenções do backend.
.NET 10 (`net10.0`). Resolva o diretório conforme o Passo 0 da SKILL.md (`DIR_BACKEND` — cache em
`~/.cache/lazy-skills/templates/dotnet-initialr`).

### Scaffold

```bash
dotnet new install "$DIR_BACKEND"
dotnet new filoroch-scaffold-solution --name <Raiz> --output ./backend --provider efcore --driver postgresql
```

- A organização deve ser feita por contexto: Domain, Application, Infrastructure (CrossCutting e IoC
  conforme o template).
- Respeite os projetos/aplicações definidos pelo template: API, Jobs, Workers e MCP (quando aplicável).
- Respeite as convenções existentes no repositório base para:
  - Observabilidade (Serilog + OpenTelemetry).
  - Tratamento de erros (exceções de domínio no CrossCutting + handler global).
  - Mecanismos de scaffolding (caso existam templates ou scripts no próprio repositório).

### Autenticação
A autenticação faz parte do scaffold base fullstack. A implementação deve seguir o padrão existente
no template .NET e a integração Angular correspondente, sem criar uma arquitetura paralela.

Para projetos multi-tenant (padrão ministry), o scaffold de auth inclui:

- **Identity**: `MinistryDbContext : IdentityDbContext<ApplicationUser, ApplicationRole, Guid>`;
  `ApplicationUser` com `IgrejaId`; roles `ADMIN_GLOBAL, PASTOR, SECRETARIA, LIDER_DEPARTAMENTO,
  LIDER_NUCLEO, COMUNICACAO_MIDIA, MEMBRO`.
- **JWT**: `ITokenService`/`IJwtTokenService` emitindo claims `sub`, `email`, `role`, `igreja_id`.
  `[Authorize]` → 401 não autenticado.
- **Multi-tenancy (dupla trava)**:
  - `ITenantEntity { Guid IgrejaId }` + `.HasQueryFilter()` dinâmico no `DbContext` — avaliar o tenant
    **por consulta** (membro da instância do contexto), nunca como `Expression.Constant` (o modelo EF
    é cacheado e o filtro congelaria); bypass apenas para `ADMIN_GLOBAL`.
  - `ITenantProvider` (claim `igreja_id` ou header `X-Igreja-Id`) + `TenantDbCommandInterceptor`
    injetando `SET LOCAL app.current_igreja_id = @param` (SQL parametrizado, sem interpolação).
- **Seed**: auto-seed no startup apenas em banco vazio, com `IgnoreQueryFilters()` na verificação.
- **Login**: `POST /api/auth/login` via `SignInManager`.

### Segredos e persistência
- Connection string e `Jwt:SigningKey` via `dotnet user-secrets` local / env vars em CI —
  **nunca commitados**.
- Migrações: `dotnet ef database update --project <Infra> --startup-project <Api>`; UUIDs com
  default `gen_random_uuid()`.
- Verificação obrigatória: `dotnet build` com **0 erros** + `dotnet test` verdes.
