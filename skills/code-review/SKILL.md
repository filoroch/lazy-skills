---
name: code-review
description: >-
  Use this skill to perform code reviews on current changes or pull requests. 
  It prioritizes business logic over pure aesthetics, avoiding over-engineering, 
  and classifying feedback into clear categories.
---

# Code Review

Sempre separe **princípios universais** de **padrões aprendidos pelo projeto**. A Skill deve evoluir durante as iterações sem transformar toda preferência pontual em regra absoluta.

## Passos para Execução do Code Review

1. **Leia e assimile as regras detalhadas de arquitetura e negócios** no arquivo [principles.md](./references/principles.md).
2. **Identifique** quais partes do sistema foram alteradas e analise os arquivos modificados.
3. **Avalie o código** seguindo rigorosamente a Hierarquia de Análise (priorizando correção de negócio e integridade de dados).
4. **Classifique os apontamentos** usando a tabela de cores/ícones e o formato estruturado exigido no documento de princípios.

> **Regra de Ouro**: Priorize a correção do comportamento de negócio. Depois avalie responsabilidades, simplicidade, manutenção, performance e padrões. Não proponha uma alteração apenas porque uma alternativa é teoricamente mais elegante.
