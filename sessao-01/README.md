# Sessão 01 — Introdução ao Linux para Segurança e Comandos de Rede

## 1. Objetivo

Executar uma análise básica da superfície de exposição de um alvo Linux, utilizando comandos de rede e o Nmap para identificar portas abertas, serviços e versões.

## 2. Ambiente local

- Hostname: `ubuntu`
- Utilizador: `root`
- Arquitetura: `x86_64`
- Kernel: `6.8.0-138-generic`

## 3. Configuração de rede

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

## 4. Portas e sockets locais

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

## 5. Scan Nmap

### Comando solicitado

```bash
nmap -sV -sC 172.30.1.2
```

### Output completo

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

## 6. Resultados

### Número de portas abertas

No scan básico do Nmap foram identificadas **1 porta TCP aberta** entre os 1000 principais portos verificados.

### Serviço em execução

| Porta | Estado | Serviço | Versão detetada |
|---|---|---|---|
| `22/tcp` | `open` | SSH | `OpenSSH 9.6p1 Ubuntu 3ubuntu13.18` |

### Scripts padrão

O script padrão do Nmap identificou duas chaves de host SSH:

- ECDSA
- ED25519

## 7. Observações da análise

O host `172.30.1.2` encontra-se ativo e respondeu ao scan. O resultado oficial de `nmap -sV -sC` mostrou 999 portas TCP fechadas e uma porta aberta, a porta 22.

O serviço SSH está associado à versão `OpenSSH 9.6p1 Ubuntu 3ubuntu13.18`.

O comando `ss -tuln` mostrou também as portas locais 40200, 40205, 40300, 40305 e 41857 em escuta. Estas portas foram observadas localmente, mas não aparecem no scan básico porque o Nmap, sem especificação de portas, verifica por padrão os 1000 portos TCP mais comuns.

## 8. Conclusão

A atividade permitiu praticar a utilização de comandos Linux para observação da configuração de rede e dos sockets locais, bem como a utilização do Nmap para deteção de portas, serviços e versões.

O principal resultado do scan solicitado foi a identificação da porta 22/TCP aberta, executando OpenSSH 9.6p1 Ubuntu 3ubuntu13.18. A documentação destes resultados permite caracterizar a superfície de exposição inicial do alvo no contexto do laboratório.


---

## 9. Ficha do laboratório

**Laboratório:** Sessão 1 — Introdução ao Linux para Segurança e Comandos de Rede  
**Curso:** Reskilling  
**Módulo:** Linux e Cibersegurança  
**Objetivo de Aprendizagem:** OA1 — Analisar  
**Duração da prática guiada:** 02:30–03:50 (1h20)  
**Formador:** Péricles Borges

### Contexto

Mapeamento e análise da superfície de exposição de um servidor alvo na rede. A atividade foi realizada no papel de auditor de sistemas, começando pela identificação da interface de rede do ambiente local, seguida da observação dos serviços em escuta e do reconhecimento do alvo com Nmap.

### Ambientes utilizados

- KillerCoda Ubuntu Playground — familiarização com a linha de comandos Linux
- TryHackMe — Further Nmap — reconhecimento do alvo

## 10. Critérios de entrega

| Critério | Resultado documentado |
|---|---|
| Número de portas abertas identificadas | **1 porta TCP aberta** no scan básico documentado |
| Serviços em execução | **SSH** em `22/tcp` |
| Versão exata detetada | **OpenSSH 9.6p1 Ubuntu 3ubuntu13.18** |
| Output completo de `ip a` | ✅ Incluído |
| Output completo de `ss -tuln` | ✅ Incluído |
| Output do Nmap | ✅ Incluído |
| Análise dos resultados | ✅ Incluída |

## 11. Checklist de submissão — Portfólio GitHub

- [x] Criar/atualizar `sessao-01/README.md`
- [x] Documentar o número de portas abertas
- [x] Documentar os serviços encontrados
- [x] Documentar as versões exatas detetadas
- [x] Incluir o output de `ip a`
- [x] Incluir o output de `ss -tuln`
- [x] Incluir o output completo do Nmap
- [x] Registar observações e conclusão
- [x] Fazer commit no repositório do portfólio

## 12. Linha de execução da prática

```text
KillerCoda
   ↓
ip a
   ↓
identificação da interface e IP
   ↓
ss -tuln
   ↓
identificação de sockets/portas em escuta
   ↓
TryHackMe Further Nmap
   ↓
nmap -sV -sC <IP_DO_ALVO>
   ↓
portas + serviços + versões
   ↓
documentação no GitHub
```

## 13. Estado final

**Sessão 1 documentada no portfólio e alinhada aos critérios de entrega do laboratório.**


## 14. Nota sobre o ambiente de estudo

Parte da prática foi realizada no computador pessoal, utilizado como laboratório complementar para executar comandos Linux, testar conceitos de rede e manter o estudo ativo durante os períodos de espera do TryHackMe, que em determinadas situações chegavam a **24 horas**.

Os resultados apresentados nesta sessão identificam explicitamente quando o output pertence ao ambiente local e quando corresponde ao laboratório remoto do TryHackMe.


---

**Autor / Formando:** Marcos dos Santos
