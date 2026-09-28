# Angular Architecture & Conventions

### Estrutura base
`
src/app/
├── core/
│   ├── models/ (requests/responses)
│   ├── services/
│   ├── guards/
│   └── interceptors/
├── shared/
│   ├── components/
│   ├── directives/
│   └── pipes/
├── public/ (contexto padrão)
│   ├── components/
│   ├── pages/
│   └── public.routes.ts
├── admin/ (contexto padrão)
│   ├── components/
│   ├── pages/
│   └── admin.routes.ts
└── app.routes.ts
`

- Cada contexto possui sua própria configuração de rotas compostas em pp.routes.ts.
- Guards e interceptors específicos de contexto são opcionais. Infra global permanece em core.

### Convenções
- Angular 19+ com Standalone components.
- Angular Signals como padrão de reatividade.
- Tailwind CSS como base de estilização.
- Spartan NG como biblioteca de UI.
- Models inicialmente em core/models.
- HTTP usa provideHttpClient().
- Código da aplicação importa o environment pelo caminho padrão. O build decide qual arquivo será utilizado.
- Não criar testes unitários no scaffold inicial (E2E será feito via skill Playwright).
- Prefira usar 
g generate para geração incremental quando apropriado.
