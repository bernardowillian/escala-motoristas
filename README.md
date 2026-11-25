# Sistema de Gestão de Escalas de Motoristas

Aplicação web planejada para organizar escalas, jornadas e ocorrências de motoristas, facilitando a operação de um gerente que hoje utiliza uma planilha Excel.

> **Estado:** planejamento inicial (DOC-001). Ainda não existe aplicação funcional, implantação ou homologação.

## Objetivo

Permitir que o gerente consulte uma escala simples, registre alterações (folgas, férias, faltas, cobertura, horários), visualize seus impactos e aprove propostas de reorganização sem ter de manipular fórmulas de várias planilhas.

## Referência inicial

- Período aprovado: **16/12/2026 a 31/01/2027**, 47 dias.
- Requisito operacional da proposta: **três motoristas por dia**, atendido nos 47 dias.
- Equipe considerada: cinco motoristas fixos (dois indicados como 12x36 e três como 5x2) e um substituto temporário confirmado para as férias de Rubens.
- Cobertura temporária: **14 jornadas planejadas** no período analisado, não necessariamente 14 dias corridos de trabalho.
- Planejamento seguinte: proposta preliminar de **fevereiro a dezembro de 2027**, com avaliação explícita de períodos inviáveis.
- A referência é o arquivo local aprovado `escala_motoristas_modelo_gerente_com_horas.xlsx` (fora do repositório neste marco). A primeira aba, `Planilha1`, contém o **modelo original do gerente**; `Escala proposta` contém a escala aprovada a preservar.

A planilha é **referência para importação e exportação**, não o banco principal. As regras que ainda dependem de validação estão marcadas como pendentes na documentação.

## Produto em uma frase

**Calendário + ocorrências + horas + sugestões verificadas + Excel de ida e volta.**

## Decisões técnicas iniciais

- Backend: Python e Django; interface web com templates e interações leves (HTMX ou equivalente, sujeito à validação de UX).
- Otimização: Google OR-Tools CP-SAT, executado sob demanda e com limites de recursos.
- Persistência inicial: SQLite, com isolamento de dados e plano de backup; reavaliar se o uso exigir PostgreSQL.
- Desenvolvimento local com Docker Compose, projeto próprio; **não reutilizar** banco, migrações ou serviços de negócio da Gaúcha Gestão.
- Implantação futura pretendida: Lenovo ThinkStation C30 com Ubuntu Server LTS, **somente após** testes, backup/restauração e autorização.
- Ajuda externa: exportar período para análise com ChatGPT, reimportar proposta e validar **antes** de salvar.

## Documentação

- [Plano Diretor](docs/PLANO_DIRETOR.md): produto, fluxos, arquitetura, riscos e critérios de aceite.
- [Regras de negócio](docs/REGRAS_NEGOCIO.md): regras confirmadas, premissas e validações pendentes.
- [Decisões](docs/DECISOES.md): escolhas aceitas e decisões abertas.
- [Roadmap](docs/ROADMAP.md): sequência de desenvolvimento e gates.
- [AGENTS.md](AGENTS.md): instruções obrigatórias para agentes de programação.

## Colaboração e fluxo

1. Uma issue descreve o resultado esperado, limitações e testes.
2. Codex implementa numa branch curta, em ambiente **local**.
3. Codex executa os testes adequados, documenta evidências e abre PR.
4. ChatGPT revisa diff, regras e evidências; o responsável valida os fluxos no navegador.
5. O responsável aprova merge e, separadamente, qualquer implantação.

PRs e issues não substituem homologação de funcionalidades nem verificações em produção.

## Fora de escopo da primeira versão

Folha de pagamento, ponto eletrônico oficial, integração automática com IA, app individual de funcionário e garantia automática de conformidade jurídica.

> **Nota:** repositório público no início do projeto por decisão do mantenedor. Não publicar credenciais, backups, dumps, dados pessoais desnecessários ou arquivos brutos operacionais sem revisão específica.
