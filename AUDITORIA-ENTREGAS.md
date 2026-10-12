# Auditoria de Entregas por Módulo — Skodji Digital

**Data de início:** 12/10/2026  
**Finalidade:** confirmar entregas pendentes por módulo do Moodle, sem repetir trabalhos já concluídos.  
**Portefólio:** https://github.com/9585141-bit/skodji-digital

## Regras da auditoria

- **Entregue — confirmado:** há confirmação explícita de Kris ou evidência verificável de submissão no Moodle.
- **Documentado no GitHub:** há README, output ou artefactos no repositório. Isto, por si só, não prova que a submissão Moodle foi feita.
- **A confirmar no Moodle:** não foi possível verificar a submissão com o acesso disponível.
- **Pendente confirmado:** só usar quando a página do Moodle indicar claramente que a atividade não foi submetida ou que exige entrega.
- A ausência de um ficheiro no GitHub não prova que o trabalho não tenha sido entregue noutro local.
- Não repetir testes, laboratórios ou entregas já confirmadas. Uma nova verificação de outro ambiente deve ser identificada como complementar, não como repetição.

## Resumo atual

| Módulo | Evidência encontrada | Estado da entrega no Moodle | Próxima ação |
|---|---|---|---|
| **R1-M5 — Linux e Cibersegurança** | Documentação das Sessões 01–06 e artefactos da Sessão 06 no repositório | **Parcialmente confirmado / resto por verificar** | Comparar cada atividade com o estado real no Moodle |
| **R1-M7 — Growth & Analytics** | Registo prévio do exercício de funil de conversão para Golden Tours Mosteiros; não foi localizada uma pasta correspondente no repositório skodji-digital | **A confirmar no Moodle** | Ver lista de atividades, submissões e feedback do módulo |
| **Outros módulos do curso** | A lista completa dos módulos não ficou disponível na leitura atual do Moodle | **Inventário por iniciar** | Acrescentar os nomes exatos a partir da página do curso |

> **Limitação de acesso:** os links do Moodle conhecidos não puderam ser lidos por esta ligação sem sessão autenticada. Por isso, esta auditoria não inventa estados de submissão nem declara uma atividade pendente sem prova. A confirmação final será feita a partir do estado que aparece no Moodle.

---

## Módulo R1-M5 — Linux e Cibersegurança

O portefólio contém documentação para seis sessões. Os checklists nos READMEs referem sobretudo a documentação/portefólio GitHub e não devem ser confundidos automaticamente com o estado da tarefa no Moodle.

| Sessão | Material presente no GitHub | Estado baseado na evidência disponível |
|---|---|---|
| **01 — Linux, rede e Nmap** | [README da Sessão 01](sessao-01/README.md), comandos e resultados práticos | Documentação publicada; submissão Moodle por confirmar |
| **02 — Auditoria Linux e logs** | [README da Sessão 02](sessao-02/README.md), análise forense, linha temporal e atividade complementar de validação client-side/API | Documentação publicada; submissão Moodle por confirmar. O README recomenda preservar os outputs brutos originais caso estejam disponíveis |
| **03 — Firewall UFW/iptables** | [README da Sessão 03](sessao-03/README.md), outputs históricos e verificação complementar do WSL | **Kris confirmou que os testes e exercícios entregáveis já foram realizados e entregues anteriormente.** Não repetir. A observação posterior do BITYJR/WSL não altera esse estado |
| **04 — Hardening SSH** | [README da Sessão 04](sessao-04/README.md), evidência de chave SSH, porta 2222 e rejeição de password; [link de submissão Moodle referenciado no documento](https://elearning.skodjidigital.cv/mod/assign/view.php?id=865) | Documentação publicada; estado da submissão Moodle por confirmar |
| **05 — Auditoria com Lynis** | [README da Sessão 05](sessao-05/README.md), comparação 62→63, warnings 1→1 e suggestions 48→46; [link de submissão Moodle referenciado no documento](https://elearning.skodjidigital.cv/mod/assign/view.php?id=891) | Documentação publicada; estado da submissão Moodle por confirmar. O warning PKGS-7392 permanece como resultado técnico documentado, não prova de falta de entrega |
| **06 — Mini-CTF defensivo** | [README da Sessão 06](sessao-06/README.md) e ficheiros de evidência; [link de submissão Moodle referenciado no documento](https://elearning.skodjidigital.cv/mod/assign/view.php?id=911) | Reprodução local documentada, separada da tentativa TryHackMe; estado da submissão Moodle por confirmar |

### Sobre os links de submissão

Os links acima são os que constam nos documentos do repositório. A existência de um link de submissão indica apenas que a atividade foi referenciada no portefólio; não confirma o estado real da submissão, nota ou feedback.

## Módulo R1-M7 — Growth & Analytics

Há um registo prévio do exercício de funil de conversão para **Golden Tours Mosteiros**. Na estrutura atualmente consultada do repositório skodji-digital, não foi localizada uma pasta específica para este módulo.

| Verificação | Estado |
|---|---|
| Exercício de funil de conversão conhecido | Registado como trabalho do percurso |
| Ficheiro/README específico em skodji-digital | Não localizado na estrutura atual |
| Estado da entrega no Moodle | A confirmar |
| Feedback/nota | A confirmar |

**Não interpretar “não localizado no GitHub” como “não entregue”.** O trabalho pode ter sido submetido no Moodle ou guardado noutro local.

## Registo a preencher durante a verificação no Moodle

Para cada atividade, registar o título exato, o estado apresentado, a data de submissão se disponível e a nota/feedback quando existir.

| Módulo | Atividade (título exato do Moodle) | Estado apresentado no Moodle | Data/nota/feedback | Ação necessária |
|---|---|---|---|---|
| R1-M5 | Por recolher da página autenticada | A confirmar | A confirmar | Não repetir atividade; verificar estado |
| R1-M7 | Por recolher da página autenticada | A confirmar | A confirmar | Não repetir atividade; verificar estado |
| Outros | A acrescentar quando os módulos forem identificados | A confirmar | A confirmar | Acrescentar ao registo |

## Próximas etapas

1. Abrir cada módulo no Moodle e recolher a lista de atividades com o estado de conclusão/submissão visível.
2. Comparar os estados com esta tabela; atualizar apenas quando a página fornecer evidência.
3. Identificar exclusivamente as atividades com entrega confirmadamente pendente.
4. Atualizar este ficheiro e o registo de continuidade, preservando as entregas e os resultados já realizados.

**Princípio de continuidade:** avançar a partir do progresso real de Kris; não mandar refazer tarefas concluídas e nunca transformar falta de visibilidade numa conclusão de que algo está por entregar.
