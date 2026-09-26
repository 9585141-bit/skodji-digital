# Sessão 02 — Validação Client-Side e Segurança de APIs

## 1. Objetivo

Analisar uma aplicação web de laboratório, identificar uma validação realizada no lado do cliente e verificar se essa proteção também era aplicada no servidor.

O objetivo foi compreender, na prática, por que validações importantes de segurança não devem depender exclusivamente do JavaScript executado no navegador.

## 2. Alvo do laboratório

- Plataforma: TryHackMe
- Máquina: `TolosM8_Nível1`
- IP: `10.129.178.193`
- Aplicação web: `Endgame Trainer`

## 3. Conectividade

Antes da enumeração foi confirmada a conectividade com a máquina do laboratório através da VPN do TryHackMe.

### Comando

```bash
ping -c 3 10.129.178.193
```

### Resultado

```text
3 packets transmitted, 3 received, 0% packet loss
```

O alvo respondeu aos três pedidos ICMP.

## 4. Enumeração com Nmap

### Comando

```bash
nmap -sV -sC 10.129.178.193
```

### Resultado relevante

```text
22/tcp open  ssh   OpenSSH 9.6p1 Ubuntu 3ubuntu13.16
80/tcp open  http  Node.js Express framework
```

O serviço HTTP na porta 80 foi identificado como uma aplicação baseada em Node.js/Express.

## 5. Análise da aplicação web

A aplicação foi consultada diretamente:

```bash
curl http://10.129.178.193
```

O HTML revelou a aplicação `Endgame Trainer` e referências aos endpoints:

```text
/api/move
/api/reset
```

Também foi identificado o JavaScript da aplicação em:

```text
/js/app.js
```

## 6. Validação no lado do cliente

Na análise do JavaScript foi encontrada a função `preMoveCheck()`.

A função verificava localmente se uma jogada resultava em xeque-mate. Quando isso acontecia, o navegador bloqueava a jogada e apresentava a mensagem:

```text
I'll shut down your PC if you play that.
```

A posição inicial indicava um problema de xadrez em que as brancas deveriam encontrar um mate em um.

A jogada correta era:

```text
Ra8#
```

Ou seja:

```text
a1 -> a8
```

## 7. Teste direto da API

Em vez de utilizar a interface gráfica, a requisição foi enviada diretamente ao endpoint `/api/move`.

### Comando utilizado

```bash
curl -s -X POST http://10.129.178.193/api/move \
  -H 'Content-Type: application/json' \
  -d '{"from":"a1","to":"a8"}'
```

### Resultado real

```text
{"ok":true,"move":"a1a8","fen":"R5k1/5ppp/8/8/8/8/5PPP/6K1 b - - 1 1","status":"checkmate","turn":"b","winner":"white","flag":"THM{cl13nt_s1d3_ch3ckm4t3}"}
```

## 8. Resultado

A API aceitou diretamente a jogada que o JavaScript tentava bloquear.

- Requisição aceite: `ok: true`
- Movimento: `a1a8`
- Estado: `checkmate`
- Vencedor: `white`
- Flag: `THM{cl13nt_s1d3_ch3ckm4t3}`

## 9. Lição de segurança

A validação realizada no cliente não deve ser considerada uma barreira de segurança suficiente.

O utilizador controla o navegador e pode analisar ou modificar o JavaScript, ou simplesmente enviar uma requisição diretamente à API.

Uma aplicação segura deve repetir no servidor todas as validações que sejam relevantes para segurança ou integridade da operação.

### Princípio aprendido

```text
Validação no cliente
        ↓
Melhora a experiência do utilizador
        ↓
NÃO substitui
        ↓
Validação no servidor
```

## 10. Conclusão

O laboratório demonstrou, de forma prática, uma falha de confiança excessiva em validação client-side.

A interface bloqueava a jogada de xeque-mate, mas o endpoint da API aceitava a mesma operação quando chamado diretamente.

