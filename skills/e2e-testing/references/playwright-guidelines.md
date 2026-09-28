# Diretrizes e Estratégia de E2E (Playwright)

## Estratégia de Cobertura
Priorize comportamento observável pelo usuário durante os testes:
- navegação; autenticação; autorização; preenchimento de formulários; ações de CRUD; validações visíveis; mensagens de erro/sucesso; redirecionamentos; estados importantes da interface; integração entre frontend e backend (quando fluxo real).

Evite testar detalhes internos de componentes, métodos ou serviços que não sejam observáveis pelo usuário.

## Estrutura Conceitual
Siga uma estrutura que permita identificar facilmente o propósito de cada caso:
`
E2E
├── regressions/
│   └── fluxo-existente.spec.ts
└── features/
    └── fluxo-da-iteracao.spec.ts
`

## Regras
1. Não altere um teste simplesmente para mascarar uma regressão da aplicação.
2. Quando um teste falhar, determine primeiro se a causa está na aplicação, no ambiente ou no próprio teste.
3. Testes devem representar fluxos reais de uso.
4. Prefira seletores estáveis e orientados à acessibilidade ou comportamento visível.
5. Evite dependência desnecessária de classes CSS, estrutura interna do DOM ou detalhes de implementação.
6. Reutilize autenticação e fixtures quando isso reduzir repetição sem esconder o comportamento validado.
7. Mantenha os testes independentes sempre que possível.
8. Não crie uma quantidade excessiva de testes para uma única iteração; comece pelos dois fluxos obrigatórios.
9. Se uma alteração quebrar intencionalmente um fluxo existente, atualize o teste e registre a mudança.
10. O resultado da iteração deve deixar claro quais testes foram executados e passaram.
