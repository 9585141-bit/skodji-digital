# Skodji Digital

Portfólio de aprendizagem e projetos do percurso **Reskilling Digital — Linux e Cibersegurança**.

**Autor / Formando:** Marcos dos Santos  
**Repositório:** [9585141-bit/skodji-digital](https://github.com/9585141-bit/skodji-digital)

## Organização do portfólio

Cada sessão possui o seu próprio diretório e `README.md`, com objetivos, contexto, procedimentos, evidências, resultados, lições aprendidas e estado das atividades.

```text
skodji-digital/
├── README.md
├── sessao-01/README.md
├── sessao-02/README.md
├── sessao-03/README.md
├── sessao-04/README.md
├── sessao-05/README.md
└── sessao-06/
    ├── README.md
    ├── sshd_config
    ├── ufw-rules.txt
    ├── ss.txt
    ├── nmap.txt
    ├── updates-before.txt
    ├── updates-after.txt
    └── lynis-final.txt
```

---

## Sessão 01 — Linux, Rede e Nmap

**Tema:** Introdução ao Linux para Segurança e Comandos de Rede.

**Foco:** fundamentos Linux; interfaces e endereços IP; sockets e portas em escuta; reconhecimento de serviços; Nmap e deteção de versões; análise inicial da superfície de exposição.

**Práticas:** `ip a` · `ss -tuln` · `nmap -sV -sC`

**Estado documental:** ✅ README e resultados práticos publicados.

📘 [Abrir o trabalho da Sessão 01](sessao-01/README.md)

---

## Sessão 02 — Auditoria Linux e Análise de Logs

**Tema:** análise de autenticação, tentativas falhadas e linha temporal de incidente.

**Foco:** logs em `/var/log`; extração e contagem de IPs; comparação entre autenticações falhadas e bem-sucedidas.

**Práticas:** `grep` · `awk` · `sort` · `uniq`

**Estado documental:** 🟡 README publicado. Rever a rastreabilidade dos valores do cenário e recuperar os logs brutos originais se estiverem disponíveis; o próprio documento assinala essa limitação de evidência.

📘 [Abrir o trabalho da Sessão 02](sessao-02/README.md)

---

## Sessão 03 — Hardening de Redes e Firewalls

**Tema:** políticas UFW e iptables.

**Foco:** políticas de entrada e saída; permissão controlada de SSH; bloqueio de IP de teste; persistência das regras.

**Práticas:** `ufw` · `iptables` · `iptables-save`

**Estado documental:** 🟡 Parcial, com nova evidência publicada. A recolha inicial mostrava UFW inativo; uma verificação posterior no ambiente `BITYJR` (Ubuntu 24.04.5) confirmou UFW ativo, `deny incoming`, `INPUT DROP` e permissão de entrada `2222/tcp` para IPv4/IPv6. Ainda falta validar pontos específicos do enunciado, incluindo a regra de bloqueio de IP e a persistência das regras após reinício.

📘 [Abrir o trabalho da Sessão 03](sessao-03/README.md)

---

## Sessão 04 — Gestão Segura de Acessos SSH

**Tema:** autenticação por chave e hardening do serviço SSH.

**Foco:** chaves Ed25519; desativação de autenticação por password; bloqueio de login root; mudança da porta; validação de `sshd_config` e teste de acesso.

**Práticas:** `ssh-keygen` · `sshd -t` · `sshd -T` · `systemctl`

**Estado documental:** ✅ Evidências de configuração, escuta na porta 2222 e autenticação por chave publicadas. A chave privada não deve ser incluída no repositório.

📘 [Abrir o trabalho da Sessão 04](sessao-04/README.md)

---

## Sessão 05 — Auditoria com Lynis

**Tema:** auditoria automatizada e remediação de fragilidades.

**Foco:** Hardening Index; Warnings; Suggestions; password aging; permissões de ficheiros; comparação antes/depois.

**Estado documental:** 🟡 Auditorias e correções documentadas. O Hardening Index subiu de 62 para 63 e as Suggestions passaram de 48 para 46; o warning `PKGS-7392` permaneceu no resultado final documentado. A pendência deve continuar visível até existir nova validação.

📘 [Abrir o trabalho da Sessão 05](sessao-05/README.md)

---

## Sessão 06 — Mini-CTF Defensivo Linux

**Tema:** resposta a incidente, contenção, remediação e validação.

**Foco:** identificação de portas/serviços; auditoria de contas; UFW; hardening SSH; patches; Lynis.

**Estado documental:** 🟡 Relatório e evidências da reprodução local no Ubuntu 24.04.5/WSL2 publicados. Resultado Lynis final documentado: Hardening Index 66, 0 warnings e 49 suggestions. A tentativa no TryHackMe Linux Agency está identificada separadamente e não é apresentada como uma execução remota concluída. A exposição da porta 22 na infraestrutura WSL2 ficou registada como não resolvida neste laboratório.

📘 [Abrir o trabalho da Sessão 06](sessao-06/README.md)

---

## Estado do percurso

| Sessão | Área principal | Estado atual |
|---|---|---|
| 01 | Linux, rede e Nmap | ✅ Documentação e resultados publicados |
| 02 | Auditoria e logs | 🟡 Rever rastreabilidade da evidência bruta |
| 03 | UFW e iptables | 🟡 Falta evidência de política final aplicada |
| 04 | SSH seguro | ✅ Configuração e testes documentados |
| 05 | Lynis | 🟡 Warning de pacotes vulneráveis permanece |
| 06 | Mini-CTF defensivo | 🟡 Reprodução local documentada; tentativa TryHackMe separada |

## Próximas prioridades

1. **Sessão 03:** recolher evidência da regra de bloqueio de IP solicitada e verificar a persistência das regras UFW/iptables após reinício, se o ambiente de laboratório permitir.
2. **Sessão 05:** rever o warning `PKGS-7392` e só atualizar o estado depois de uma nova auditoria com evidência.
3. **Sessão 06:** fazer uma revisão final dos ficheiros obrigatórios, dos links e da distinção entre a reprodução local e a sala TryHackMe.

## Linha de aprendizagem

```text
Sessão 01 — Linux + Rede + Nmap
        ↓
Sessão 02 — Web + Autenticação + Logs
        ↓
Sessão 03 — Hardening + Firewalls
        ↓
Sessão 04 — SSH + Chaves criptográficas
        ↓
Sessão 05 — Auditoria + Lynis
        ↓
Sessão 06 — Resposta a incidente + Validação
```

## Princípios de documentação

O Git funciona como registo vivo do percurso de aprendizagem. As evidências devem ser reproduzíveis e distinguir sempre:
- resultados efetivamente observados;
- interpretações técnicas;
- limitações do ambiente;
- tarefas ainda pendentes.

Não publicar passwords, chaves privadas, tokens, cookies ou outros dados sensíveis. Quando um resultado não estiver confirmado, manter a pendência explícita em vez de declarar a tarefa concluída.

---

**Portfólio organizado até à Sessão 06; continuam identificadas as pendências técnicas e de evidência acima.**
