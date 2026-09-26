# Laboratório — Sessão 6 (Desafio MiniCTF)
## Desafio Prático Integrador — Mini-CTF Defensivo Linux

**Curso:** Reskilling  
**Módulo:** Linux e Cibersegurança  
**Objetivo de Aprendizagem:** Integração de OA1 a OA5 (foco em Criar)  
**Duração da prática:** 19:15 – 20:50 (Hackathon Defensivo — Partes 1 e 2)  
**Formador:** Péricles Borges  
**Peso na avaliação:** 65% da nota final (Portfólio GitHub)

## 1. Cenário

O servidor Ubuntu da empresa fictícia **Linux Agency** apresenta indícios de atividade suspeita e configurações severamente inseguras.

A missão é auditar, conter os danos, aplicar as correções e documentar toda a intervenção, como num cenário controlado de resposta a um incidente.

## 2. Ambiente Virtual

- **TryHackMe — Linux Agency** (gratuito): https://tryhackme.com/room/linuxagency
- **TryHackMe — Linux Incident Surface** (gratuito, alternativa): https://tryhackme.com/room/linuxincidentsurface

Uma das duas salas deve ser utilizada como base do desafio, de acordo com a orientação do formador.

## 3. Metodologia de Resposta — Roteiro de Ações Exigidas

### Fase 1 — Identificação e Triagem

#### 3.1 Análise de rede e portas

Identificar quais os portos e serviços ativos que estão expostos desnecessariamente.

```bash
ss -tuln
nmap -sV localhost
```

#### 3.2 Auditoria de contas

Procurar utilizadores com permissões excessivas, contas sem palavra-passe associada ou chaves públicas suspeitas em `authorized_keys`.

```bash
sudo cat /etc/shadow | awk -F: '($2==""){print $1}'
cat ~/.ssh/authorized_keys
```

Os resultados devem ser analisados e documentados como evidências do estado inicial.

### Fase 2 — Contenção

Ativar a firewall UFW e bloquear as portas de entrada que não sejam estritamente necessárias para o negócio.

```bash
sudo ufw default deny incoming
sudo ufw allow 22/tcp
sudo ufw enable
```

Registar as regras efetivamente aplicadas e guardar evidência do estado da firewall.

### Fase 3 — Enrijecimento / Remediação

Corrigir a configuração SSH de acordo com as boas práticas:

- desativar login root;
- bloquear autenticação por password;
- migrar para chaves criptográficas.

Aplicar também os patches de segurança relevantes identificados durante a triagem.

### Validação

Executar o Lynis para verificar a melhoria da postura de segurança global do host:

```bash
sudo lynis audit system
```

Registar o resultado final e o score pós-hardening.

## 4. Critérios de Entrega Obrigatória

O trabalho final deve ser um **Relatório Técnico de Auditoria e Mitigação em Markdown (`README.md`)**, estruturado pelas fases:

```text
Identificação
      ↓
Contenção
      ↓
Remediação
      ↓
Validação
```

Também devem ser incluídos:

- ficheiros de configuração corrigidos, incluindo cópia limpa do `sshd_config`;
- regras UFW exportadas;
- publicação do ecossistema completo de evidências no portfólio individual GitHub para avaliação formal do formador.

## 5. Checklist de Submissão — Portfólio GitHub

- [ ] Criar/atualizar `sessao-06/README.md` com o Relatório Técnico completo
- [ ] Documentar Identificação → Contenção → Remediação → Validação
- [ ] Incluir `sessao-06/sshd_config` (cópia limpa, sem dados sensíveis)
- [ ] Incluir `sessao-06/ufw-rules.txt` (output de `ufw status verbose`)
- [ ] Incluir excerto do relatório Lynis final (score pós-hardening)
- [ ] Confirmar que o repositório está público ou partilhado com o formador
- [ ] Fazer commit e push final antes do prazo de submissão

> **Nota de evidência:** resultados, scores, utilizadores, portas, regras e outputs devem corresponder à execução real do laboratório. Não serão inventados.

---

# Resultados Práticos

## 6. Fase 1 — Identificação e Triagem

### 6.1 Rede e portas

#### `ss -tuln`

```text
A preencher com o output real.
```

#### `nmap -sV localhost`

```text
A preencher com o output real.
```

### 6.2 Auditoria de contas

#### Contas sem password

```text
A preencher com o output real do comando de auditoria.
```

#### Chaves autorizadas

```text
A preencher com o conteúdo relevante de authorized_keys, removendo dados sensíveis quando necessário.
```

## 7. Fase 2 — Contenção

### Estado da firewall

Comandos principais:

```bash
sudo ufw default deny incoming
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status verbose
```

### Evidência

```text
A preencher com o output real de ufw status verbose.
```

## 8. Fase 3 — Enrijecimento / Remediação

### Configuração SSH

Registar as linhas efetivamente modificadas no `sshd_config`.

```text
A preencher com a configuração efetivamente aplicada.
```

### Patches

**Medidas aplicadas:** a preencher com os patches/correções efetivamente realizados.

## 9. Validação com Lynis

```bash
sudo lynis audit system
```

### Score pós-hardening

**Hardening Score final:** a preencher com o resultado real.

### Evidência Lynis

```text
A preencher com o excerto real do relatório final.
```

## 10. Ficheiros obrigatórios

### `sessao-06/sshd_config`

Deve conter uma cópia limpa da configuração utilizada, sem chaves privadas, passwords ou outros dados sensíveis.

**Estado:** ⏳ A aguardar configuração real.

### `sessao-06/ufw-rules.txt`

Deve conter o output real de:

```bash
sudo ufw status verbose
```

**Estado:** ⏳ A aguardar output real.

## 11. Relatório Técnico Final

O relatório deverá ligar cada evidência à respetiva ação:

```text
evidência encontrada
       ↓
problema identificado
       ↓
risco
       ↓
medida de contenção
       ↓
correção
       ↓
validação
       ↓
estado final
```

## 12. Estado da Sessão

**Sessão 6 — Enunciado oficial integrado. Estrutura do relatório criada; resultados e ficheiros de evidência aguardam integração das execuções reais.**

## 13. Segurança do portfólio

- Nunca publicar chaves privadas.
- Remover passwords, tokens e outros dados sensíveis dos ficheiros publicados.
- Utilizar apenas dados necessários para comprovar o trabalho.
- Manter as evidências identificadas como laboratório controlado.

---

## 14. Materiais da formação

**Sessão 5:** https://elearning.skodjidigital.cv/course/section.php?id=350

**Inquérito de Reação do Formando:** https://elearning.skodjidigital.cv/course/section.php?id=414

### Slide da Sessão 6

https://elearning.skodjidigital.cv/course/section.php?id=400

**Slide 6:** https://elearning.skodjidigital.cv/mod/resource/view.php?id=914

### Laboratório Final

https://elearning.skodjidigital.cv/course/section.php?id=399

**Enunciado Lab 6:** https://elearning.skodjidigital.cv/mod/resource/view.php?id=910

**Submissão:** https://elearning.skodjidigital.cv/mod/assign/view.php?id=911

---

**Autor / Formando:** Marcos dos Santos