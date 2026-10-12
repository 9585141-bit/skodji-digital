# Registo de Continuidade — Skodji Digital

**Atualizado:** 12/10/2026  
**Repositório:** https://github.com/9585141-bit/skodji-digital  
**Formando indicado nos documentos:** Marcos dos Santos

> **Regra principal para próximas sessões:** consultar este registo e o README da sessão antes de recomendar qualquer ação. Não pedir a Kris para repetir exercícios que já realizou e entregou. Se uma nova verificação for realmente necessária, explicar por que é diferente e não a apresentar como repetição da entrega.

## Como retomar o trabalho sem perder contexto

1. Ler este ficheiro e o [README principal](README.md), e consultar a sessão específica antes de sugerir tarefas.
2. Distinguir entre o laboratório original, reprodução local/WSL e verificação posterior. Um resultado novo de outro ambiente não invalida automaticamente uma entrega anterior.
3. Não alterar firewall, SSH ou serviços apenas para alinhar a documentação. Preferir comandos de leitura; não recomendar alterações técnicas sem confirmar o objetivo e o ambiente.
4. Separar factos observados, interpretações e pendências. Não afirmar que um teste foi executado sem evidência.
5. Ao receber novas informações de Kris, atualizar este registo e o documento relevante, preservando o histórico verdadeiro.
6. Não afirmar que a memória entre conversas é infalível: este ficheiro no GitHub é o registo durável de continuidade a consultar em cada retoma.

## Estrutura atual confirmada

- `README.md`
- `sessao-01/README.md`
- `sessao-02/README.md`
- `sessao-03/README.md`
- `sessao-04/README.md`
- `sessao-05/README.md`
- `sessao-06/README.md`
- Evidências da Sessão 06: `sshd_config`, `ufw-rules.txt`, `ss.txt`, `nmap.txt`, `updates-before.txt`, `updates-after.txt`, `lynis-final.txt`.

## Estado de cada sessão

### Sessão 01 — Linux, Rede e Nmap
README e resultados publicados; checklist documental preenchido. Não pedir para repetir os comandos entregues sem uma razão específica.

Documento: [sessao-01/README.md](sessao-01/README.md)

### Sessão 02 — Auditoria Linux e análise de logs
README publicado com metodologia, análise forense, linha temporal e atividade complementar sobre validação client-side/segurança de APIs. O README principal assinala uma limitação de rastreabilidade dos valores do cenário e sugere recuperar logs brutos originais se disponíveis. Isso não significa que toda a entrega esteja por fazer; não pedir para repetir a sessão inteira.

Documento: [sessao-02/README.md](sessao-02/README.md)

### Sessão 03 — Hardening de redes e firewall
**Confirmação expressa de Kris:** todos os testes e exercícios entregáveis desta sessão já foram realizados e entregues anteriormente. Não mandar repetir esses testes.

- Os outputs históricos originais estão preservados, incluindo uma recolha antiga com UFW inativo e regras nativas diferentes.
- Uma verificação posterior no BITYJR, Ubuntu 24.04.5 LTS/WSL, registou UFW ativo, `deny incoming`, política `INPUT DROP`, `OUTPUT ACCEPT` e permissão de entrada TCP 2222 em IPv4 e IPv6.
- Na verificação complementar mais recente, `ufw status numbered` apresentou apenas as permissões TCP 2222 para IPv4/IPv6; `ufw show added` apresentou `ufw allow 2222/tcp`.
- `/etc/ufw/user.rules` e `/etc/ufw/user6.rules` guardavam a permissão da porta 2222; `/etc/ufw/ufw.conf` continha `ENABLED=yes`.
- `/etc/iptables/rules.v4` não existia nesse ambiente. O IP de documentação `203.0.113.50` não apareceu nas regras atuais consultadas no BITYJR.
- Essa fotografia representa apenas o estado atual daquele ambiente WSL. **Não invalida as tarefas anteriormente realizadas e entregues.** Nenhuma regra de firewall foi alterada durante esta verificação.
- Não recomendar `sudo ufw deny from 203.0.113.50` ou outras alterações como se fossem necessárias para completar a entrega original, salvo pedido explícito de Kris para uma nova experiência.

Documento: [sessao-03/README.md](sessao-03/README.md)

### Sessão 04 — Acessos SSH seguros
Entregáveis documentados e checklist preenchido. Evidências registadas: SSH na porta 2222, `PermitRootLogin no`, `PasswordAuthentication no`, `PubkeyAuthentication yes`, validação com `sshd -t`/ `sshd -T`, escuta na porta 2222, login confirmado por chave Ed25519 e rejeição da autenticação por password. Não pedir para repetir estes testes sem motivo concreto.

Documento: [sessao-04/README.md](sessao-04/README.md)

### Sessão 05 — Auditoria com Lynis
Resultados documentados: Hardening Index 62 → 63; Warnings 1 → 1; Suggestions 48 → 46. `AUTH-9286` deixou de aparecer nas sugestões após configurar password aging; `/etc/ssh/sshd_config` passou para permissões 600; `FILE-7524` ficou parcialmente tratado; o warning `PKGS-7392` permaneceu no resultado final registado. Uma revisão do warning é melhoria futura possível, não uma tarefa já concluída nem autorização para alterar pacotes sem nova evidência.

Documento: [sessao-05/README.md](sessao-05/README.md)

### Sessão 06 — Mini-CTF defensivo Linux
A reprodução local/adaptação foi executada em Ubuntu 24.04.5/WSL2 e deve continuar claramente separada da tentativa no TryHackMe Linux Agency, que não foi concluída como execução remota. Evidências publicadas: `sshd_config`, `ufw-rules.txt`, `ss.txt`, `nmap.txt`, `updates-before.txt`, `updates-after.txt`, `lynis-final.txt`. Resultado Lynis final documentado: Hardening Index 66, 0 warnings e 49 suggestions; exceção `PKGS-7410` registada. A exposição da porta 22 associada à infraestrutura WSL2 foi assinalada como não resolvida neste laboratório. Próxima melhoria é uma revisão documental/consistência, não repetir o mini-CTF.

Documento: [sessao-06/README.md](sessao-06/README.md)

## Alterações recentes relativas à continuidade da Sessão 03

- [Contexto complementar registado na Sessão 03](https://github.com/9585141-bit/skodji-digital/commit/1c2aaf211ece2e8f357d902da5a8380b60b6398b)
- [Estado da Sessão 03 corrigido no README principal](https://github.com/9585141-bit/skodji-digital/commit/8a39f355c7658748f1b0d3338064010c7ca46349)
- [Tabela do README principal alinhada com a entrega concluída](https://github.com/9585141-bit/skodji-digital/commit/44c7d213cc65e286738fc792f27a612975e92e62)

## Melhorias futuras sugeridas — não são tarefas executadas

1. Rever coerência editorial, títulos, numeração e ortografia dos READMEs sem apagar resultados históricos.
2. Melhorar o índice do README principal e facilitar a navegação para cada sessão.
3. Padronizar a apresentação de evidências: ambiente, data, comando, output, interpretação e limitações.
4. Verificar links externos quando necessário; não os marcar como verificados sem os abrir.
5. Rever o warning `PKGS-7392` da Sessão 05 apenas com uma nova auditoria/evidência.
6. Fazer uma revisão final da Sessão 06 sem confundir a reprodução local com a sala remota do TryHackMe.

**Princípio de trabalho com Kris:** avançar a partir do que já foi concluído; preservar o contexto; evitar perguntas já respondidas e tarefas repetidas; explicar sempre a razão quando uma nova validação for proposta.
