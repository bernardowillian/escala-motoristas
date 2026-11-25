# Plano Diretor do Sistema de Gestão de Escalas de Motoristas

**Versão:** 0.1 — 25/11/2026  
**Marco:** DOC-001 — planejamento, não implementação.  
**Estado:** proposta técnica para revisão; áreas pendentes identificadas, não presumidas como resolvidas.

## 1. Por que este projeto existe

O gerente aprovou uma escala Excel funcional, mas considerou difícil operar o conjunto de abas, fórmulas e apontamentos. O sistema deve transformar esse processo em **calendário compreensível**, edição assistida, cálculo confiável de horas e exportações no padrão que ele conhece.

**Resultado esperado:** o gerente consegue consultar quem trabalha, pedir mudança de folga ou período de ausência, entender as consequências, escolher alternativa e confirmar; para casos excepcionais, um arquivo exportado pode ser ajustado externamente e reimportado com validação.

### Critérios de sucesso de produto

1. Consultar escala mensal sem conhecer fórmulas.
2. Registrar ausência e compreender seu impacto em cobertura e horas.
3. Visualizar proposta antes de confirmar alterações multipessoais.
4. Consultar horas **previstas**, **registradas** e **pendentes** sem confundi-las.
5. Exportar escala no layout original e reimportar uma proposta de intervalo sem perda histórica.

## 2. Fonte de verdade inicial

Referência externa: `escala_motoristas_modelo_gerente_com_horas.xlsx` (não versionada neste marco).

Aba `Planilha1`: modelo original do gerente.  
Aba `Escala proposta`: escala aprovada de **16/12/2026 a 31/01/2027**.  
Outras abas: resumo/alertas, folgas semanais, base diária, horários padrão, apontamentos e resumo de horas.

**Pontos confirmados:** 47 dias no período-base; exatamente três motoristas escalados por dia; cinco integrantes fixos (dois identificados como 12x36 e três como 5x2) e um trabalhador temporário confirmado para a cobertura de férias de Rubens, com 14 jornadas previstas.

**Pontos não confirmados:** horários completos de três pessoas; interpretação contratual estrita dos regimes; calendário de férias de 2027; aptidão da equipe permanente para cobrir todos os próximos meses sem profissionais adicionais.

A referência aprovada será migrada com **reconciliação e testes**, nunca substituída automaticamente por uma saída do otimizador.

## 3. Escopo funcional

### 3.1 Tela inicial
Indicadores acionáveis: cobertura por dia, próximos afastamentos/férias, jornadas sem horários, horas previstas/apontadas, pendências e alterações não publicadas. Evitar gráficos puramente decorativos.

### 3.2 Calendário de escalas
Visões mensal e diária, filtros de funcionário, legenda clara (trabalho, folga, férias, afastamento, fora da equipe), status planejado/publicado, ações por data e **prévia de diferença**.

### 3.3 Funcionários
Cadastro, vínculo ativo/temporário, regime com vigência, horário padrão e histórico de disponibilidade. Afastamento não implica desligamento.

### 3.4 Jornadas e ocorrências
Quatro marcações usuais de entrada/intervalo/retorno/saída, exceções por dia, ausências, folga extra, hora extra indicada e motivo/estado de aprovação. Suportar jornadas noturnas e apuração por semana/mês.

### 3.5 Recomendações de ajustes
Ações de trocar folga, substituir ausente, mover horário, convocar extra ou replanejar período. O sistema valida primeiro, simula alternativas e só aplica uma escolha aprovada. Nunca esconde ausência de solução.

### 3.6 Relatórios
Resumo por funcionário/semana/mês/intervalo, dias trabalhados, folgas, férias, domingos, registros de horas e cobertura. Expor pendências de marcação e eventuais dados incompletos, sem inventar totais realizados.

### 3.7 Excel bidirecional
A) **Exportar para gerente** no estilo da primeira aba `Planilha1`.  
B) **Exportar para edição externa** por mês/semana/período, com metadados/identificadores estáveis.  
C) **Importar proposta** com validação, comparação antes/depois e aprovação transacional.

A IA/ChatGPT participa **fora da aplicação** por arquivos, sem chaves de API ou acesso direto ao banco na v1.

## 4. Modelo conceitual de dados

As entidades abaixo são **candidatas**: a modelagem final ficará na issue DB-001.

| Entidade | Responsabilidade |
|---|---|
| Employee | Identificador estável, nome, função, situação, vínculo e elegibilidade. |
| EmploymentAvailability | Período disponível/indisponível e justificativa administrativa mínima. |
| WorkPattern | Regime/horário padrão **versionado por vigência**, sem recomputar retroativamente. |
| Shift | Data local, início/fim planejados, posto/turno se aplicável. |
| ScheduleVersion | Período, estado rascunho/publicado/substituído, revisão e autoria. |
| ShiftAssignment | Pessoa, turno, estado planejado, origem e versão da escala. |
| TimeRecord | Marcação real, origem, confirmação e situação do apontamento. |
| Occurrence | Tipo, período, autor, aprovação e referência à pessoa afetada. |
| OptimizationRun | Entradas/restrições aplicadas, limite, resultado, diffs e motivos de falha. |
| ImportBatch | Arquivo de origem, hash, versão-base, escopo, validação e aprovação. |
| AuditEvent | Autor, data/hora, operação, antes/depois e referência à revisão. |
| UserRole | Login, permissão para ver, editar, aprovar e administrar. |

