# AGENTS.md — Regras para agentes de desenvolvimento

**Projeto:** Sistema de Gestão de Escalas de Motoristas.  
**Estado:** documentos DOC-001; não há autorização implícita para desenvolver ou implantar funcionalidades fora de uma issue.

## 1. Fontes e precedência

Antes de qualquer implementação:
1. Ler esta página, README e a issue atribuída.
2. Consultar `docs/PLANO_DIRETOR.md`, `docs/REGRAS_NEGOCIO.md`, `docs/DECISOES.md` e `docs/ROADMAP.md`.
3. Respeitar critérios de aceite definidos na issue.
4. Em divergência relevante, registrar a dúvida e propor decisão; **não preencher lacunas legais ou operacionais por suposição**.

Ordem: decisões recentes explicitamente aprovadas na issue > regras/decisões documentadas > proposta do agente. Mudança de arquitetura exige registro em DECISOES.md e aceite humano.

## 2. Fluxo de trabalho

- Trabalhar na branch da issue, sem commits diretos na main após a inicialização.
- Fazer mudanças pequenas e focadas; não refatorar áreas alheias à tarefa.
- Não acrescentar dependência, serviço ou framework relevante sem justificativa.
- Implementar e executar testes direcionados e, quando possível, a suíte existente.
- Entregar PR com **resumo**, **arquivos afetados**, **comandos de teste e resultados**, **riscos** e **pendências**.
- Declarar honestamente quando um teste não pôde ser executado.
- Não fazer merge sem aprovação do mantenedor.
- Nunca alegar que documentação, simulação ou testes sintéticos equivalem a sistema pronto para produção.

## 3. Integridade da escala — invariantes

- Escala **planejada**, apontamentos **realizados** e **ocorrências** são entidades distintas.
- Uma escala **publicada** não pode mudar silenciosamente; preservar histórico e exigir aprovação de nova versão.
- A proposta-base aprovada de 16/12/2026 a 31/01/2027 deve ser reimportável e testável sem alteração involuntária.
- Meta inicial: **três motoristas por dia**, com resultado inviável explícito quando impossível, nunca falsa cobertura.
- Funcionários indisponíveis (férias/afastamentos aprovados) não podem ser escalados por automatismo, salvo exceção explicitamente modelada e autorizada.
- Evitar sobreposição de turnos, horários impossíveis e violação das **restrições operacionais confirmadas**.
- Não transformar o rótulo 5x2/12x36 em regras legais rígidas sem dados e validação da empresa.
- Replanejamentos devem minimizar alterações no que já foi aprovado; restrições obrigatórias têm precedência sobre preferências.
- Alterações e importações multipessoais devem ser **atômicas** e verificadas antes da confirmação.
- Usar identificadores estáveis para funcionários e datas; cores e posições de células não identificam dados de forma confiável.
- Planejar fronteiras entre meses/semanas e jornadas atravessando a meia-noite.
- Trabalhar no fuso **America/Sao_Paulo** para visualização e regras de dias locais; armazenamento de instantes com timezone explícito.

## 4. OR-Tools e cálculos

- OR-Tools é ferramenta de busca de soluções; validação de regras deve existir **independentemente** do solver.
- Diferenciar: solução válida, ótima, limite de tempo, inviável e erro.
- Não alterar restrições obrigatórias para forçar solução sem aprovação.
- Otimizações devem ser reprodutíveis quando possível, ter limites de tempo/CPU/memória configuráveis e nunca bloquear indefinidamente a interface.
- Separar horas **previstas**, horas **efetivamente apontadas**, ausências e eventuais compensações; horário padrão não prova presença.
- Calcular intervalos, horários alterados e virada de dia com testes específicos.

## 5. Segurança e ambientes

- Não implantar na C30 ou acessar serviços da Gaúcha Gestão/ERP como efeito colateral.
- Docker local deve usar nome de projeto, volumes e portas próprios; **nunca** executar limpeza global ou `down -v` de projetos externos.
- Não executar operações destrutivas em bancos/volumes, nem `git clean`/reset agressivo sem autorização.
- Não versionar segredos, tokens, senhas, dumps, bases reais ou backups.
- Banco e arquivos de dados nunca devem ser acessíveis publicamente sem autenticação.
- Usar fixtures sintéticas nos testes; só incorporar a planilha de referência original após decisão específica sobre uso e minimização de dados.
- O estado público atual do repositório **não dispensa** cuidados básicos com dados de funcionários.

## 6. Qualidade de entrega

- Preferir simplicidade: monólito Django, interfaces sem dependências excessivas, banco SQLite no início.
- Código legível, tipado onde útil, erros compreensíveis para o gerente e logs sem dados sensíveis.
- Critérios mínimos de UX: ações com linguagem clara; prévia antes de alterar; avisos acionáveis; nunca exigir edição de fórmulas.
- Documentar mudanças que alterem regras, formato de Excel ou algoritmo.
- Acrescentar testes de regressão para bugs do otimizador e importação/exportação.

## 7. Resposta ao encerrar uma tarefa

Informar:
- O que foi implementado e o que ficou de fora.
- Comandos de execução/teste e resultados efetivos.
- Arquivos/rotas relevantes, migrações e riscos.
- URL do PR e issue associada, quando existirem.
- Necessidade de aprovação antes de merge/deploy.
