## Objetivo

Padronizar testes E2E com Playwright para os projetos, usando os testes como uma etapa de validação ao final de cada iteração.

## Regra principal da iteração

Ao finalizar cada iteração, devem ser executados dois tipos de validação:

1. **Regressão do fluxo existente** — validar um fluxo que já funcionava antes da iteração, garantindo que a nova alteração não quebrou comportamento existente.
2. **Validação do fluxo novo** — validar especificamente o fluxo ou funcionalidade implementado na iteração.

A seleção dos fluxos deve considerar o impacto da alteração. O teste existente deve representar uma parte relevante do sistema afetada ou dependente da funcionalidade alterada.

## Responsabilidades da skill

- Criar testes E2E com Playwright.
- Identificar fluxos de usuário que precisam ser cobertos.
- Criar ou atualizar testes quando uma funcionalidade existente mudar.
- Executar os testes ao final da iteração.
- Diferenciar claramente teste de regressão e teste da funcionalidade nova.
- Investigar falhas antes de alterar o teste para fazê-lo passar.
- Evitar testes excessivamente acoplados à implementação interna da aplicação.

## Estratégia de cobertura

Os testes devem priorizar comportamento observável pelo usuário:

- navegação;
- autenticação;
- autorização;
- preenchimento de formulários;
- ações de CRUD;
- validações visíveis;
- mensagens de erro/sucesso;
- redirecionamentos;
- estados importantes da interface;
- integração entre frontend e backend quando fizer parte do fluxo real.

Evitar testar detalhes internos de componentes, métodos ou serviços que não sejam observáveis pelo usuário.

## Estrutura conceitual

Os testes devem permitir identificar facilmente o propósito de cada caso, por exemplo:

```
E2E
├── regressions/
│   └── fluxo-existente.spec.ts
└── features/
    └── fluxo-da-iteracao.spec.ts
```

A estrutura física pode ser adaptada à organização do projeto, desde que continue claro quais testes representam regressão e quais validam funcionalidades novas.

## Ciclo de uma iteração

```
Implementação
     ↓
Identificar fluxo novo
     ↓
Criar/atualizar teste do fluxo novo
     ↓
Selecionar fluxo existente relevante
     ↓
Executar teste de regressão
     ↓
Executar teste do fluxo novo
     ↓
Corrigir aplicação se houver regressão
     ↓
Reexecutar validação
```

## Regras

1. Não alterar um teste simplesmente para mascarar uma regressão da aplicação.
2. Quando um teste falhar, determinar primeiro se a causa está na aplicação, no ambiente ou no próprio teste.
3. Testes devem representar fluxos reais de uso.
4. Preferir seletores estáveis e orientados à acessibilidade ou comportamento visível.
5. Evitar dependência desnecessária de classes CSS, estrutura interna do DOM ou detalhes de implementação.
6. Reutilizar autenticação e fixtures quando isso reduzir repetição sem esconder o comportamento que o teste precisa validar.
7. Manter os testes independentes sempre que possível.
8. Não criar uma quantidade excessiva de testes para uma única iteração; começar pelos dois fluxos obrigatórios e expandir a cobertura quando houver risco relevante.
9. Se uma alteração quebrar intencionalmente um fluxo existente, atualizar o teste e registrar a mudança de comportamento em vez de tratá-la automaticamente como regressão.
10. O resultado da iteração deve deixar claro quais testes foram executados e se o fluxo existente e o fluxo novo passaram.

## Relação com project-scaffold

`e2e-playwright` é uma skill independente de `project-scaffold`.

O `project-scaffold` cria a estrutura da aplicação. Esta skill adiciona e mantém a validação E2E com Playwright sobre essa estrutura.

## Evolução futura

A skill pode posteriormente incorporar padrões para:

- fixtures compartilhadas;
- autenticação persistida;
- múltiplos ambientes;
- dados de teste;
- execução em CI;
- screenshots e traces em falhas;
- testes paralelos;
- relatórios de execução.

Esses recursos não devem ser introduzidos automaticamente até que exista uma necessidade concreta.