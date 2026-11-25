# Regras de negócio — versão inicial

**DOC-001 | 25/11/2026 | Estado:** referência para especificação e testes, **não** regras contratuais homologadas.  
Legenda: **CONFIRMADO** = comprovado pela planilha aprovada ou confirmado na conversa; **PROPOSTO** = decisão de produto a validar; **PENDENTE** = requer esclarecimento operacional/jurídico.

## 1. Referência e dados confirmados

**Arquivo de origem externo ao Git:** `escala_motoristas_modelo_gerente_com_horas.xlsx`.

| Informação | Estado | Evidência / observação |
|---|---|---|
| Início do período-base: **16/12/2026** | CONFIRMADO | Escala proposta aprovada pelo gerente. |
| Término da base aprovada: **31/01/2027** | CONFIRMADO | Escala proposta aprovada. |
| **47 dias**, todos com **três motoristas escalados** | CONFIRMADO | Aba Resumo e alertas da planilha aprovada. |
| Cinco funcionários principais: dois rótulos **12x36**, três **5x2** | CONFIRMADO | Aba Horários padrão e escala aprovada. |
| Um profissional de **cobertura temporária**, existência confirmada | CONFIRMADO | Confirmação posterior do gerente. Nome/horário não preenchidos no arquivo. |
| **14 jornadas** atribuídas à cobertura no período | CONFIRMADO | Resumo da planilha; não equivale a 14 dias consecutivos. |
| Rubens em férias na base, com término indicado em **05/01/2027** e retorno planejado **06/01/2027** | CONFIRMADO NA PROPOSTA | Conferir com registro administrativo ao migrar. |
| `Planilha1` é o **modelo original do gerente** | CONFIRMADO | Não usar as abas derivadas como modelo primário de layout. |
| `Escala proposta` atual representa a proposta **aprovada** | CONFIRMADO | Preservar na migração, sem regerar silenciosamente. |

### Interpretação da célula da escala

- **Célula vazia** em dia ativo do funcionário na aba Escala proposta adaptada = trabalho planejado.
- **FOL** = folga planejada; **FER** = férias planejadas.
- **—** = fora do período ou fora da equipe; **não** contar como folga, presença nem férias.
- Datas anteriores a 16/12/2026 na grade de dezembro não fazem parte do período-base importado.
- As cores são apenas representação. O sistema deve guardar **estado estruturado** e gerar formatação ao exportar.
- Cabeçalhos e feriados da planilha são referências de apresentação; feriados e seus efeitos devem ser configurados e validados, não inferidos como regra de jornada.

## 2. Capacidade, cobertura e disponibilidade

### REG-001 — Cobertura diária
**CONFIRMADO para a base; PROPOSTO como parâmetro inicial geral.**

Na proposta-base exige-se **exatamente três motoristas escalados por dia**. Na aplicação, esse parâmetro precisa ser configurável por data/turno, se a operação mudar.

- Menos de três: apontar **descobertura**.
- Mais de três: apontar **excesso**, a menos que haja exceção aprovada e parametrizada.
- O otimizador não pode declarar sucesso quando o requisito de cobertura não foi atendido.
- Impedimento: apresentar um resultado **inviável** com motivo, nunca inventar trabalhador.

### REG-002 — Disponibilidade
**PROPOSTO.**

Férias/afastamentos confirmados retiram a pessoa da disponibilidade do período. A cobertura temporária tem vínculo e disponibilidade com início/fim e não deve ser tratada como membro permanente automaticamente. Toda sugestão deve verificar disponibilidade, condições de turno e restrições das datas vizinhas.

### REG-003 — Regimes 12x36 e 5x2
**Rótulos CONFIRMADOS; regras exatas PENDENTES.**

