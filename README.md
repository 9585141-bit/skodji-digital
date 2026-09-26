# Skodji Digital

Portfólio de aprendizagem e projetos do percurso **Reskilling Digital — Linux e Cibersegurança**.

**Autor / Formando:** Marcos dos Santos

## Organização do portfólio

Cada sessão possui o seu próprio diretório e `README.md`, com objetivo, contexto, procedimentos, evidências, resultados, lições aprendidas e estado da atividade.

```text
skodji-digital/
├── README.md
├── sessao-01/
│   └── README.md
├── sessao-02/
│   └── README.md
└── sessao-03/
    └── README.md
```

---

## Sessão 01 — Linux, Rede e Nmap

**Tema:** Introdução ao Linux para Segurança e Comandos de Rede.

**Foco:**
- fundamentos Linux;
- identificação de interfaces e endereços;
- sockets e portas em escuta;
- reconhecimento de serviços;
- Nmap e deteção de versões;
- análise inicial da superfície de exposição.

**Principais práticas:** `ip a` · `ss -tuln` · `nmap -sV -sC`

**Estado:** ✅ Concluída

📘 **[Abrir o trabalho da Sessão 01](sessao-01/README.md)**

---

## Sessão 02 — Web, APIs, Autenticação e Logs

**Tema:** Validação client-side, segurança de APIs e auditoria de sistemas Linux.

**Foco:**
- análise de aplicação web;
- identificação de validação no lado do cliente;
- teste direto de API em ambiente de laboratório;
- análise de `/var/log/auth.log`;
- identificação de tentativas de autenticação;
- reconstrução de uma linha temporal de eventos.

**Principais práticas:** `curl` · `nmap` · `grep` · `awk` · `sort` · `uniq`

**Estado:** ✅ Concluída

📘 **[Abrir o trabalho da Sessão 02](sessao-02/README.md)**

---

## Sessão 03 — Forensics e Persistência via systemd

**Tema:** Análise Forense de Servidores Linux e Persistência via systemd.

**Foco:**
- enumeração de serviços;
- investigação de serviços suspeitos;
- análise de processos;
- identificação de executáveis apagados;
- investigação de unidades systemd;
- análise de scripts executados por serviços;
- construção da cadeia forense.

**Principais práticas:** `systemctl` · `readlink` · `/proc/<PID>/exe` · análise de unidades e scripts

**Estado:** ✅ Concluída

📘 **[Abrir o trabalho da Sessão 03](sessao-03/README.md)**

---

## Estado do percurso

| Sessão | Área principal | Estado |
|---|---|---|
| 01 | Linux, rede e Nmap | ✅ Concluída |
| 02 | Web, APIs, autenticação e logs | ✅ Concluída |
| 03 | Forensics e systemd | ✅ Concluída |

## Linha de aprendizagem

```text
Sessão 01
Linux + Rede + Nmap
        ↓
Sessão 02
Web + API + Autenticação + Logs
        ↓
Sessão 03
Forensics + systemd + Persistência
```

## Objetivo do portfólio

Documentar o percurso de forma organizada e reproduzível, preservando:

- conhecimento adquirido;
- procedimentos praticados;
- evidências e resultados;
- interpretação técnica;
- lições aprendidas;
- evolução de cada sessão;
- histórico das atividades no Git.

## Relação com a aprendizagem prática

O computador pessoal foi utilizado como laboratório complementar para praticar comandos, testar conceitos e continuar a documentação durante períodos de espera do TryHackMe.

Os resultados específicos dos laboratórios remotos são identificados dentro das respetivas sessões.

## Nota sobre a evolução da documentação

A documentação pode ser atualizada à medida que novas evidências, outputs, explicações ou correções sejam recuperados. O objetivo é manter o Git como **registo vivo do percurso de aprendizagem**, e não apenas como depósito dos trabalhos finais.

---

**Percurso documentado até à Sessão 03.**