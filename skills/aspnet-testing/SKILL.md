---
name: aspnet-testing
description: >-
  Use this skill when running or fixing unit tests and validating relevant ASP.NET API endpoints with curl.
  It covers test discovery, regression diagnosis, HTTP checks, and a complete execution report.
---

# ASP.NET Testing

## Objetivo

Validar alterações em APIs ASP.NET por meio de testes automatizados e chamadas HTTP direcionadas aos endpoints relevantes. Priorize a causa da falha, não a simples alteração de expectativas para fazer a suíte passar.

## Descoberta e testes unitários

1. Inspecione a solução, os projetos `.csproj`, a configuração de testes e as convenções do repositório antes de executar comandos.
2. Localize todos os projetos de testes. Abra os arquivos `.csproj` para verificar a presença de bibliotecas de teste (como xUnit, NUnit, MSTest) antes de executar comandos. Ignore projetos utilitários, de sandbox ou que não possuam pacotes de teste, mesmo que estejam em pastas de teste. Use nomes, referências e configurações para classificá-los como unitários ou de integração quando possível.
3. Execute a suíte descoberta com `dotnet test`, preservando os comandos, a duração e o resultado de cada projeto.
4. Para cada falha, identifique se há regressão na aplicação, problema de ambiente/dependência, expectativa obsoleta ou quebra de contrato de compilação. É muito comum a refatoração na aplicação quebrar a compilação dos testes por mudanças de assinatura (construtores, injeção de dependência). Revise o setup e as instâncias de mock das classes de teste antes de tratar falhas de asserção. Corrija a causa na aplicação quando estiver no escopo; ajuste o teste somente quando a expectativa anterior não representar mais o comportamento pretendido.
5. Após uma correção, reexecute os testes afetados e a suíte aplicável. Não reporte sucesso enquanto houver falhas não explicadas ou não resolvidas.

## Validação HTTP com curl

1. Descubra os endpoints relevantes pelas alterações, controllers, rotas mínimas, OpenAPI e testes existentes. Não transforme a validação em uma varredura completa da API.
2. Antes de chamar `curl`, confirme que a API já está em execução procurando ativamente por arquivos como `launchSettings.json` ou `docker-compose.yml` para inferir a URL base automaticamente. Faça um *health check* silencioso usando `curl -I` para validar se a aplicação está rodando na porta descoberta, sem interromper o fluxo para perguntar ao usuário. Não inicie, pare ou reconfigure a API.
3. Em Windows, prefira `curl.exe` para evitar o alias do PowerShell. Valide o código HTTP, o tipo de conteúdo e os campos essenciais da resposta, sem comparar o payload completo salvo quando o contrato exigir isso.
4. Execute POST, PUT, PATCH ou DELETE somente em ambiente local ou de desenvolvimento comprovadamente isolado, com dados de teste. Sem esse isolamento, limite-se a endpoints de leitura, health ou status.
5. Não exponha tokens, senhas, cookies ou payloads sensíveis nos comandos nem no relatório. Se autenticação ou uma dependência externa estiver indisponível, priorize a busca por rotas de leitura públicas (como endpoints anotados com `[AllowAnonymous]`) ou endpoints de status/health para garantir que a aplicação responde, ao invés de abortar toda a validação. Caso não haja alternativas, interrompa a validação daquele fluxo e registre o bloqueio.

## Relatório obrigatório

Entregue o relatório final na resposta ao solicitante, contendo:

- escopo descoberto e projetos executados, incluindo itens sem classificação segura;
- comandos de teste, resultados e duração;
- falhas encontradas, causa diagnosticada, correções aplicadas e reexecuções;
- endpoints chamados, método, asserções HTTP e resultado, omitindo dados sensíveis;
- testes ou fluxos não executados, bloqueios e a ação necessária para prosseguir;
- conclusão explícita: aprovado, aprovado com ressalvas ou bloqueado/reprovado.

> Não gere um arquivo de relatório no projeto, a menos que o solicitante peça explicitamente.