A planilha identifica dois motoristas como 12x36 e três como 5x2. Ela também documenta que, com um motorista de cada dia vindo do grupo 12x36 e apenas dois dos três 5x2 necessários por dia, há **14 jornadas semanais** para o grupo 5x2, não 15. Portanto, na proposta aprovada, ocorre rodízio que pode resultar em **terceira folga semanal**.

- Não converter automaticamente o nome do regime em obrigação contratual, limite de horas ou direito a compensação.
- O gerador deve preservar o histórico e permitir parâmetros de regime **com vigência**.
- Restrições de descanso, limites de jornada, intervalos e feriados dependerão de validação da empresa/RH e legislação/acordo aplicáveis.
- **Não** assumir que todo período futuro será solucionável com o quadro fixo, em especial durante férias.

## 3. Horários e horas

### REG-004 — Jornada padrão e exceção
**CONFIRMADO como necessidade do produto; PROPOSTO como desenho.**

Cada funcionário pode ter horário padrão com quatro marcações: entrada, saída para intervalo, retorno e saída. Exceções podem alterar um dia específico, sem recalcular retroativamente períodos anteriores.

Os horários padrão de Marcos, José Maria e cobertura temporária constam **pendentes** na planilha aprovada. Há uma divergência de referência na jornada de Rubens: pelas marcações existentes ela totaliza **7h20**, não as 8h presumidas em contexto anterior; a empresa deverá conferir.

### REG-005 — Horas planejadas vs. realizadas
**PROPOSTO, obrigatório para integridade dos relatórios.**

- Horas **previstas**: cálculo a partir de uma jornada escalada com horários planejados completos.
- Horas **realizadas**: marcações reais registradas ou confirmadas; não preencher como realizado apenas porque existe escala planejada.
- Horas **pendentes**: não há marcações suficientes para apuração confiável.
- Ausência confirmada pode resultar em zero horas de trabalho efetivo, mas o motivo administrativo permanece em ocorrência separada.
- Não somar férias, folgas, datas fora da equipe ou faltas como **horas trabalhadas**.
- Exibir totais por **dia, semana, mês, funcionário e intervalo**; diferenciar o período de apuração.
- Proteger jornadas que atravessam 00:00, intervalos e mudanças de horários; armazenar data/hora completa para instantes, não apenas quatro strings soltas.
- O sistema **não** é, na v1, ponto eletrônico oficial nem folha de pagamento.

## 4. Ocorrências e modificações

### REG-006 — Tipos previstos
**PROPOSTO.**

Férias, falta justificada, falta sem justificativa, afastamento por doença sem dado médico desnecessário, folga extra, troca de folga, convocação extra, atraso, saída antecipada, mudança pontual ou futura de horário, entrada/saída de funcionário e extensão de cobertura temporária.

Ocorrências devem ter tipo, início/fim, estado de aprovação, motivo administrativo mínimo, autor, data da operação e referência às revisões afetadas.

### REG-007 — Planejado, publicado e realizado
**PROPOSTO.**

- **Rascunho/planejado**: pode ser simulado e revisto.
- **Publicado**: versão comunicada/aprovada; alteração gera uma **nova revisão**, não reescrita silenciosa.
- **Realizado**: registros posteriores que podem divergir do publicado; não substituir o planejamento histórico.

### REG-008 — Processo de alteração
**PROPOSTO.**

1. Selecionar funcionário e intervalo.
2. Registrar a intenção/ocorrência.
3. Calcular impacto na cobertura, horas e restrições.
4. Exibir alternativas (ou inviabilidade) e **diferenças antes/depois**.
5. Exigir confirmação de usuário autorizado.
6. Persistir todas as alterações correlatas em **uma transação**, com auditoria.
7. Caso haja falha ou conflito de versão, não aplicar parcialmente.

Nenhum processo automático deve criar férias, revogar afastamentos, violar restrições confirmadas ou publicar uma nova versão sem confirmação.

## 5. Otimização e planejamento longo

### REG-009 — Replanejamento com OR-Tools
**PROPOSTO.**