### Regras transversais de armazenamento

- Identidades persistem independentemente de nome, linha ou cor no Excel.
- Planejamento e apontamentos reais são registros distintos.
- Datas de operação no fuso **America/Sao_Paulo**; instantes armazenados com informação inequívoca de timezone.
- Revisão aprovada nunca é sobrescrita sem histórico e autorização.
- Atualizações com várias atribuições ocorrem em transação única.
- Mensalidade é **forma de exibição**, não fronteira lógica obrigatória para descanso e solver.

## 5. Fluxos principais

### 5.1 Inicialização
Importar base aprovada de 16/12/2026–31/01/2027 → conferir 47 dias/3 motoristas/14 jornadas temporárias → revisar divergências → marcar como versão-base publicada **após aceite**.

### 5.2 Geração do restante de 2027
Registrar regras e indisponibilidades já conhecidas → gerar propostas preliminares de **01/02/2027 a 31/12/2027** → verificar fronteiras e indisponibilidades futuras → marcar períodos viáveis/inviáveis → apresentar para aprovação mensal ou em lotes. Sem publicação automática do ano todo.

### 5.3 Ausência ou troca
Selecionar pessoa/datas → criar ocorrência provisória → validar cobertura e descanso → OR-Tools sugere substituições minimizando mudanças → exibir diff e avisos → confirmar → transação e auditoria → atualizar resumos.

### 5.4 Assistência externa
Exportar intervalo e versão-base → ajuste externo com ChatGPT ou planilha → importar em staging de validação → checar identificadores, conflitos de versão, dias vizinhos, horários e cobertura → mostrar impacto → aprovar ou rejeitar **sem mudança parcial**.

### 5.5 Desfazer
Reverter por **nova revisão compensatória** (respeitando registros posteriores), não por apagar histórico publicado.

## 6. Arquitetura sugerida

### 6.1 Componentes
- **Django monolítico:** autenticação, entidades, regras, endpoints e telas.
- **Templates + HTMX (ou equivalente mínimo):** interações rápidas sem SPA obrigatória.
- **OR-Tools CP-SAT:** biblioteca Python para gerar/simular alternativas.
- **Validador determinístico independente:** cobre regras mesmo para importação manual ou falha do solver.
- **Excel:** leitura/escrita de XLSX a partir de esquema definido e template do gerente; biblioteca Python adequada escolhida na implementação.
- **SQLite** inicialmente: database local protegida, transações e backup consistente; migração futura para PostgreSQL se uso justificar.
- **Docker Compose local e independente:** sem dependência de serviços corporativos já instalados.

Não são requisitos da v1: Kubernetes, Redis, Celery, frontend SPA, microsserviços, IA em tempo real ou integração com folha.

### 6.2 Limites iniciais do solver
- Rodar **sob demanda**, em processo isolado do servidor HTTP.
- Tentar janelas pequenas primeiro e ampliar horizonte quando necessário.
- Limitar recursos de CPU/memória/tempo; não bloquear indefinidamente o gerente.
- Exibir status **ótimo**, **viável**, **inviável**, **limite sem solução** ou **erro**, sem tratá-los como equivalentes.
- Penalizar mudanças em escalas publicadas; preservar regras obrigatórias, não as transformar em simples preferências.
- Registrar configurações suficientes para repetir/diagnosticar um problema.

**Hipótese para benchmark:** aplicativo até ~512 MiB e solver até ~1 GiB, com restrição de 1–2 CPUs lógicas. Não é consumo aferido nem garantia de que toda otimização caberá nesses limites.

### 6.3 Infraestrutura-alvo documentada
Host: Lenovo ThinkStation C30, Ubuntu Server 22.04.5 LTS e Docker. Snapshot comunicado em 25/11/2026:

- 7,7 GiB RAM total, 2,0 GiB usados, **5,3 GiB available**; swap 4 GiB, uso zero.
- 12 CPUs lógicas.
- SSD: / com 76 GiB livres; /home com 127 GiB livres; HDD em /armazenamento2 com 868 GiB livres.
- Sete contêineres existentes (cinco da Gaúcha Gestão STAGING e dois do ERP de Contratos); Kong apresentou ~1,25 GiB RAM no snapshot.
- HDD antigo com risco físico registrado no repositório Gaúcha-Gestao. Documentação do STAGING ainda marca PROD **NO-GO**.

**Consequência:** não fazer deploy durante DOC-001. Futuro deploy exige segregação por projeto, testes em carga, portas/rotas próprias, backup off-site e restauração comprovada; não alterar Caddy, Cloudflare, volumes, contêineres ou ERP sem plano próprio e autorização.

## 7. Modelo de restrições do OR-Tools

**Entradas:** intervalo e dias de margem, funcionários ativos, disponibilidade, horários, cobertura desejada, atribuições já publicadas, regras de jornada validadas e preferências configuráveis.

