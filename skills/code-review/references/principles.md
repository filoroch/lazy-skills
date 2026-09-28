# Princípios de Code Review

O code review não é procurar "código bonito"; é verificar se a implementação preserva o comportamento de negócio, mantendo a solução simples, compreensível e sustentável.

### Estrutura conceitual
1. Objetivo e prioridade
2. Correção de negócio
3. Simplicidade e legibilidade
4. Arquitetura e responsabilidades
5. Persistência e transações
6. Testes
7. Performance e otimização
8. Padrões de projeto
9. Padrões aprendidos do projeto
10. Classificação dos apontamentos

### 1. Hierarquia de análise
**1. Funcionamento correto do negócio**  
**2. Integridade e consistência dos dados**  
**3. Responsabilidades e arquitetura**  
**4. Simplicidade/compreensão**  
**5. Performance**  
**6. Padrões e estilo**

### 2. DRY, KISS e duplicação
> **DRY não significa eliminar toda duplicação.**
Avalie sempre o **custo da abstração**.
Regras para análise de duplicação:
- duplicação pequena e clara → aceitável;
- duplicação de regra de negócio → investigar;
- duplicação estrutural simples → pode ser mantida;
- duplicação que gera inconsistência quando alterada → forte candidato a abstração;
- abstração criada apenas para eliminar algumas linhas → questionar;
- abstração que esconde diferenças importantes entre comportamentos → evitar.

### 3. Responsabilidades
Procure por **vazamento de responsabilidade**.
Pergunta principal:
> **Essa responsabilidade está onde alguém que conhece o domínio esperaria encontrá-la?**

### 4. Testes
> **Testes devem validar comportamento de negócio, não simplesmente execução de código.**
Prefira: Dado / Quando / Então. Mocks isolam dependências, não reproduzem a implementação.

### 5. Persistência
`
ORM -> É suficiente? -> Sim
 |-> Não -> A complexidade justifica SQL explícito? -> Sim/Não
`
Sempre considere: transação, atomicidade, concorrência, N+1, integridade.

### 6. Padrões de projeto
**Não procure oportunidades para forçar Design Patterns no código.**
> **O padrão reduz complexidade ou apenas a reorganiza?**

### 7. Performance
Diferencie claramente BUG de OPORTUNIDADE DE OTIMIZAÇÃO.

### 8. Padrões aprendidos durante o projeto
Observe padrões recorrentes nas iterações e registre como convenção. Não transforme preferências pontuais em regras.

### 9. Classificação dos Apontamentos
| Categoria | Significado |
|---|---|
| 🔴 **Problema** | Pode produzir comportamento incorreto ou violar uma regra importante |
| 🟠 **Arquitetura** | Responsabilidade, acoplamento ou desenho problemático |
| 🟡 **Manutenção** | Complexidade, duplicação ou legibilidade |
| 🔵 **Otimização** | Possível melhoria de performance |
| 🟣 **Padrão** | Oportunidade de utilizar padrão/convenção existente |
| 🟢 **Sugestão** | Melhoria opcional |

Cada apontamento deve explicar estruturadamente:
1. Problema
2. Por que isso importa
3. Impacto
4. Alternativas
5. Recomendação

Nunca diga simplesmente "Refatore isso".
