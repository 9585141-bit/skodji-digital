# Laboratório — Sessão 6 (Mini-CTF Defensivo Linux)

## Desafio Prático Integrador — Mini-CTF Defensivo Linux

**Curso:** Reskilling  
**Módulo:** Linux e Cibersegurança  
**Objetivo de Aprendizagem:** Integração de OA1 a OA5  
**Formador:** Péricles Borges  
**Peso na avaliação:** 65% da nota final  
**Autor / Formando:** Marcos dos Santos

> **Nota:** esta documentação regista a execução real de uma reprodução local/adaptação do laboratório em Ubuntu 24.04.5 no WSL2, separada da tentativa do TryHackMe Linux Agency. Não são inventados resultados.

---

## 1. Metodologia

```text
Identificação / Triagem
          ↓
Contenção
          ↓
Remediação
          ↓
Validação
```

---

## 2. Fase 1 — Identificação e Triagem

### 2.1 Rede e portas

### `ss -tuln`

Foi identificada uma exposição adicional de SSH na porta `22`, além do SSH do laboratório na porta `2222`.

Principais portas TCP observadas:

```text
0.0.0.0:22      LISTEN
0.0.0.0:2222    LISTEN
[::]:22         LISTEN
[::]:2222       LISTEN
```

Também foram observados serviços DNS locais nas portas 53.

### `nmap -sV localhost`

Resultado real:

```text
22/tcp   open  ssh   OpenSSH 10.2p1 Ubuntu 2ubuntu3.5
2222/tcp open  ssh   OpenSSH 9.6p1 Ubuntu 3ubuntu13.19
```

A porta `2222` corresponde ao SSH do BITY-LAB. A porta `22` permaneceu visível, mas não apareceu associada ao `ssh.service` local através do `systemctl list-sockets`, indicando uma camada adicional da infraestrutura WSL. A origem da porta `22` não foi alterada neste laboratório para evitar uma intervenção não controlada.

### 2.2 Auditoria de contas

#### Contas sem password

Comando:

```bash
sudo cat /etc/shadow | awk -F: '($2==""){print $1}'
```

Resultado:

```text
Nenhuma conta foi apresentada pelo comando.
```

#### Chaves autorizadas

Foi encontrada uma chave pública Ed25519 em `~/.ssh/authorized_keys` para o utilizador do laboratório. A chave privada não foi publicada nem incluída neste portfólio.

### 2.3 Estado inicial de segurança

A triagem mostrou:

- SSH inicialmente disponível também na porta 22;
- SSH do laboratório migrado para a porta 2222;
- autenticação por chave validada;
- nenhuma conta com campo de password vazio;
- firewall UFW inicialmente inativa.

---

## 3. Fase 2 — Contenção

A política da UFW foi configurada para negar ligações de entrada por omissão e permitir saída.

Configuração efetivamente aplicada:

```text
Default: deny (incoming)
Default: allow (outgoing)
Default: disabled (routed)
Logging: on (low)
```

Como o SSH do laboratório utiliza a porta `2222`, foi criada a exceção:

```text
2222/tcp   ALLOW IN   Anywhere
2222/tcp (v6)   ALLOW IN   Anywhere (v6)
```

### Validação

Com UFW ativo, o acesso SSH por chave continuou a funcionar:

```text
whoami
bitylab

SSH_CONNECTION
127.0.0.1 44356 127.0.0.1 2222
```

Evidências gravadas no laboratório:

- `ufw-rules.txt`
- `ss.txt`
- `nmap.txt`

---

## 4. Fase 3 — Enrijecimento / Remediação

### 4.1 Hardening SSH

Configuração efetiva do SSH do BITY-LAB:

```text
Port 2222
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
Subsystem sftp internal-sftp
```

Também foi validado pelo `sshd -T`:

```text
port 2222
permitrootlogin no
pubkeyauthentication yes
passwordauthentication no
```

A ligação de teste por chave foi bem-sucedida.

### 4.2 Patches

Antes da atualização:

```text
43 packages can be upgraded
29 standard LTS security updates
```

Foi executado:

```bash
sudo apt upgrade -y
```

Resultado final:

```text
43 upgraded
0 newly installed
0 to remove
0 not upgraded
```

Após a atualização:

```text
apt list --upgradable
Listing...
```

Ou seja, não permaneceram pacotes atualizáveis segundo essa verificação.

---

## 5. Fase 4 — Validação com Lynis

### Resultado final

```text
Lynis version:      3.0.9
Ubuntu:              24.04
Tests performed:    250
Hardening index:    66
Warnings:           0
Suggestions:        49
Firewall:           ACTIVE
Malware scanner:    NOT INSTALLED
```

### Interpretação

A auditoria final registou **0 warnings**. Permaneceram 49 sugestões, entre elas recomendações sobre:

- hardening adicional do SSH;
- permissões de ficheiros;
- password aging e umask;
- auditd e file integrity;
- proteção de serviços systemd;
- configuração de AppArmor;
- instalação de ferramentas adicionais de segurança.

O Lynis também registou uma exceção `PKGS-7410`, informando que não encontrou pacotes de kernel através do gestor de pacotes. Este comportamento foi registado como parte do ambiente WSL2 e não tratado como falha do hardening SSH/UFW.

---

## 6. Resumo das fases

### Identificação

```text
✅ ss -tuln
✅ nmap -sV localhost
✅ auditoria de /etc/shadow
✅ verificação de authorized_keys
✅ identificação da exposição nas portas 22 e 2222
```

### Contenção

```text
✅ UFW ativado
✅ deny incoming por omissão
✅ 2222/tcp permitida
✅ acesso SSH validado com firewall ativo
```

### Remediação

```text
✅ root login desativado
✅ password authentication desativada
✅ autenticação por chave validada
✅ 43 pacotes atualizados
✅ 0 pacotes restantes no apt list --upgradable
```

### Validação

```text
✅ Lynis executado após as correções
✅ Hardening Index: 66
✅ 0 warnings
✅ 49 sugestões registadas
```

---

## 7. Evidências publicadas

```text
sessao-06/
├── README.md
├── sshd_config
├── ufw-rules.txt
├── ss.txt
├── nmap.txt
└── lynis-final.txt
```

Os ficheiros não devem conter passwords, chaves privadas, tokens ou outros dados sensíveis.

---

## 8. Relação com o TryHackMe

Foi realizada uma tentativa separada no **TryHackMe Linux Agency**. O ambiente remoto ficou posteriormente inacessível durante a prática. Por isso, os resultados deste README correspondem explicitamente ao **laboratório local BITY-LAB**, não a uma execução completa da máquina remota do TryHackMe.

---

## 9. Materiais da formação

**Sessão 5:** https://elearning.skodjidigital.cv/course/section.php?id=350  
**Slide 6:** https://elearning.skodjidigital.cv/mod/resource/view.php?id=914  
**Enunciado Lab 6:** https://elearning.skodjidigital.cv/mod/resource/view.php?id=910  
**Submissão:** https://elearning.skodjidigital.cv/mod/assign/view.php?id=911

---

## 10. Estado da Sessão

**Sessão 6 — reprodução local executada, evidências recolhidas e validação final realizada.**
