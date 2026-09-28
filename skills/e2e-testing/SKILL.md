---
name: e2e-testing
description: >-
  Use this skill when generating, updating, or running End-to-End (E2E) tests with Playwright.
  It ensures tests are properly split between regressions and new features, prioritizing user-observable behavior.
---

# E2E Playwright Testing

## Objetivo
Padronizar testes E2E com Playwright para os projetos, usando os testes como uma etapa de validação ao final de cada iteração.

## Regra principal da iteração
Ao finalizar cada iteração, execute e valide dois tipos de fluxos:
1. **Regressão do fluxo existente**
2. **Validação do fluxo novo**

## Passos para Execução (Ciclo de Iteração)

1. **Leia as Diretrizes** em [playwright-guidelines.md](./references/playwright-guidelines.md) para garantir aderência às regras e convenções.
2. **Identifique** o fluxo novo implementado.
3. **Crie/atualize** o teste do fluxo novo.
4. **Selecione** o fluxo existente relevante que pode ter sido afetado.
5. **Execute** teste de regressão.
6. **Execute** teste do fluxo novo.
7. **Corrija** a aplicação se houver regressão (e reexecute a validação).

> Nota: A skill e2e-testing atua paralelamente ao scaffold-project, mantendo a validação E2E sem se misturar com as regras de scaffold da aplicação em si.