**Restrições obrigatórias iniciais:** pessoa elegível, data válida, não duplicar atribuição/turno, férias/afastamentos aprovados, cobertura por data ou posto e bloqueios publicados conforme política. Restrições trabalhistas só serão ativadas depois de confirmação formal dos parâmetros.

**Preferências:** minimizar diferenças (dias/pessoas), manter rodízio, evitar concentração de domingos/folgas e reduzir coberturas temporárias, desde que as métricas e pesos sejam revisados com o gerente.

**Saídas:** alterações sugeridas e justificativas, cumprimento de requisitos, resumo de horas/dias, pontuação e status de solução. Resultado inviável deve listar restrições conflitantes **quando diagnosticáveis**; não alegar diagnóstico exato sempre.

**Importante:** consultar fronteiras do período (semana anterior/seguinte, virada de mês) e garantir que períodos consecutivos não criem problemas invisíveis.

## 8. Contrato esperado para Excel

### 8.1 Modelo visual do gerente
Exportar por mês/intervalo no formato equivalente à primeira aba original: funcionário em linha, calendário em colunas, trabalho em branco, `FOL` verde, `FER` azul, feriados, legenda e impressão. A correspondência será conferida visual e programaticamente.

### 8.2 Arquivo de ida e volta
Arquivo XLSX de intercâmbio com:
- Aba legível de escala do período.
- Aba técnica com identificador de funcionário, data, estado/turno, versão-base, escopo e identificador do export.
- Versão explícita do contrato (schema/version).
- Metadados que permitam detectar conflitos e linhas ilegítimas.

O importador interpreta valores estruturados, **não** cores. Recusa datas fora do escopo, nomes ambíguos, IDs desconhecidos, duplicidades e versões obsoletas sem reconciliação explícita. Mostra diff e prévia antes de salvar. Uma solução para março não altera abril silenciosamente; se descanso cruzar fronteira, solicitar extensão do período ou resolver conflito.

## 9. Segurança, operação e histórico

- Contas e permissões para ler/editar/aprovar/publicar; papel exato é pendente.
- Privilégio mínimo; logs de auditoria legíveis, sem segredos ou diagnósticos médicos.
- Backups verificáveis de banco e arquivos, criptografados e com cópia fora da C30.
- Validação de uploads (tamanho, estrutura, extensões, conteúdo), proteção CSRF e controle de acesso nas operações.
- Controle concorrente de versões: duas pessoas não podem confirmar simultaneamente alterações conflitantes sem detecção.
- Repositório é público por decisão do mantenedor no estágio inicial; **não** colocar credenciais, dumps ou dados reais desnecessários.
- Ausências por saúde: guardar apenas o necessário para disponibilidade administrativa.

## 10. Estratégia de testes

1. **Dados de referência:** 47 datas e três escalados/dia, 14 jornadas temporárias, status preservados.
2. **Validações:** impossibilidade de duplicação, indisponibilidade, cobertura insuficiente e bloqueio publicado.
3. **Cálculos:** semana, mês, intervalos, feriados sinalizados, turnos noturnos, marcações incompletas e vigência futura.
4. **Solver:** casos solucionáveis, inviáveis, limite sem resposta e mínimos de mudanças.
5. **XLSX:** exportação no layout gerente, round-trip estruturado, conflito de revisão, input malformado e rejeição sem escrita parcial.
6. **UI:** gerente altera folga e entende as consequências; validação em notebook e celular.
7. **Operação:** teste de recuperação, saúde do serviço e limites da otimização sem interferir nos outros contêineres.

## 11. Etapas e critérios de passagem

Ver [ROADMAP.md](ROADMAP.md) para dependências e issues. O planejamento deve ser atualizado quando uma regra for confirmada ou uma hipótese cair.

**Gate para começar a codificar:** DOC-001 revisado e critérios das primeiras issues definidos.  
**Gate para chamar v1 de utilizável:** importação fiel da base, visualização, edições com validação, horas, Excel e histórico funcionando com testes.  
**Gate para produção:** aceite do gerente, backup/restauração e segurança testados, sem impactar ERP/Gaúcha Gestão.

## 12. O que NÃO estamos prometendo

- Conformidade jurídica automática sem revisão trabalhista.
- Fechamento garantido de todo 2027 com o quadro atual.
- Marcação de ponto oficial ou cálculo de folha.
- Resolver todo cenário com OR-Tools em prazo fixo.
- Publicar automaticamente mudanças ou aplicar importação sem aprovação.
- Implantação na C30 sem procedimento e autorização.

## 13. Pendências prioritárias do produto

- Confirmar jornada e regras legais de cada regime com gestor/RH.
- Validar necessidade de cobertura por turno e diferenças por feriado.
- Completar horários e dados da cobertura temporária.
- Definir disponibilidade/contratação de cobertura para férias de 2027.
- Confirmar perfis de acesso e autorização para publicar alterações.
- Estabelecer política de registros reais, extras e compensações.
- Definir formato final de Excel de intercâmbio e comportamento de conflitos.
