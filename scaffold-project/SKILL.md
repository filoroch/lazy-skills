## Objetivo

Padronizar a criação e evolução dos projetos fullstack do Filipe, mantendo Angular e .NET sob uma única skill orquestradora, mas com convenções separadas por tecnologia.

## Princípio arquitetural

A skill principal é `project-scaffold`. Ela coordena frontend e backend sem transformar cada lado em uma skill independente.

- Frontend: Angular 19+ com standalone components.
- Backend: .NET conforme o padrão do repositório `filoroch/dotnet-initialr`.
- Autenticação: faz parte do scaffold base.
- E2E: responsabilidade de uma skill separada com Playwright.
- Preferir templates e scripts determinísticos.
- Não adicionar abstrações, pastas ou tecnologias que não estejam definidas pelo padrão.

## Angular

### Estrutura base

```
src/app/
├── core/
│   ├── models/
│   │   ├── requests/
│   │   └── responses/
│   ├── services/
│   ├── guards/
│   └── interceptors/
├── shared/
│   ├── components/
│   ├── directives/
│   └── pipes/
├── public/
│   ├── components/
│   ├── pages/
│   └── public.routes.ts
├── admin/
│   ├── components/
│   ├── pages/
│   └── admin.routes.ts
└── app.routes.ts
```

`public` e `admin` são contextos padrão. Outros contextos podem ser adicionados incrementalmente.

Cada contexto possui sua própria configuração de rotas. `app.routes.ts` compõe as rotas dos contextos.

Guards e interceptors específicos de contexto são opcionais e só devem ser criados quando necessários. Infraestrutura global permanece em `core`.

### Convenções

- Angular 19+.
- Standalone components.
- Angular Signals como padrão de reatividade.
- Tailwind CSS como base de estilização.
- Spartan NG como biblioteca de UI.
- Models inicialmente em `core/models`.
- Environments `development` e `production`.
- Código da aplicação importa o environment pelo caminho padrão, sem selecionar manualmente o ambiente no import.
- HTTP usa `provideHttpClient()`.
- Não criar testes unitários no scaffold inicial.
- E2E será adicionado por skill própria de Playwright.
- Preferir `ng generate` para geração incremental quando apropriado.

### Environment

Manter uma convenção de import estável para a aplicação. O build/configuração do Angular decide qual arquivo de environment será utilizado conforme o ambiente, permitindo que o código não precise importar `environment.development` ou outro arquivo específico.

## .NET

Usar `filoroch/dotnet-initialr` como referência principal para a estrutura e convenções do backend, incluindo organização por contexto, Domain, Application, Infrastructure e os projetos/aplicações definidos pelo template, como API, Jobs, Workers e MCP quando aplicável.

Respeitar as convenções existentes no `dotnet-initialr`, incluindo observabilidade, tratamento de erros e os mecanismos de scaffolding existentes no repositório.

## Autenticação

A autenticação faz parte do scaffold base fullstack. A implementação deve seguir o padrão existente no template .NET e a integração Angular correspondente, sem criar uma arquitetura paralela.

## Operações do scaffold

### Projeto

- Criar projeto fullstack.
- Criar somente backend.
- Criar somente frontend.
- Inicializar autenticação.

### Backend

- Criar contexto.
- Criar entidade.
- Criar repository.
- Criar command.
- Criar query.
- Criar request/response.
- Criar endpoint.
- Criar service.
- Criar CRUD.

### Frontend

- Criar contexto.
- Criar página.
- Criar componente.
- Criar service.
- Criar model.
- Criar rota.
- Criar guard.
- Criar interceptor.
- Criar CRUD.

### Fullstack

Operações de alto nível podem compor operações de frontend e backend. Por exemplo, `criar CRUD Product` deve gerar as peças necessárias nos dois lados respeitando as convenções de cada tecnologia.

## Regras de comportamento

1. Antes de modificar um projeto existente, detectar a estrutura e as convenções já presentes.
2. Preferir reutilizar o scaffold existente e seus scripts/templates.
3. Usar `ng generate` para operações Angular quando apropriado.
4. Usar os mecanismos de scaffold do `dotnet-initialr` para o backend quando aplicável.
5. Não criar pastas opcionais vazias.
6. Não criar testes E2E automaticamente.
7. Não criar novas abstrações arquiteturais sem necessidade ou correspondência no padrão.
8. Após uma operação incremental, validar imports, referências de projeto, rotas e estrutura resultante.
9. Em operações fullstack, manter os contratos entre API e frontend coerentes.
10. Se uma convenção ainda não estiver definida, não inventá-la silenciosamente quando ela impactar a estrutura do scaffold.

## Evolução futura

Novas responsabilidades grandes, como E2E/Playwright, deploy ou observabilidade especializada, devem preferencialmente virar skills independentes.