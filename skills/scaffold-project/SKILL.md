---
name: scaffold-project
description: >-
  Use this skill when scaffolding new fullstack projects, or adding new features/CRUDs 
  to existing Angular (.NET backend) applications following the established architecture.
---

# Scaffold Project

## Objetivo
Padronizar a criação e evolução dos projetos fullstack do Filipe, mantendo Angular e .NET sob uma única skill orquestradora.

## Passos para Execução do Scaffold

1. **Leia as Diretrizes de Arquitetura**:
   - Para Frontend: Consulte [angular-architecture.md](./references/angular-architecture.md).
   - Para Backend: Consulte [dotnet-architecture.md](./references/dotnet-architecture.md).
2. **Analise o projeto existente**: Detecte a estrutura e as convenções já presentes antes de adicionar arquivos.
3. **Reutilize scripts**: Prefira reutilizar o scaffold existente, seus scripts/templates ou usar 
g generate (Frontend) e os mecanismos do dotnet-initialr (Backend).
4. **Execute a operação solicitada**, que pode ser:
   - **Fullstack**: Criar CRUD completo coordenando frontend e backend (mantenha os contratos coerentes).
   - **Frontend**: Criar contexto, página, componente, service, model, rota, guard, interceptor.
   - **Backend**: Criar contexto, entidade, repository, command, query, request/response, endpoint, service.
5. **Valide os resultados**: Após a operação, valide imports, referências de projeto, rotas e estrutura resultante.

> **Regras Restritas**:
> - Não adicione abstrações, pastas ou tecnologias que não estejam definidas no padrão referenciado.
> - Não crie pastas opcionais vazias.
> - Não crie testes E2E automaticamente (use a skill e2e-testing).
> - Se uma convenção não estiver definida, pergunte; não a invente silenciosamente.
