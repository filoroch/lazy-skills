Eu montaria essa Skill com uma ideia central: **code review não é procurar “código bonito”; é verificar se a implementação preserva o comportamento de negócio, mantendo a solução simples, compreensível e sustentável**.

E acho importante separar **princípios universais** de **padrões aprendidos pelo projeto**. Assim a Skill consegue evoluir durante as iterações sem transformar toda preferência pontual em regra absoluta.

### Estrutura conceitual

```text
code-review
├── 1. Objetivo e prioridade
├── 2. Correção de negócio
├── 3. Simplicidade e legibilidade
├── 4. Arquitetura e responsabilidades
├── 5. Persistência e transações
├── 6. Testes
├── 7. Performance e otimização
├── 8. Padrões de projeto
├── 9. Padrões aprendidos do projeto
└── 10. Classificação dos apontamentos
```

### 1. Hierarquia de análise

Eu colocaria uma ordem explícita para evitar que a Skill fique obcecada por refatoração:

**1. Funcionamento correto do negócio**  
**2. Integridade e consistência dos dados**  
**3. Responsabilidades e arquitetura**  
**4. Simplicidade/compreensão**  
**5. Performance**  
**6. Padrões e estilo**

Ou seja, uma violação de DRY não deve receber a mesma importância que uma regra de negócio implementada incorretamente.

---

### 2. DRY, KISS e duplicação

Aqui está um princípio que vale muito a pena deixar explícito:

> **DRY não significa eliminar toda duplicação.**

A Skill deve avaliar o **custo da abstração**.

Por exemplo:

```text
Código A
Código B
```

pode ser melhor que:

```text
Código A
      ↓
Abstração genérica
      ↓
Código B
```

se a abstração exigir parâmetros, interfaces, herança ou condicionais que tornem o comportamento menos óbvio.

Eu colocaria algo como:

- duplicação pequena e clara → aceitável;
- duplicação de regra de negócio → investigar;
- duplicação estrutural simples → pode ser mantida;
- duplicação que gera inconsistência quando alterada → forte candidato a abstração;
- abstração criada apenas para eliminar algumas linhas → questionar;
- abstração que esconde diferenças importantes entre comportamentos → evitar.

Isso combina **DRY + KISS**, em vez de tratar DRY como regra absoluta.

---

### 3. Responsabilidades

A Skill deve procurar **vazamento de responsabilidade**.

Exemplos:

```text
Controller
 └── regra de negócio
```

ou:

```text
Repository
 └── decisão de negócio
```

ou:

```text
Service
 └── detalhes específicos de apresentação
```

ou:

```text
Domain
 └── dependência de infraestrutura
```

Mas também evitar o extremo oposto:

```text
ServiceA
 └── ServiceB
      └── ServiceC
           └── ServiceD
```

só porque cada pequena operação foi abstraída.

A pergunta principal seria:

> **Essa responsabilidade está onde alguém que conhece o domínio esperaria encontrá-la?**

---

### 4. Testes

Esse ponto merece bastante destaque.

Para backend:

> **Testes devem validar comportamento de negócio, não simplesmente execução de código.**

Então a Skill deve questionar testes que essencialmente fazem:

```text
chamou método X
chamou método Y
repository recebeu exatamente tal chamada
```

quando isso não representa um comportamento relevante.

Preferir:

```text
Dado determinado estado
Quando determinada operação acontece
Então determinada regra de negócio é satisfeita
```

Mocks devem existir para isolar dependências quando necessário, não para transformar o teste em uma reprodução da implementação.

Isso também ajuda a evitar testes que quebram a cada refatoração interna sem alteração de comportamento.

---

### 5. Persistência

Eu transformaria sua regra em uma política de decisão:

```text
ORM
 ↓
É suficiente?
 ├── Sim → utilizar ORM
 └── Não
      ↓
A complexidade/performance justifica SQL explícito?
 ├── Sim → SQL/Dapper/etc.
 └── Não → continuar com ORM
```

E sempre considerar:

- transação;
- atomicidade;
- concorrência;
- quantidade de queries;
- N+1;
- tracking desnecessário;
- carregamento excessivo;
- integridade dos dados.