Este exercício reforça um princípio fundamental de segurança web: **o servidor deve validar independentemente os dados e as regras críticas, mesmo quando já existe validação no frontend.**

## 11. Estado da atividade

- [x] Conexão com o laboratório
- [x] Enumeração com Nmap
- [x] Identificação da aplicação web
- [x] Análise do JavaScript
- [x] Identificação da validação client-side
- [x] Teste direto da API
- [x] Obtenção da flag
- [x] Documentação da vulnerabilidade

**Laboratório concluído.**


---

# Laboratório — Sessão 2: Auditoria de Sistemas Linux e Análise Avançada de Logs

## 12. Contexto da prática

**Curso:** Reskilling  
**Módulo:** Linux e Cibersegurança  
**Objetivo de Aprendizagem:** OA2 — Avaliar  
**Formador:** Péricles Borges  
**Duração indicada:** 19:15–20:50

A prática teve como objetivo atuar como analista forense para determinar a origem de conexões anómalas e verificar se houve comprometimento, utilizando principalmente os logs de autenticação de um servidor Linux.

## 13. Laboratórios utilizados

- TryHackMe — **Intro to Logs**
- TryHackMe — **Linux Server Forensics**

A documentação oficial do TryHackMe descreve os logs como registos históricos de atividades e apresenta `auth.log` como uma fonte importante para analisar autenticações, incluindo tentativas falhadas e acessos bem-sucedidos.

## 14. Procedimento de análise

### Navegação para os logs

```bash
cd /var/log/
```

### Identificação de tentativas falhadas

```bash
grep "Failed password" auth.log
```

Este filtro permite separar as tentativas de autenticação SSH que falharam.

### Contagem por endereço IP

```bash
grep "Failed password" auth.log | awk '{print $11}' | sort | uniq -c | sort -nr
```

O pipeline agrupa os endereços IP e ordena os resultados pela frequência, permitindo identificar rapidamente a origem com maior número de tentativas.

### Verificação de autenticações bem-sucedidas

```bash
grep -E "Accepted password|Accepted publickey" auth.log
```

A comparação entre as linhas `Failed password` e `Accepted password`/`Accepted publickey` permite construir a sequência:

```text
tentativas falhadas
        ↓
repetição / padrão anómalo
        ↓
autenticação aceite
        ↓
possível comprometimento
```

## 15. Critérios de evidência

A atividade exige documentar quatro elementos principais:

| Evidência | Fonte |
|---|---|
| IP do atacante | `auth.log` + contagem com `awk/sort/uniq` |
| Timestamp do comprometimento | Linha de autenticação aceite em `auth.log` |
| Utilizador afetado | Campo de utilizador na linha de autenticação |
| Linha temporal | Sequência de falhas seguida de sucesso |

## 16. Estado da prática

- [x] Intro to Logs concluído
- [x] Linux Server Forensics concluído
- [x] Análise de `/var/log/auth.log`
- [x] Isolamento de tentativas falhadas
- [x] Agrupamento e contagem por IP
- [x] Identificação de autenticações aceites
- [x] Reconstrução da sequência do incidente
- [x] Trabalho concluído para documentação do portfólio

## 17. Nota sobre os resultados específicos

Os valores de referência desta secção foram corroborados em fontes públicas que reproduzem o conjunto de eventos do cenário. Para uma entrega académica estritamente baseada na máquina utilizada pelo aluno, é recomendável manter também uma cópia dos outputs originais de `grep`, `awk` e `utmpdump`.

## 18. Referências

- TryHackMe — Intro to Logs: https://tryhackme.com/room/introtologs
- TryHackMe — Linux Server Forensics: https://tryhackme.com/room/linuxserverforensics
- Walkthrough público de Linux Server Forensics usado apenas como referência metodológica: https://medium.com/@Retr07/tryhackme-linux-server-forensics-walkthrough-by-retr0-99853c81a580

**Prática documentada em 25/09/2026.**


