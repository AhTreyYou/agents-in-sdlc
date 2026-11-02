---
title: "Lição 10 - Recapitulação e próximas etapas"
description: "Recapitule o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo de trabalho contínuo na Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem já cadastrada;
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e fez merge manual do primeiro pull request (PR);
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou no modo Autopilot e revisou no modo Interactive;
- orientou o agente com instruções personalizadas, personalizou a skill existente `quality-checks` e a usou para executar testes de unidade, lint e verificações de tipos;
- adicionou o servidor Playwright Model Context Protocol (MCP) e o usou para explorar a filtragem em um navegador real;
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador;
- revisou toda a alteração de filtragem e autorizou o Agent Merge para o PR de filtragem;
- explorou comandos slash para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch criada a partir de `main` atualizada:

1. **Avaliações por estrelas:** exiba `starRating` existente e um estado explícito de ausência de avaliação nos cartões dos jogos.
2. **Fluxo de filtragem e qualidade:** implemente a filtragem, atualize as instruções e aplique-as à funcionalidade, personalize o relatório `quality-checks`, crie um perfil de QA e inclua os testes correspondentes.

Do planejamento da filtragem até a abertura do PR, você usou a mesma conversa e branch. Reunimos esse trabalho em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, inspeção manual no navegador e exploração do navegador pelo Copilot via MCP. A skill `quality-checks` executou testes de unidade, lint e verificações de tipos e apresentou os resultados no novo formato. O QA reuniu esses resultados com uma revisão dos requisitos e da cobertura de testes antes do PR.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que conclui que não são necessários testes novos também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de qualquer alteração.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot influenciam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e ajuste-as conforme as necessidades da equipe mudarem. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** adequados à tarefa. Use **Plan** para pensar na abordagem antes de começar, **Interactive** para acompanhar de perto alterações focadas e **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e um modelo mais capaz para trabalhos complexos.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer criar, *por que* e *como* muda significativamente o resultado.

## Mais para explorar

Você cobriu o fluxo de trabalho principal. Confira mais alguns recursos da CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações;
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva;
- `/fleet` para orquestrar subtarefas independentes em paralelo;
- `/worktree` para isolar uma tarefa separada;
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximas etapas

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais e naquele pequeno aplicativo que você imagina há anos, mas ainda não teve tempo de criar. Compartilhe seus aprendizados com a equipe e aprenda com os demais. E, como sempre, explore a documentação.

Se quiser expandir a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
