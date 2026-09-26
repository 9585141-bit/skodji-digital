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


## Nota sobre o ambiente de estudo

Durante o percurso, o computador pessoal **BITYJR** foi utilizado várias vezes como laboratório complementar para executar comandos, testar conceitos e continuar a documentação prática. Isto foi especialmente necessário em períodos em que o ambiente gratuito do TryHackMe impunha um período de espera de até **24 horas** para voltar a utilizar determinadas máquinas/laboratórios.

Assim, o portfólio distingue entre resultados recolhidos diretamente no TryHackMe e exercícios/práticas realizados no computador pessoal, mantendo o objetivo de continuar a aprendizagem mesmo durante os períodos de espera da plataforma.


## Nota sobre o percurso e a entrega dos trabalhos

O atraso na entrega de alguns trabalhos esteve relacionado, principalmente, com a falta de um computador durante parte da formação. Nesse período, as aulas eram acompanhadas essencialmente através do **telemóvel**, o que por vezes criava limitações para executar comandos, trabalhar com ambientes Linux, utilizar ferramentas de laboratório e organizar a documentação técnica.

Posteriormente, foi possível adquirir um **computador próprio**, no qual foi preparado um ambiente adequado para a prática e para o desenvolvimento do percurso da **Skodji Digital**, incluindo ferramentas e ambientes necessários para os exercícios de Linux, redes, cibersegurança e documentação em Git.

Com esse novo ambiente, os conteúdos foram revistos de forma prática e os trabalhos que estavam pendentes foram retomados, permitindo consolidar os conceitos estudados nas aulas e organizar os resultados no portfólio.

Esta documentação procura, assim, registar não apenas os resultados finais, mas também a evolução do processo de aprendizagem e a transição de um estudo limitado ao telemóvel para um ambiente técnico mais completo e adequado à formação.


---

**Autor / Formando:** Marcos dos Santos