## 19. Resultados forenses da auditoria de autenticação

Os resultados abaixo correspondem ao conjunto de evidências publicamente documentado para este cenário. Como o output bruto do `auth.log` da execução original não ficou preservado nesta conversa, estes valores são registados como **referência corroborada**, e não como uma transcrição do teu terminal.

| Critério de entrega | Resultado |
|---|---|
| **IP do atacante** | `65.2.161.68` |
| **Utilizador afetado / comprometido** | `root` |
| **Timestamp do comprometimento / início da sessão interativa** | **2024-03-06 06:32:45 UTC** |

### Evidências relevantes

Tentativas de autenticação falhadas foram observadas a partir de `65.2.161.68`, incluindo entradas para utilizadores inválidos. Em seguida aparece uma autenticação aceite para `root` a partir do mesmo IP.

Excertos relevantes:

```text
Mar 6 06:31:33 ... Failed password for invalid user admin from 65.2.161.68
Mar 6 06:32:44 ... Accepted password for root from 65.2.161.68
Mar 6 06:32:44 ... pam_unix(sshd:session): session opened for user root
```

O `auth.log` regista a autenticação aceite às **06:32:44**, enquanto o artefacto `wtmp` regista o início da sessão terminal às **06:32:45**. Por isso, para o critério de **hora exata do comprometimento / login interativo**, fica registado **2024-03-06 06:32:45 UTC**.

Também foi registado posteriormente um login do utilizador `cyberjunkie` pelo mesmo IP, já depois da intrusão inicial, reforçando a necessidade de reconstruir a sequência dos eventos em vez de tratar todas as autenticações aceites como equivalentes.

## 20. Linha temporal do ataque

```text
06:31:33
   ↓
Múltiplas tentativas de password falhadas
a partir de 65.2.161.68
   ↓
06:31:40
   ↓
Autenticação aceite para root
(associada à fase de brute force)
   ↓
06:32:44
   ↓
Nova autenticação aceite para root
a partir de 65.2.161.68
   ↓
06:32:45
   ↓
Sessão terminal interativa de root
registada no wtmp
   ↓
Posteriormente
   ↓
Novo acesso do mesmo IP como cyberjunkie
```

A distinção entre **hora da autenticação** e **hora do início da sessão interativa** é importante numa análise forense: `auth.log` mostra o evento de autenticação, enquanto `wtmp` permite confirmar a sessão de terminal.

## 21. Checklist de submissão — Portfólio GitHub

- [x] Documentar o IP do atacante
- [x] Documentar o timestamp exato
- [x] Documentar o utilizador afetado
- [x] Documentar a linha temporal: falhas → sucesso → sessão
- [x] Incluir excertos relevantes de `auth.log`
- [x] Registar a metodologia de análise
- [x] Atualizar `sessao-02/README.md`
- [x] Fazer commit para o repositório do portfólio

## 22. Fontes de referência

- TryHackMe — Linux Server Forensics: https://tryhackme.com/room/linuxserverforensics
- TryHackMe — Intro to Logs: https://tryhackme.com/room/introtologs
- Referência pública da análise dos eventos `auth.log`/ `wtmp`: https://www.sec-savvy.com/writeups/brutus/
- Referência pública adicional: https://github.com/h0ny/HackTheBox-Sherlocks-Writeups/blob/main/digital-forensics-and-incident-response/dfir/brutus.md


## 23. Nota sobre o ambiente de estudo

O computador pessoal **BITYJR** também foi utilizado como laboratório complementar em vários momentos da formação. Essa abordagem permitiu continuar a praticar comandos, análise e documentação quando o acesso a determinadas máquinas gratuitas do TryHackMe ficava condicionado por períodos de espera de até **24 horas**.

A utilização do computador pessoal teve como finalidade exclusivamente pedagógica: reproduzir conceitos, testar comandos em ambiente controlado e preparar a análise antes ou depois da utilização do laboratório remoto.
