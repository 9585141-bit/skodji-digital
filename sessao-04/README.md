# Laboratório — Sessão 4
## Gestão Segura de Acessos Remotos SSH em Linux

**Curso:** Reskilling  
**Módulo:** Linux e Cibersegurança  
**Objetivo de Aprendizagem:** OA4 · Aplicar  
**Duração da prática guiada:** 19:15 – 20:50  
**Formador:** Péricles Borges

## 1. Contexto

Proteger o canal de gestão remota do servidor Ubuntu, eliminando a autenticação tradicional por password e migrando para autenticação criptográfica.

## 2. Ambiente Virtual

- **TryHackMe — Linux Strength Training** (gratuito): https://tryhackme.com/room/linuxstrengthtraining
- **KillerCoda Ubuntu Playground:** https://killercoda.com/playgrounds/scenario/ubuntu

## 3. Tarefas a Executar

### 3.1 Criar utilizador de teste

Criar um novo utilizador de teste no sistema e configurar o ambiente para aceitar chaves.

### 3.2 Gerar um par de chaves Ed25519

```bash
ssh-keygen -t ed25519
```

### 3.3 Transferir a chave pública

```bash
ssh-copy-id <utilizador>@<IP_DO_SERVIDOR>
```

### 3.4 Editar a configuração do daemon SSH

```bash
sudo nano /etc/ssh/sshd_config
```

### 3.5 Aplicar as alterações solicitadas

```text
PermitRootLogin no
PasswordAuthentication no
Port 2222
```

### 3.6 Validar a sintaxe antes de reiniciar

```bash
sudo sshd -t
```

### 3.7 Reiniciar o serviço SSH

```bash
sudo systemctl restart sshd
```

### 3.8 Testar o novo acesso

Num novo terminal, testar o acesso utilizando a chave privada e a nova porta:

```bash
ssh -i <caminho_da_chave> -p 2222 <utilizador>@<IP>
```

## 4. Nota de Segurança

**Nunca fechar a sessão atual antes de confirmar que o novo acesso funciona.** Um erro de sintaxe ou configuração pode bloquear o acesso remoto ao servidor.

## 5. Critérios de Entrega

Documentar no portfólio:

- cópia das linhas modificadas do `sshd_config`;
- evidência de login bem-sucedido via chave criptográfica, através do output do terminal.

## 6. Checklist de Submissão — Portfólio GitHub

- [ ] Criar/atualizar `sessao-04/README.md` com os resultados documentados
- [ ] Incluir cópia limpa do `sshd_config` corrigido
- [ ] Incluir evidência de login bem-sucedido via chave
- [ ] Garantir que não existem chaves privadas ou dados sensíveis
- [ ] Fazer commit e push para o repositório do portfólio

---

# Resultados Práticos

## 7. Evidência de configuração

### Linhas modificadas no `sshd_config`

```text
PermitRootLogin no
PasswordAuthentication no
Port 2222
```

> As linhas acima correspondem à configuração solicitada pelo enunciado. A versão final deverá refletir o conteúdo efetivamente aplicado no laboratório.

## 8. Validação da configuração

### Comando

```bash
sudo sshd -t
```

**Resultado real:** a preencher com o output/registo obtido durante a execução.

## 9. Login via chave criptográfica

### Comando

```bash
ssh -i <caminho_da_chave> -p 2222 <utilizador>@<IP>
```

**Evidência real:** a preencher com o output ou captura do login bem-sucedido.

> **Segurança:** nunca colocar no GitHub a chave privada, conteúdo de ficheiros privados ou credenciais. A evidência deve demonstrar apenas o necessário para comprovar o acesso.

## 10. Estado da sessão

**Sessão 4 — Enunciado oficial integrado; resultados de execução aguardam inserção das evidências reais do laboratório.**

---

## 11. Materiais Moodle e navegação da sessão

**Curso:** R1-M5 — Linux e Cibersegurança — Ed1

https://elearning.skodjidigital.cv/course/view.php?id=23

**Sessão 3:** https://elearning.skodjidigital.cv/course/section.php?id=136

**Sessão 5:** https://elearning.skodjidigital.cv/course/section.php?id=350

**Slides da Sessão 4:** https://elearning.skodjidigital.cv/mod/resource/view.php?id=850

**Pré-Teste:** https://elearning.skodjidigital.cv/mod/quiz/view.php?id=835

**Pos-Teste:** https://elearning.skodjidigital.cv/mod/quiz/view.php?id=851

**Enunciado Lab4:** https://elearning.skodjidigital.cv/mod/resource/view.php?id=864

**Submissão — GitHub Trabalho:** https://elearning.skodjidigital.cv/mod/assign/view.php?id=865

**Autor / Formando:** Marcos dos Santos

## 8. Resultados práticos

> Esta secção será preenchida com os outputs e evidências reais da execução do laboratório, preservando a distinção entre o enunciado e aquilo que foi efetivamente testado.

### 8.1 Geração da chave

**Comando executado:** a preencher com o comando real utilizado.

### 8.2 Configuração do SSH

**Linhas modificadas:** a preencher com as linhas reais de `sshd_config`.

### 8.3 Validação

**Output de `sshd -t`:** a preencher com o resultado real.

### 8.4 Login por chave

**Evidência:** a preencher com o output/captura correspondente ao login SSH bem-sucedido.

## 9. Segurança e boas práticas

1. Nunca publicar a chave privada no GitHub.
2. Publicar apenas a informação necessária para demonstrar a configuração e o resultado.
3. Validar `sshd_config` com `sshd -t` antes de reiniciar o serviço.
4. Em documentação pública, remover endereços, utilizadores, chaves ou outros dados privados desnecessários.

## 10. Estado da sessão

**Sessão 4 — Estrutura documental criada; resultados práticos aguardam integração das evidências reais do laboratório.**

---

**Autor / Formando:** Marcos dos Santos