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