**Restrições obrigatórias iniciais:** disponibilidade aprovada, inexistência de duplicidade/choque de turnos, cobertura exigida, preservação de datas bloqueadas/publicadas salvo autorização, integridade entre período e equipe.

**Preferências:** minimizar mudanças, equilibrar distribuição de folgas/domingos conforme política definida, respeitar padrões e reduzir necessidade de terceiros.

O motor de validação **não depende** do solver. Resultado do solver pode ser: válida, ótima, inviável, sem resposta dentro do limite ou erro; explicar estado corretamente ao usuário.

### REG-010 — Planejamento de 2027
**PROPOSTO.**

Preservar 16/12/2026–31/01/2027. Gerar **propostas preliminares** até 31/12/2027 e seguir automaticamente em períodos futuros, com capacidade de replanejar um mês, semana ou intervalo. Verificar também datas **anteriores e posteriores** para continuidade de folgas/descanso.

A geração anual deve reportar meses inviáveis sem cobertura adicional. Nunca publicar automaticamente todo o ano após executar o otimizador.

## 6. Excel bidirecional

### REG-011 — Exportação gerente
**CONFIRMADO como requisito.**

Exportar escala por mês/intervalo no **layout da primeira folha original** (funcionários nas linhas, dias nas colunas, FOL verde, FER azul, trabalho em branco, dias e cabeçalhos). Feriados e limites de impressão devem ser testados.

### REG-012 — Exportação editável e reimportação
**CONFIRMADO como requisito; contrato de arquivo PROPOSTO.**

- Exportar um mês, semana ou intervalo, com identificadores estáveis, versão-base e status estruturados em aba técnica.
- Uma proposta alterada externamente deve poder ser importada **apenas no intervalo correspondente**, sem sobrescrever outros meses.
- Antes de aplicar: validar arquivo, estrutura, datas, IDs, conflitos com a versão atual, jornada, indisponibilidade e cobertura; mostrar diff.
- O mecanismo **não pode usar somente cores** para interpretar trabalho/folga.
- Se o intervalo exigir ajuste fora da seleção, bloquear ou pedir extensão explícita do horizonte, sem mudanças invisíveis.
- Salvar importação aprovada como nova versão **atômica** e auditável.

## 7. Pendências a resolver antes de habilitar regras rígidas

1. Quantos motoristas são necessários em cada **turno**, não apenas por dia?
2. Horários completos e vigência de cada funcionário, inclusive temporário.
3. Regimes reais, folgas, limites legais, intervalos e acordo coletivo aplicável (consulta RH/gestor responsável).
4. Calendário de férias e disponibilidade de cobertura após janeiro de 2027.
5. Políticas de horas extras, banco/compensação e autorização de convocação.
6. Quem aprova mudanças, quem pode registrar apontamentos e se será necessária segregação de papéis.
7. Calendário de feriados aplicável à unidade.
8. Tratamento de cancelamento/reversão de publicação e conflitos de reimportação.

## 8. Casos de aceite de referência

- Importação da base deve produzir **47 datas** e **três trabalhadores planejados em cada data**.
- Carga importada deve conservar **14 jornadas do substituto** e férias do funcionário indicado, sem preencher horas realizadas artificialmente.
- Um pedido de folga que derrube a cobertura de três para dois deve mostrar conflito e, se existir, sugestão de substituição.
- Se ninguém estiver disponível, mostrar inviabilidade sem publicação.
- Alterar horário padrão com vigência futura não pode alterar horas planejadas de datas anteriores.
- Reimportar um mês deve detectar versão antiga e não alterar outro mês de forma silenciosa.
- Exportar após mudanças deve representar exatamente a escala aprovada atual.

> **Gatilho para o desenvolvimento:** transformar esses casos em fixtures e testes sintéticos rastreáveis na issue DOC-002. Não copiar arquivo bruto para o repositório público sem decisão específica.
