# Laboratório — Sessão 5
## Análise de Vulnerabilidades em Linux e Ferramentas de Auditoria

**Curso:** Reskilling  
**Módulo:** Linux e Cibersegurança  
**Objetivo de Aprendizagem:** OA5 · Criar  
**Formador:** Péricles Borges  
**Autor / Formando:** Marcos dos Santos

## 1. Contexto

Utilização do Lynis para realizar uma auditoria automatizada de segurança no ambiente Linux, interpretar os resultados e aplicar correções controladas.

## 2. Ambiente

- TryHackMe — Linux Process Analysis: https://tryhackme.com/room/linuxprocessanalysis
- KillerCoda Ubuntu Playground: https://killercoda.com/playgrounds/scenario/ubuntu

## 3. Instalação do Lynis

Comandos executados como `root`:

```bash
apt update && apt install lynis -y
lynis --version
```

Resultado:

```text
3.0.9
```

## 4. Auditoria inicial

Comando:

```bash
lynis audit system
```

### 4.1 Resumo inicial

| Indicador | Resultado |
|---|---:|
| Versão Lynis | 3.0.9 |
| Sistema operacional | Ubuntu 24.04 |
| Kernel | 6.8.0 |
| Testes realizados | 262 |
| Hardening Index | 62 |
| Warnings | 1 |
| Suggestions | 48 |

### 4.2 Warning encontrado

```text
PKGS-7392 — Found one or more vulnerable packages.
```

O Lynis relacionou este warning à necessidade de atualizar o sistema/pacotes.

### 4.3 Principais áreas observadas

O relatório inicial apresentou, entre outras, sugestões relacionadas com autenticação, permissões de ficheiros, SSH, kernel, firewall e logging.

## 5. Duas questões selecionadas

### 5.1 AUTH-9286 — Password aging

**Área:** Authentication

O Lynis registou:

```text
password minimum age is not configured
password aging limits are not configured
```

Estado inicial observado em `/etc/login.defs`:

```text
PASS_MAX_DAYS       99999
PASS_MIN_DAYS       0
PASS_WARN_AGE       7
```

**Correção aplicada no laboratório:**

```text
PASS_MAX_DAYS       90
PASS_MIN_DAYS       1
PASS_WARN_AGE       7
```

A CISOfy descreve o AUTH-9286 como um controlo de *password aging* e relaciona a renovação periódica de passwords com a redução do risco associado a passwords fracas ou obtidas por terceiros. A própria página também ressalva que a necessidade de password aging depende da política e do modelo de autenticação do ambiente. citeturn391069search0

**Validação:** na segunda auditoria, o Lynis passou a reportar:

```text
User password aging (minimum) [ CONFIGURED ]
User password aging (maximum) [ CONFIGURED ]
```

Assim, o AUTH-9286 deixou de aparecer entre as Suggestions finais.

### 5.2 FILE-7524 — File permissions

**Área:** File Integrity

Na auditoria inicial, o Lynis encontrou permissões fora do baseline em:

```text
/etc/crontab                 644 != 600
/etc/ssh/sshd_config         644 != 600
/etc/cron.d                  755 != 700
/etc/cron.daily              755 != 700
/etc/cron.hourly             755 != 700
/etc/cron.weekly             755 != 700
/etc/cron.monthly            755 != 700
```

**Correção aplicada no laboratório:**

```bash
chmod 600 /etc/ssh/sshd_config
```

Validação:

```text
600 root:root /etc/ssh/sshd_config
```

A CISOfy define o FILE-7524 como um controlo de permissões esperado pelo perfil de auditoria e indica que, conforme o ficheiro analisado, deve ser verificado o motivo da diferença e corrigida a permissão quando apropriado. A página comunitária atual não fornece uma correção universal adicional. citeturn391069search1

**Resultado da segunda auditoria:** o `/etc/ssh/sshd_config` passou a `[ OK ]`, mas o `FILE-7524` continua como Suggestion devido às restantes permissões identificadas em `/etc/crontab` e nos diretórios `cron.*`.

Portanto, este achado é considerado **parcialmente tratado**, não totalmente encerrado.

## 6. Segunda auditoria e comparação

Após as correções, foi executado novamente:

```bash
lynis audit system
```

| Indicador | Inicial | Após correções |
|---|---:|---:|
| Hardening Index | 62 | 63 |
| Warnings | 1 | 1 |
| Suggestions | 48 | 46 |

### 6.1 Evidências da melhoria

- `AUTH-9286` deixou de aparecer nas Suggestions.
- `/etc/ssh/sshd_config` passou de `[ SUGGESTION ]` para `[ OK ]` no teste de permissões.
- O Hardening Index passou de **62 para 63**.
- O número de Suggestions passou de **48 para 46**.
- O warning `PKGS-7392` permaneceu presente, indicando que ainda existem pacotes identificados pelo Lynis como vulneráveis.

## 7. Evidência do terminal

### Auditoria inicial

```text
Warnings (1)
Suggestions (48)
Hardening index : 62
Tests performed : 262
```

### Segunda auditoria

```text
Warnings (1)
Suggestions (46)
Hardening index : 63
Tests performed : 262
```

## 8. Relatórios gerados pelo Lynis

```text
/var/log/lynis.log
/var/log/lynis-report.dat
```

Os relatórios foram utilizados para confirmar os resultados e comparar o estado do sistema antes e depois das correções.

## 9. Mini-relatório técnico

### Situação inicial

O sistema apresentou Hardening Index 62, com 1 Warning e 48 Suggestions. Entre os pontos selecionados estavam a ausência de configuração de password aging (`AUTH-9286`) e permissões fora do baseline em ficheiros/diretórios (`FILE-7524`).

### Ações realizadas

Foi definida uma política de password aging no `/etc/login.defs` e ajustada a permissão do `/etc/ssh/sshd_config` para `600`.

### Resultado

A segunda auditoria apresentou Hardening Index 63, 1 Warning e 46 Suggestions. O `AUTH-9286` foi eliminado do conjunto de Suggestions e a permissão do `sshd_config` passou para o estado esperado pelo teste. O `FILE-7524` permanece parcialmente tratado devido aos restantes alvos de permissões identificados.

## 10. Segurança e boas práticas

1. As correções foram aplicadas no ambiente de laboratório.
2. Não foram publicados passwords, tokens ou chaves privadas.
3. Os valores do Lynis foram transcritos a partir da execução real.
4. A diferença entre problema encontrado, correção aplicada e estado final foi mantida explícita.

## 11. Checklist de submissão — Portfólio GitHub

- [x] Criar/atualizar `sessao-05/README.md` com o mini-relatório técnico
- [x] Registar o Hardening Score inicial
- [x] Registar Warnings
- [x] Registar Suggestions
- [x] Selecionar duas questões de Authentication / Filesystem
- [x] Documentar as correções realizadas
- [x] Incluir evidências reais do Lynis
- [x] Registar a comparação antes/depois
- [x] Fazer commit e push para o repositório do portfólio

## 12. Materiais da formação

**Curso:** https://elearning.skodjidigital.cv/course/view.php?id=23

**Conteúdos da Sessão:** https://elearning.skodjidigital.cv/course/section.php?id=392

**Slide 5:** https://elearning.skodjidigital.cv/mod/resource/view.php?id=888

**Laboratório síncrono:** https://elearning.skodjidigital.cv/course/section.php?id=394

**Enunciado Lab 5:** https://elearning.skodjidigital.cv/mod/resource/view.php?id=887

**Submissão Lab 5:** https://elearning.skodjidigital.cv/mod/assign/view.php?id=891

---

**Autor / Formando:** Marcos dos Santos