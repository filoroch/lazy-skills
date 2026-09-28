# .NET Architecture & Conventions

Use iloroch/dotnet-initialr como referência principal para a estrutura e convenções do backend.

- A organização deve ser feita por contexto, Domain, Application, Infrastructure.
- Respeite os projetos/aplicações definidos pelo template: API, Jobs, Workers e MCP (quando aplicável).
- Respeite as convenções existentes no repositório base para:
  - Observabilidade.
  - Tratamento de erros.
  - Mecanismos de scaffolding (caso existam templates ou scripts no próprio repositório).

### Autenticação
A autenticação faz parte do scaffold base fullstack. A implementação deve seguir o padrão existente no template .NET e a integração Angular correspondente, sem criar uma arquitetura paralela.
