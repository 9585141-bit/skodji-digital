# Laboratório — Sessão 1
## Introdução ao Linux para Segurança e Comandos de Rede

**Curso:** Reskilling  
**Módulo:** Linux e Cibersegurança  
**Objetivo de Aprendizagem:** OA1 · Analisar  
**Duração da prática guiada:** 02:30 – 03:50 (1h20)  
**Formador:** Péricles Borges

## 1. Contexto

Mapeamento e análise da superfície de exposição de um servidor alvo na rede local.

Nesta sessão, assume-se o papel de **auditor de sistemas**: o objetivo é identificar a interface de rede do próprio ambiente, listar os serviços em escuta e, de seguida, mapear um alvo remoto com o Nmap.

## 2. Ambiente virtual

- **KillerCoda Ubuntu Playground** — terminal Linux gratuito no browser, sem instalação: https://killercoda.com/playgrounds/scenario/ubuntu
- **TryHackMe — Further Nmap** (gratuito): https://tryhackme.com/room/furthernmap

## 3. Tarefas a executar

1. Aceder ao KillerCoda Ubuntu Playground para familiarização com o CLI.
2. Executar o comando e identificar o endereço IP da interface principal:

```bash
ip a
```

3. Usar o comando para listar todos os portos abertos em escuta no ambiente local:

```bash
ss -tuln
```

4. Aceder à sala TryHackMe Further Nmap e iniciar a máquina alvo.
5. Executar um scan básico Nmap contra o alvo fornecido, com deteção de versões e scripts padrão:

```bash
nmap -sV -sC <IP_DO_ALVO>
```

## 4. Critérios de entrega

Documentar no portfólio:

- número de portas abertas identificadas;
- serviços em execução em cada porta;
- versões exatas detetadas pelo Nmap;
- output completo do comando `ip a`;
- output completo do comando `ss -tuln`.

## 5. Checklist de submissão — Portfólio GitHub

- [x] Criar/atualizar `sessao-01/README.md` com os resultados documentados
- [x] Incluir os outputs dos comandos em texto
- [x] Registar o resultado do Nmap
- [x] Fazer commit e push para o repositório do portfólio

---

# Resultados práticos

## 6. Ambiente utilizado no laboratório

- Arquitetura: `x86_64`
- Kernel: `6.8.0-138-generic`

## 7. Configuração de rede

### Comando

```bash
ip a
```

### Output

```text
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc fq_codel state UP group default qlen 1000
    link/ether 7e:24:2c:bc:b1:bc brd ff:ff:ff:ff:ff:ff
    inet 172.30.1.2/24 brd 172.30.1.255 scope global dynamic noprefixroute enp1s0
       valid_lft 86309209sec preferred_lft 75520009sec
    inet6 fe80::63d3:caba:7c67:ff63/64 scope link
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1454 qdisc fq_codel state DOWN group default qlen 1000
    link/ether 1a:fa:f7:44:2f:fc brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

## 8. Portas e sockets locais

### Comando

```bash
ss -tuln
```

### Output

```text
Netid  State   Recv-Q  Send-Q                        Local Address:Port    Peer Address:Port Process
udp    UNCONN  0       0                                127.0.0.54:53           0.0.0.0:*
udp    UNCONN  0       0                             127.0.0.53%lo:53           0.0.0.0:*
udp    UNCONN  0       0                                172.30.1.2:68           0.0.0.0:*
udp    UNCONN  0       0                         172.30.1.2%enp1s0:68           0.0.0.0:*
udp    UNCONN  0       0        [fe80::63d3:caba:7c67:ff63]%enp1s0:546             [::]:*
tcp    LISTEN  0       4096                          127.0.0.53:53           0.0.0.0:*
tcp    LISTEN  0       4096                             127.0.0.54:53           0.0.0.0:*
tcp    LISTEN  0       511                                 0.0.0.0:40205        0.0.0.0:*
tcp    LISTEN  0       128                                 0.0.0.0:40200        0.0.0.0:*
tcp    LISTEN  0       4096                                0.0.0.0:22           0.0.0.0:*
tcp    LISTEN  0       4096                              127.0.0.1:41857        0.0.0.0:*
tcp    LISTEN  0       4096                                      *:40300              *:*
tcp    LISTEN  0       4096                                      *:40305              *:*
tcp    LISTEN  0       4096                                   [::]:22              *:*
```

## 9. Scan Nmap

### Comando

```bash
nmap -sV -sC 172.30.1.2
```

### Output

```text
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-09-02 22:59 UTC
Nmap scan report for 172.30.1.2
Host is up (0.000005s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.18 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 79:14:d2:53:0b:21:37:c1:59:f6:ed:32:73:a0:22:3f (ECDSA)
|_  256 22:4b:c7:43:82:c9:d5:f8:f4:d4:ae:d5:a7:a1:ed:27 (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 0.55 seconds
```

## 10. Resultados

No scan básico do Nmap foram identificadas **1 porta TCP aberta** entre os 1000 principais portos verificados.

| Porta | Estado | Serviço | Versão detetada |
|---|---|---|---|
| `22/tcp` | `open` | SSH | `OpenSSH 9.6p1 Ubuntu 3ubuntu13.18` |

O resultado oficial do scan mostrou também **999 portas TCP fechadas** dentro do conjunto dos 1000 principais portos verificados.

## 11. Análise

O host `172.30.1.2` respondeu ao scan e apresentou a porta `22/tcp` aberta, com serviço SSH identificado pelo Nmap.

O comando `ss -tuln` mostrou outras portas em escuta localmente, incluindo `40200`, `40205`, `40300`, `40305` e `41857`. Estas observações locais não contradizem o scan básico: sem especificação de portas, o Nmap verifica por defeito os 1000 portos TCP mais comuns.

## 12. Conclusão

A atividade permitiu praticar a observação da configuração de rede, a identificação de sockets locais e o reconhecimento de portas, serviços e versões com Nmap.

O principal resultado do scan solicitado foi a identificação da porta `22/TCP` aberta, executando **OpenSSH 9.6p1 Ubuntu 3ubuntu13.18**.

## 13. Estado final

**Sessão 1 documentada no portfólio e alinhada com os critérios de entrega do laboratório.**

## 14. Nota sobre o ambiente de estudo

Parte da prática foi realizada no computador pessoal, utilizado como laboratório complementar para executar comandos Linux, testar conceitos de rede e manter o estudo ativo durante períodos de espera do TryHackMe.

---

**Autor / Formando:** Marcos dos Santos
