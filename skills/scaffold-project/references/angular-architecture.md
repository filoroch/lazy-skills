# Angular Architecture & Conventions

Template base: **`github.com/filoroch/angular-initialr`** (Angular 20, Standalone + Signals).
Resolva o diretório conforme o Passo 0 da SKILL.md (`DIR_FRONTEND` — cache em
`~/.cache/lazy-skills/templates/angular-initialr`).

### Estrutura base (como o template realmente entrega)
`
src/app/
├── core/            # singletons — nunca importe componentes daqui
│   ├── models/ (requests/responses, ex.: auth.model.ts)
│   ├── services/    # auth.service.ts, api.service.ts, token-storage
│   ├── guards/      # admin.guard.ts
│   └── interceptors/ # admin.interceptor.ts, error.interceptor.ts
├── shared/
│   ├── components/
│   ├── directives/
│   └── pipes/
├── layouts/         # admin-layout (Sidebar/Header/Content), public-layout
├── features/
│   ├── public/      # pages/ (login, home, not-found) + public.routes.ts
│   └── admin/       # pages/ + admin.routes.ts (+ components/ só desta feature)
├── app.routes.ts    # compõe publicRoutes + adminRoutes (lazy)
└── app.config.ts    # provideRouter + provideHttpClient(withInterceptors([...]))
`

- Contextos são `features/public` e `features/admin` (não pastas de topo `public/`/`admin/`).
- Rotas sempre lazy: `loadComponent`/`loadChildren`. Guard de contexto no rota pai
  (`canMatch`/`canActivate`) + `canActivateChild` interno.
- Guards e interceptors específicos de contexto ficam em `core/` se globais; infra nunca em features.

### Presets de UI (mecanismo do template)
- `node scripts/use-preset.mjs spartan-tailwind` → Tailwind CSS + Spartan NG (`@spartan-ng/brain`).
- `node scripts/use-preset.mjs keen-bootstrap` → Bootstrap 5 + slots Keen (default do template).
- O preset ajusta `package.json`, `angular.json`, `src/styles.*` (diretivas `@tailwind`), copia
  `tailwind.config.js`/`postcss.config.js` e grava `.preset-active`.
- Após qualquer troca de preset: `pnpm install`.

### Convenções
- Angular 20 com Standalone components; Angular Signals como padrão de reatividade.
- Tailwind CSS como base de estilização; Spartan NG como biblioteca de UI.
- Package manager: **pnpm** (o template nasce com npm — converta com `pnpm install`).
- Models inicialmente em `core/models`.
- HTTP usa `provideHttpClient()`; interceptor de auth registrado via `withInterceptors` no `app.config.ts`.
- Código da aplicação importa o environment pelo caminho padrão (`@environments/environment`).
  O build decide qual arquivo será utilizado.
- Não criar testes unitários no scaffold inicial (E2E será feito via skill e2e-testing).
- Prefira usar `pnpm exec ng generate` para geração incremental quando apropriado
  (`ng` não está instalado globalmente).
