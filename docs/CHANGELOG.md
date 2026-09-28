# Changelog

## [Unreleased] - 2026-09-28

### Added
- **code-review**: Adicionado arquivo eferences/principles.md com as regras detalhadas de arquitetura, DRY, KISS e responsabilidades.
- **e2e-testing**: Adicionado arquivo eferences/playwright-guidelines.md com as estratégias e regras de cobertura para Playwright.
- **scaffold-project**: Adicionados arquivos eferences/angular-architecture.md e eferences/dotnet-architecture.md detalhando a infraestrutura e convenções.
- **AGENTS.md**: Adicionada regra global de Changelog Maintenance para que o agente principal mantenha este arquivo atualizado em todas as iterações.

### Changed
- Refatoração das skills adotando o padrão **Progressive Disclosure**:
  - Os arquivos SKILL.md receberam o frontmatter YAML obrigatório (
ame e description).
  - Os textos longos foram movidos para a pasta eferences/.
  - O conteúdo dos SKILL.md foi reescrito no formato de "runbooks" e checklists acionáveis para guiar a execução do agente.
- Os diretórios das skills (code-review, e2e-testing, scaffold-project) foram movidos para a estrutura oficial dentro da pasta raiz skills/.
