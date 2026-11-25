# Registro de decisões — v0.1

**Data:** 25/11/2026. **Status:** baseline de planejamento, revisável por decisão explícita.

Uma decisão alterada deve registrar **motivo, impacto, data e aprovação**. Este arquivo não comprova conformidade trabalhista nem produção liberada.

| ID | Estado | Decisão | Motivo / limites |
|---|---|---|---|
| DEC-001 | Aceita | Aplicação **separada** do repositório Gaúcha Gestão | Evitar impacto no ERP, migrações e serviços preexistentes da C30. |
| DEC-002 | Aceita | Monólito **Python/Django**, interface leve | Reduzir componentes, complexidade operacional e custo de manutenção. |
| DEC-003 | Aceita | **OR-Tools CP-SAT** desde as primeiras funcionalidades de sugestão | Facilitar escala sob restrições; regras e validações continuam independentes. |
| DEC-004 | Aceita | **SQLite** na primeira etapa; reavaliar PostgreSQL diante de requisitos reais | Equipe pequena e poucos usuários; backup e concorrência precisam ser testados. |
| DEC-005 | Aceita | **Excel bidirecional por período** e modelo de exportação fiel à primeira aba da planilha original | Permitir impressão/compartilhamento e assistência externa do ChatGPT sem integração direta. |
| DEC-006 | Aceita | Inicialização em **16/12/2026**, preservar proposta até **31/01/2027**, planejar até **31/12/2027** | Continuidade anual sem sobreposição automática de períodos já aprovados. |
| DEC-007 | Aceita | Histórico de versões, prévia de mudanças e **aprovação humana** | Evitar alterações silenciosas, inclusive por importação ou solver. |
| DEC-008 | Aceita | Desenvolver no notebook com Docker; C30 é alvo **futuro**, não destino automático | Isolar serviços existentes e validar recursos/backup antes de deploy. |
| DEC-009 | Aceita | Assistência ChatGPT **por arquivo**, não IA embutida na v1 | Fluxo simples, barato, auditável e sem acesso da IA ao banco de produção. |
| DEC-010 | Aceita | Issue curta → branch → testes → PR → revisão → aceite do mantenedor | Melhorar rastreabilidade e economia de contexto no Codex. |

## Decisões pendentes

1. **Jornadas e legalidade:** regras contratuais e coletivas efetivas de 5x2 e 12x36, intervalos e limites aplicáveis, a validar com a empresa/RH.
2. **Cobertura diária:** a meta de três trabalhadores vale para todo dia ou varia conforme posto/turno?
3. **Horários faltantes:** completar jornadas padrão não fornecidas na planilha aprovada.
4. **Férias futuras/temporários:** períodos, disponibilidade e política de contratação de cobertura para 2027.
5. **Controle de horas:** somente acompanhamento administrativo ou banco de horas/compensação formal?
6. **Usuários/permissões:** gerente apenas, supervisão ou níveis de edição/aprovação?
7. **Hospedagem:** URL privada, autenticação de acesso, backup e restauração antes de uso real.
8. **Conflitos de importação:** critérios para conflitos entre versão exportada e versão mais recente, com política definida de resolução.

## Limite entre fatos e hipóteses

- **Fato de referência:** planilha aprovada atende três motoristas em cada um dos 47 dias de 16/12/2026 a 31/01/2027 e utiliza 14 jornadas do substituto temporário.
- **Fato de infraestrutura (snapshot 25/11/2026):** C30 Ubuntu Server, 7,7 GiB RAM, 5,3 GiB disponíveis, 12 processadores lógicos, 76 GiB livres em /, sete contêineres em execução. Isso **não** aprova produção.
- **Hipótese inicial de engenharia:** processo Django limitado a ~512 MiB e solver a ~1 GiB, ajustável após benchmark. Não confundir com medição real de um aplicativo que ainda não existe.