A Skill não deve dizer simplesmente **“ORM sempre”**.

Ela deve perguntar:

> **O ganho obtido pelo SQL explícito justifica o custo adicional de manutenção, acoplamento e complexidade?**

Isso é particularmente interessante para o seu contexto de EF Core/NHibernate/Dapper.

---

### 6. Padrões de projeto

Eu faria uma distinção importante aqui.

A Skill **não deve procurar oportunidades para enfiar Design Patterns no código**.

Em vez disso:

> Quando existe um problema estrutural conhecido, verificar se um padrão estabelecido resolve o problema de maneira mais simples e compreensível.

Exemplos de padrões que podem ser considerados:

- Abstract Factory
- Factory Method
- Template Method
- Builder
- Strategy
- Observer
- Singleton
- Adapter
- Decorator
- Chain of Responsibility

Mas sempre com a pergunta:

> **O padrão reduz complexidade ou apenas a reorganiza?**

Por exemplo, transformar 20 linhas em 8 classes não é automaticamente uma melhoria.

---

### 7. Performance

Gostei bastante do seu último ponto e faria dele uma regra própria:

> **Todo comportamento que puder ser otimizado deve ser explicitado na revisão, mesmo quando a otimização não deve ser aplicada imediatamente.**

Isso permite diferenciar:

```text
BUG
```

de:

```text
OPORTUNIDADE DE OTIMIZAÇÃO
```

Por exemplo:

```text
Atualmente:
100 queries

Possível alternativa:
1 query

Impacto atual:
baixo para o volume existente

Recomendação:
não alterar agora; considerar quando o volume crescer.
```

Assim o review não vira uma caça prematura a performance.

---

### 8. Padrões aprendidos durante o projeto

Essa é provavelmente a parte que vai tornar essa Skill realmente interessante.

Além dos princípios gerais, ela deve observar:

```text
Iteração 1
↓
Iteração 2
↓
Iteração 3
↓
padrão recorrente
↓
registrar como convenção
```

Por exemplo, depois de várias implementações:

> Todos os comandos que alteram estado retornam `Result<T>`.

A Skill passa a considerar isso uma **convenção do projeto**.

Mas eu colocaria níveis:

```text
Princípio universal
    ↓
Convenção arquitetural
    ↓
Padrão observado
    ↓
Preferência pontual
```

Uma preferência pontual não deve virar regra automaticamente.

Isso também cria uma conexão muito boa com a Skill `auto-learn` que você já criou.

---

### 9. Resultado do Code Review

Eu faria a Skill produzir apontamentos classificados:

| Categoria | Significado |
|---|---|
| 🔴 **Problema** | Pode produzir comportamento incorreto ou violar uma regra importante |
| 🟠 **Arquitetura** | Responsabilidade, acoplamento ou desenho problemático |
| 🟡 **Manutenção** | Complexidade, duplicação ou legibilidade |
| 🔵 **Otimização** | Possível melhoria de performance |
| 🟣 **Padrão** | Oportunidade de utilizar padrão/convenção existente |
| 🟢 **Sugestão** | Melhoria opcional |

E cada apontamento deveria explicar:

```text
Problema
↓
Por que isso importa
↓
Impacto
↓
Alternativas
↓
Recomendação
```

Sem simplesmente dizer:

> "Refatore isso."

---

## Princípio central da Skill

Eu resumiria tudo em uma regra que ficaria logo no início da Skill:

> **Priorize a correção do comportamento de negócio. Depois avalie responsabilidades, simplicidade, manutenção, performance e padrões. Não introduza abstrações, padrões ou otimizações cujo custo de complexidade seja maior que o benefício obtido.**

Isso deixa a Skill alinhada com o que você vem buscando nos outros projetos: **arquitetura consistente, mas sem transformar arquitetura em burocracia**.

E eu adicionaria uma regra especialmente importante para os reviews feitos por IA:

> **Não propor uma alteração apenas porque uma alternativa é teoricamente mais elegante. A recomendação deve apresentar o problema concreto, o impacto e o benefício esperado da mudança.**