# Laboratório — Sessão 4
## Gestão Segura de Acessos Remotos SSH em Linux

**Curso:** Reskilling  
**Módulo:** Linux e Cibersegurança  
**Objetivo de Aprendizagem:** OA4 · Aplicar  
**Duração da prática guiada:** 19:15 – 20:50  
**Formador:** Péricles Borges  
**Autor / Formando:** Marcos dos Santos

## 1. Contexto

Proteger o canal de gestão remota do servidor Ubuntu, eliminando a autenticação tradicional por password e migrando para autenticação criptográfica por chave SSH.

## 2. Ambiente Virtual

- **TryHackMe — Linux Strength Training:** https://tryhackme.com/room/linuxstrengthtraining
- **KillerCoda Ubuntu Playground:** https://killercoda.com/playgrounds/scenario/ubuntu

## 3. Tarefas do laboratório

### 3.1 Criar utilizador de teste

Foi criado o utilizador de laboratório `teste`.

```bash
sudo adduser teste
id teste
```

Resultado confirmado:

```text
uid=1001(teste) gid=1001(teste) groups=1001(teste)
```

### 3.2 Gerar um par de chaves Ed25519

```bash
ssh-keygen -t ed25519
```

A chave foi criada no ambiente de laboratório com o par:

```text
/root/.ssh/id_ed25519
/root/.ssh/id_ed25519.pub
```

**Nota de segurança:** a chave privada não é publicada no repositório.

### 3.3 Instalar a chave pública

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub teste@172.30.1.2
```

O comando informou que a chave já existia no servidor. A presença da chave foi posteriormente confirmada em:

```bash
sudo cat /home/teste/.ssh/authorized_keys
```

O ficheiro continha uma entrada `ssh-ed25519`.

### 3.4 Configurar o SSH

As diretivas aplicadas no `/etc/ssh/sshd_config` foram:

```text
Port 2222
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
```

Como o ambiente utiliza ativação por socket, também foi criado um drop-in para `ssh.socket`:

```ini
[Socket]
ListenStream=
ListenStream=0.0.0.0:2222
ListenStream=[::]:2222
```

### 3.5 Validar a configuração

```bash
sudo sshd -t
```

**Resultado real:** nenhum erro apresentado.

A configuração efetiva foi confirmada com:

```bash
sudo sshd -T | grep -E '^(port|permitrootlogin|passwordauthentication|pubkeyauthentication)'
```

Resultado:

```text
port 2222
permitrootlogin no
pubkeyauthentication yes
passwordauthentication no
```

### 3.6 Reiniciar e verificar o serviço

Foram executados:

```bash
sudo systemctl daemon-reload
sudo systemctl restart ssh.socket
sudo systemctl restart ssh
```

Verificação:

```bash
ss -tuln | grep -E ':(22|2222)\b'
```

Resultado observado:

```text
tcp   LISTEN 0      4096   0.0.0.0:2222   0.0.0.0:*
tcp   LISTEN 0      4096      [::]:2222      [::]:*
```

O serviço SSH permaneceu ativo e os logs registaram:

```text
Server listening on 0.0.0.0 port 2222.
Server listening on :: port 2222.
```

### 3.7 Testar o novo acesso por chave

```bash
ssh -i ~/.ssh/id_ed25519 -p 2222 teste@172.30.1.2
```

Login confirmado:

```bash
whoami
```

Resultado:

```text
teste
```

A variável de ambiente `SSH_CONNECTION` confirmou a utilização da porta `2222`.

### 3.8 Testar a desativação da autenticação por password

Foi executado:

```bash
ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no -p 2222 teste@172.30.1.2
```

Resultado observado:

```text
Permission denied (publickey,keyboard-interactive).
```

Este resultado confirma que a autenticação por password foi rejeitada.

## 4. Estado final da configuração

| Controlo | Estado |
|---|---|
| Porta SSH 22 | desativada |
| Porta SSH 2222 | ativa |
| Login direto de root | desativado |
| Autenticação por password | desativada |
| Autenticação por chave pública | ativa |
| Chave utilizada | Ed25519 |
| Login de teste | confirmado |

## 5. Critérios de entrega

- [x] Copiar as linhas modificadas do `sshd_config`
- [x] Validar a configuração com `sshd -t`
- [x] Comprovar a escuta na porta 2222
- [x] Comprovar login bem-sucedido via chave criptográfica
- [x] Comprovar rejeição da autenticação por password
- [x] Não publicar chave privada nem credenciais

## 6. Conclusão

A Sessão 4 foi executada e validada no laboratório.

A configuração SSH foi migrada de:

```text
porta 22
PermitRootLogin yes
PasswordAuthentication yes
```

para:

```text
porta 2222
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

O acesso foi testado com sucesso através de chave Ed25519, enquanto uma tentativa explícita de autenticação por password foi rejeitada.

## 7. Materiais Moodle

**Curso:** R1-M5 — Linux e Cibersegurança — Ed1  
https://elearning.skodjidigital.cv/course/view.php?id=23

**Slides da Sessão 4:**  
https://elearning.skodjidigital.cv/mod/resource/view.php?id=850

**Pré-Teste:**  
https://elearning.skodjidigital.cv/mod/quiz/view.php?id=835

**Pós-Teste:**  
https://elearning.skodjidigital.cv/mod/quiz/view.php?id=851

**Enunciado Lab 4:**  
https://elearning.skodjidigital.cv/mod/resource/view.php?id=864

**Submissão — GitHub Trabalho:**  
https://elearning.skodjidigital.cv/mod/assign/view.php?id=865

## 8. Segurança e boas práticas

1. Nunca publicar a chave privada no GitHub.
2. Não publicar passwords, tokens ou outras credenciais.
3. Validar `sshd_config` com `sshd -t` antes de reiniciar o serviço.
4. Manter uma sessão administrativa aberta até o novo acesso remoto ser comprovado.
5. Documentar apenas as evidências necessárias para avaliação.

---

**Autor / Formando:** Marcos dos Santos