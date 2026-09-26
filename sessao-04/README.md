# Sessão 4 — Gestão Segura de Acessos Remotos SSH em Linux

**Curso:** Reskilling  
**Módulo:** Linux e Cibersegurança  
**Objetivo de Aprendizagem:** OA4 — Aplicar  
**Duração:** 4 horas  
**Formador:** Péricles Borges

## 1. Enquadramento

O SSH é um dos principais pontos de entrada para administração remota de servidores. Nesta sessão, o objetivo é reforçar a segurança do serviço SSH através de autenticação por chaves criptográficas, restrição do acesso root, desativação da autenticação por password e alteração da porta padrão.

## 2. Ambiente prático

- **TryHackMe — Linux Strength Training**
- **KillerCoda — Ubuntu Playground**: https://killercoda.com/playgrounds/scenario/ubuntu

## 3. Conteúdo da sessão

### 3.1 Geração de chaves Ed25519

Gerar um par de chaves criptográficas utilizando o algoritmo **Ed25519**, mantendo a chave privada protegida e utilizando a chave pública para autenticação.

### 3.2 Distribuição segura da chave pública

Instalar a chave pública no utilizador remoto através do mecanismo apropriado de chaves autorizadas, sem expor a chave privada.

### 3.3 Reconfiguração do sshd

A sessão trabalha a configuração do `/etc/ssh/sshd_config` para:

- eliminar a autenticação por password;
- bloquear o acesso root direto;
- alterar a porta padrão do SSH.

**Atenção:** um erro de sintaxe em `sshd_config` pode impedir novos acessos remotos. A configuração deve ser validada antes de reiniciar o serviço.

### 3.4 Validação da configuração

```bash
sshd -t
```

O comando deve ser utilizado para verificar a sintaxe antes do reinício do serviço SSH.

## 4. Evidências exigidas

No final da sessão, o portfólio GitHub deve apresentar:

- as linhas modificadas do `sshd_config`;
- evidência de login bem-sucedido através de chave SSH;
- registo da validação da configuração com `sshd -t`;
- explicação das alterações de segurança aplicadas.

## 5. Checklist do laboratório

- [ ] Gerar par de chaves Ed25519
- [ ] Instalar/distribuir a chave pública
- [ ] Configurar autenticação por chave
- [ ] Desativar autenticação por password
- [ ] Bloquear login root direto
- [ ] Alterar a porta padrão do SSH
- [ ] Validar `sshd_config` com `sshd -t`
- [ ] Reiniciar/recarregar o serviço após validação
- [ ] Confirmar login bem-sucedido via chave
- [ ] Registar as linhas alteradas do `sshd_config`

## 6. Materiais da formação

**Curso Moodle:** R1-M5 — Linux e Cibersegurança — Ed1

https://elearning.skodjidigital.cv/course/view.php?id=23

### Navegação da sessão

- Sessão 3 — Hardening de Redes Linux e Configuração de Firewalls: https://elearning.skodjidigital.cv/course/section.php?id=136
- Sessão 5 — Análise de Vulnerabilidades em Linux e Ferramentas de Auditoria: https://elearning.skodjidigital.cv/course/section.php?id=350

### Conteúdos e avaliação

- Pré-Teste: https://elearning.skodjidigital.cv/mod/quiz/view.php?id=835
- Slides da Sessão 4: https://elearning.skodjidigital.cv/mod/resource/view.php?id=850
- Pos-Teste: https://elearning.skodjidigital.cv/mod/quiz/view.php?id=851
- Dúvidas e Debates: https://elearning.skodjidigital.cv/mod/forum/view.php?id=838

### Bibliografia complementar

- Vídeo — SSH mais seguro: https://elearning.skodjidigital.cv/mod/url/view.php?id=839
- Vídeo — Controle o Linux de QUALQUER LUGAR com esta ferramenta: https://elearning.skodjidigital.cv/mod/url/view.php?id=840
- Vídeo — SSH SEGURO!: https://elearning.skodjidigital.cv/mod/url/view.php?id=842

## 7. Laboratório síncrono e submissão

- Enunciado Lab4 — Sessão 4: https://elearning.skodjidigital.cv/mod/resource/view.php?id=864
- Submissão — GitHub Trabalho: https://elearning.skodjidigital.cv/mod/assign/view.php?id=865

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