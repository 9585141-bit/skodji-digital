# Sessão 5 — Análise de Vulnerabilidades em Linux e Ferramentas de Auditoria

**Curso:** Reskilling — Linux e Cibersegurança  
**Objetivo de Aprendizagem:** OA5 — Criar  
**Duração:** 4 horas  
**Ambiente prático:** TryHackMe — Linux Process Analysis + KillerCoda Ubuntu Playground

## 1. Contexto

Como saber, de forma objetiva, se um sistema está bem protegido? Nesta sessão será utilizada a ferramenta **Lynis** para realizar uma auditoria automatizada do sistema Linux e interpretar o respetivo relatório.

O trabalho centra-se na leitura de:

- **Warnings**;
- **Suggestions**;
- **Hardening Score**.

Depois da auditoria, serão selecionadas **duas vulnerabilidades ou fragilidades críticas** identificadas no ambiente e serão propostas medidas corretivas.

## 2. Objetivo da sessão

Executar uma auditoria de segurança com Lynis, interpretar os resultados e produzir um mini-relatório técnico com o score inicial, os avisos encontrados e as medidas corretivas propostas.

## 3. Ambiente prático

- **TryHackMe — Linux Process Analysis**
- **KillerCoda Ubuntu Playground**: https://killercoda.com/playgrounds/scenario/ubuntu

## 4. Conteúdo principal

### 4.1 Auditoria automatizada

Instalar e executar o **Lynis** no ambiente Linux de laboratório.

### 4.2 Interpretação do relatório

Analisar o resultado produzido pela auditoria e identificar:

- score inicial de hardening;
- warnings apresentados;
- suggestions apresentadas;
- áreas prioritárias para correção.

### 4.3 Escolha de duas vulnerabilidades/fragilidades

Selecionar duas questões relevantes identificadas pelo Lynis e, para cada uma, registar:

**problema → risco/impacto → medida corretiva → forma de validação.**

## 5. Entrega esperada

No final da sessão, o GitHub deve conter um **mini-relatório técnico** com:

1. score inicial do Lynis;
2. principais warnings;
3. principais suggestions;
4. duas vulnerabilidades ou fragilidades selecionadas;
5. medidas corretivas propostas;
6. estado/validação das correções, quando aplicável.

## 6. Checklist do portfólio

- [ ] Instalar o Lynis no ambiente de laboratório
- [ ] Executar a auditoria
- [ ] Registar o score inicial
- [ ] Registar os warnings
- [ ] Registar as suggestions
- [ ] Selecionar duas vulnerabilidades/fragilidades críticas
- [ ] Propor medidas corretivas
- [ ] Registar a validação ou o estado da correção
- [ ] Incluir evidências reais no README
- [ ] Fazer commit e push para o GitHub

## 7. Resultados práticos

> Esta secção será preenchida com os resultados reais da execução do laboratório. Nenhum score, warning ou suggestion será inventado.

### 7.1 Score inicial

**Lynis Hardening Score:** a preencher com o valor real.

### 7.2 Warnings

Registar os warnings apresentados pelo Lynis, mantendo o texto essencial da ferramenta e acrescentando a interpretação técnica.

### 7.3 Suggestions

Registar as sugestões relevantes apresentadas pelo Lynis e indicar a prioridade de cada uma.

### 7.4 Duas vulnerabilidades/fragilidades escolhidas

#### Caso 1

**Problema:** a preencher.  
**Risco/impacto:** a preencher.  
**Medida corretiva proposta:** a preencher.  
**Validação:** a preencher.

#### Caso 2

**Problema:** a preencher.  
**Risco/impacto:** a preencher.  
**Medida corretiva proposta:** a preencher.  
**Validação:** a preencher.

## 8. Estrutura do mini-relatório

```text
Auditoria Lynis
      ↓
score inicial
      ↓
warnings + suggestions
      ↓
análise e priorização
      ↓
2 questões críticas
      ↓
medidas corretivas
      ↓
validação
      ↓
mini-relatório técnico
```

## 9. Materiais da formação

### Conteúdos da Sessão

https://elearning.skodjidigital.cv/course/section.php?id=392

### Slide 5

https://elearning.skodjidigital.cv/mod/resource/view.php?id=888

### Bibliografia complementar

**Vídeo — Lynis no Linux (Ubuntu): Descubra as falhas do seu sistema | Coffops**

https://elearning.skodjidigital.cv/mod/url/view.php?id=889

### Laboratório síncrono

**Secção:** https://elearning.skodjidigital.cv/course/section.php?id=394

**Enunciado Lab 5:** https://elearning.skodjidigital.cv/mod/resource/view.php?id=887

**Submissão de Lab 5:** https://elearning.skodjidigital.cv/mod/assign/view.php?id=891

## 10. Navegação do curso

**Sessão 4 — Gestão Segura de Acessos Remotos SSH em Linux:** https://elearning.skodjidigital.cv/course/section.php?id=349

**Sessão 6 — Desafio Prático Integrador:** https://elearning.skodjidigital.cv/course/section.php?id=351

## 11. Estado da sessão

**Sessão 5 — Estrutura documental criada. Resultados práticos aguardam a execução/recuperação das evidências reais do laboratório.**

---

**Autor / Formando:** Marcos dos Santos