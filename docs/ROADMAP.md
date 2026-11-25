# Roadmap de implementação — v0.1

**Data:** 25/11/2026  
**Método:** uma issue curta → branch → Codex → testes → PR → revisão técnica → aceite humano → merge.  
**Estado:** DOC-001 em elaboração nesta branch; todas as demais entregas são **planejadas**, não implementadas.

## Marco 0 — Fundar o projeto (sem Codex, preferencialmente)

| Issue | Entrega | Critério objetivo |
|---|---|---|
| **DOC-001** | README, AGENTS, Plano Diretor, Regras de negócio, Decisões, Roadmap | Fontes claras; pendências visíveis; documentos revisados e aprovados pelo mantenedor. |
| **DOC-002** | Casos de teste e fixture sintética da escala aprovada | 47 datas, três motoristas/dia, 14 jornadas de cobertura, legenda coerente com a planilha, sem dados reais desnecessários. |

**Gate 0:** documentação aceita; fixture de referência reprodutível e critérios operacionais pendentes destacados.

## Marco 1 — Fundação técnica local

| Issue | Entrega | Critério objetivo |
|---|---|---|
| **INF-001** | Django, ambiente Python, Docker Compose **local**, testes e healthcheck | Inicialização reproduzível; sem conflito de portas/volumes com outros projetos; testes passando. |
| **DB-001** | Modelo inicial, migrações, admin mínimo e histórico de jornadas com vigência | Integridade e constraints cobertas; migração reversível em dados sintéticos; nada implantado na C30. |
| **QA-001** | Testes de regras, versão de escala e cobertura | Casos de conflito e aprovação cobertos antes da UI complexa. |

**Gate 1:** aplicação sobe no notebook sem alterar infraestrutura externa; persistência e testes básicos funcionam.

## Marco 2 — Base aprovada e operação mínima

| Issue | Entrega | Critério objetivo |
|---|---|---|
| **ESC-001** | Importação controlada da base aprovada 16/12/2026–31/01/2027 | Contagens idênticas; validação de status e datas; nenhum mês replanejado sem consentimento. |
| **UI-001** | Calendário mensal com estados visuais e navegação | O gerente entende trabalho, férias, folga e cobertura; desktop e celular testados. |
| **HOR-001** | Cadastro de horários, exceções e horas previstas/realizadas | Totais diários/semanais/mensais corretos, sem usar horas planejadas como realizadas. |
| **OCO-001** | Férias, faltas, folgas extras e registro de motivos mínimos | Evento de ausência afeta disponibilidade e alerta, mas não altera publicação sozinho. |
| **XLS-001** | Exportação visual no modelo original do gerente | Documento aprovado em conferência de células, cores, datas e impressão. |

**Gate 2:** fluxo básico de consulta, registro de ocorrência, horas e exportação demonstra valor sem depender do solver.

## Marco 3 — Alterações assistidas e planejamento de 2027

| Issue | Entrega | Critério objetivo |
|---|---|---|
| **OPT-001** | Modelo mínimo OR-Tools CP-SAT e validador separado | Caso viável/inviável testado; limites definidos; nenhuma violação encoberta. |
| **OPT-002** | Simulação de troca/ausência com objetivo de mínima alteração | Diff antes/depois, impacto em cobertura e folgas, aprovação e histórico. |
| **ESC-002** | Planejar fevereiro–dezembro de 2027 em períodos | Preserva base aprovada; verifica fronteiras; identifica falta de pessoal, sem inventar solução. |
| **ESC-003** | Publicar, revisar, comparar e reverter versão | Trilha de auditoria e persistência atômica, incluindo alterações multipessoais. |

**Gate 3:** sistema sugere alterações, distingue inviabilidade e explica efeitos; 2027 pode ser consultado como planejamento **preliminar**, com pendências visíveis.

## Marco 4 — Excel de ida e volta

| Issue | Entrega | Critério objetivo |
|---|---|---|
| **XLS-002** | Exportação estruturada por mês/semana/intervalo | Contém identificadores, versão-base, intervalo e esquema legível pela ferramenta. |
| **XLS-003** | Importação e prévia de alterações | Rejeita dados malformados e revisão obsoleta; informa mudanças; não altera dias fora do escopo. |
| **QA-002** | Round-trip, conflitos, reversão e atomicidade | Importar proposta válida e inválida com testes automatizados sem corrupção. |

**Gate 4:** caso complexo pode sair do sistema, ser analisado externamente e voltar como nova revisão controlada.

## Marco 5 — Relatórios e implantação

| Issue | Entrega | Critério objetivo |
|---|---|---|
| **REL-001** | Dashboard de cobertura, horas, folgas, ausências e pendências | Números correspondem ao banco e distinguem previsto de realizado. |
| **SEC-001** | Papéis e proteção das operações sensíveis | Testes de permissão, uploads, sessões e trilha de ações. |
| **INF-002** | Plano de backup/restauração e simulação de carga | Restaurar ambiente isolado, medir RAM/CPU do solver e recuperação. |
| **INF-003** | Deploy controlado na C30, sujeito a autorização específica | Isolamento de projetos e recursos, sem interromper STAGING/ERP; rollback demonstrável. |
| **QA-003** | Homologação pelo gerente | Fluxos de mudança e Excel validados na operação real. |

**Gate 5:** somente após autorização, testes e backup/restauração o sistema poderá ser considerado apto para uso operacional.

## Dependências essenciais

- DOC-001 precede decisões de implementação; DOC-002 precede importação/solver com referência.
- DB-001 precede ESC-001 e versões.
- HOR-001 e OCO-001 influenciam regras de OPT-001.
- ESC-003 precede XLS-003 (reimportação exige versionamento/auditoria).
- INF-002 e SEC-001 precedem INF-003.
- Regra trabalhista incerta não deve ser marcada como teste aprovado sem validação humana.

## Padrão de issue para o Codex

Cada issue de implementação precisa conter:
1. **Objetivo de negócio** e resultado observável.
2. **Contexto mínimo** e arquivos/documentos relevantes.
3. **Escopo incluído** e **fora de escopo**.
4. **Invariantes** que não podem ser quebradas.
5. **Critérios de aceite verificáveis**.
6. **Testes exigidos** e evidências de execução.
7. **Estratégia de retorno** e riscos se alterar dados/migrações.

## Política de modelos e custos

- O modelo do Codex será indicado **por issue**, após conferir os modelos realmente disponíveis na conta e a dificuldade da tarefa.
- Usar raciocínio moderado para implementação ordinária; elevar esforço somente em migrações críticas, otimização ou bugs difíceis.
- Evitar múltiplos agentes concorrentes sobre o mesmo conjunto de arquivos.
- Preferir ler diff e testes de uma entrega a reenviar o repositório inteiro a cada interação.
- Não prometer uma combinação de modelo/versão como universalmente mais econômica sem medição.

## Definição de pronto por PR

- Critérios da issue atendidos ou pendências explicitadas.
- Testes implementados e **resultados** informados; sem teste não executado descrito como aprovado.
- Nenhuma alteração fora do escopo sem justificativa.
- Documentação e dados de exemplo consistentes.
- Revisão de código concluída.
- Aceite humano antes de merge e **outro aceite** antes de produção.

## Próxima ação após DOC-001

Criar/validar **DOC-002** usando **fixture sintética**, derivada da estrutura da planilha aprovada, para transformar nossa referência de negócio em cenários automatizados. Somente depois iniciar **INF-001** via Codex.

> Roadmap é uma sequência de resultados, não um calendário de prazos. Replanejar issues em caso de descoberta operacional, sem ampliar silenciosamente o escopo.
