# skodji-digital

Portfólio de aprendizagem e projetos do percurso **Reskilling Digital — Linux e Cibersegurança**.

## Sessões documentadas

### Sessão 01 — Introdução ao Linux para Segurança e Comandos de Rede

**Foco:** fundamentos Linux, rede e reconhecimento de serviços.

Práticas documentadas:
- `ip a`
- `ss -tuln`
- `nmap -sV -sC`
- identificação de portas e serviços
- análise inicial da superfície de exposição

Resultado principal:
- `22/tcp` aberto
- SSH: `OpenSSH 9.6p1 Ubuntu 3ubuntu13.18`

[📘 Abrir Sessão 01](sessao-01/README.md)

---

### Sessão 02 — Auditoria de Sistemas Linux e Análise Avançada de Logs

**Foco:** validação client-side, segurança de APIs e investigação forense de autenticação Linux.

Práticas documentadas:
- análise de aplicação web e validação client-side
- teste direto de API
- análise de `/var/log/auth.log`
- `grep` para falhas de autenticação
- `awk`, `sort` e `uniq` para identificar origens
- identificação de autenticação aceite
- reconstrução de linha temporal
- checklist de entrega do portfólio

Resultado do laboratório de logs documentado:
- IP de referência: `65.2.161.68`
- utilizador: `root`
- sessão interativa: `2024-03-06 06:32:45 UTC`

[📘 Abrir Sessão 02](sessao-02/README.md)

---

### Sessão 03 — Análise Forense de Servidores Linux e Persistência via systemd

**Foco:** persistência, serviços systemd, processos e análise de artefactos apagados.

Práticas documentadas:
- enumeração de serviços systemd
- investigação de `badr.service`
- processo com executável `(deleted)`
- identificação de `IpManager.service`
- análise de script executado como `root`
- utilização de `/proc/<PID>/exe`
- recuperação e validação da flag

Resultado da sala TryHackMe:
- **11 tarefas concluídas**
- **96 pontos**
- **Streak/Onda: 3**
- Flag validada: `[gh0st_1n_the_machine]`

[📘 Abrir Sessão 03](sessao-03/README.md)

---

## Progresso documentado

| Sessão | Tema | Estado |
|---|---|---|
| 01 | Linux, rede e Nmap | ✅ Concluída |
| 02 | APIs, autenticação e logs | ✅ Concluída |
| 03 | Forensics e persistência systemd | ✅ Concluída |

## Linha de aprendizagem

```text
Sessão 01
Linux + Rede + Nmap
        ↓
Sessão 02
Web + API + auth.log + investigação
        ↓
Sessão 03
Forensics + systemd + persistência
```

## Objetivo do portfólio

Registar as práticas do percurso de forma reproduzível, mantendo:

- comandos utilizados;
- evidências observadas;
- interpretação técnica;
- resultados;
- lições aprendidas;
- estado de cada laboratório.

**Percurso documentado até à Sessão 03.**
