# Laboratório — Sessão 3
## Hardening de Redes Linux e Configuração de Firewalls

**Curso:** Reskilling  
**Módulo:** Linux e Cibersegurança  
**Objetivo de Aprendizagem:** OA3 · Aplicar  
**Duração da prática guiada:** 19:15 – 20:50  
**Formador:** Péricles Borges

## 1. Contexto

Configuração de uma política defensiva estrita para impedir acessos não autorizados a serviços críticos do servidor, combinando **UFW** e **iptables**.

## 2. Ambiente Virtual

- **KillerCoda Ubuntu Playground:** https://killercoda.com/playgrounds/scenario/ubuntu
- **TryHackMe — Network Security Essentials** (gratuito): https://tryhackme.com/room/networksecurityessentials

## 3. Tarefas a Executar

### 3.1 Verificar o estado atual do UFW

```bash
sudo ufw status
```

### 3.2 Alterar as políticas padrão

Bloquear ligações de entrada e permitir ligações de saída:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### 3.3 Permitir acesso SSH

```bash
sudo ufw allow 22/tcp
```

Esta regra mantém o acesso SSH pela porta TCP 22 enquanto o tráfego de entrada não autorizado permanece bloqueado pela política padrão.

### 3.4 Adicionar regra de bloqueio no iptables

Para simular o bloqueio de um IP malicioso fictício na chain `INPUT`:

```bash
sudo iptables -A INPUT -s 203.0.113.50 -j DROP
```

### 3.5 Guardar o estado persistente do iptables

```bash
sudo iptables-save | sudo tee /etc/iptables/rules.v4
```

## 4. Critérios de Entrega

Documentar no portfólio:

- captura de ecrã ou output de texto das regras UFW ativas:

```bash
sudo ufw status verbose
```

- listagem completa do `iptables`:

```bash
sudo iptables -L -v
```

- breve explicação da política aplicada, indicando o que está bloqueado e porquê.

## 5. Checklist de Submissão — Portfólio GitHub

- [x] Criar/atualizar `sessao-03/README.md` com a ficha e os resultados documentados
- [x] Executar anteriormente `sudo ufw status verbose`
- [x] Executar anteriormente `sudo iptables -L -v`
- [x] Recuperar e registar os outputs reais dos dois comandos
- [x] Registar uma verificação posterior do UFW/iptables no ambiente BITYJR (Ubuntu 24.04.5; ver secção 6.4)
- [x] Fazer commit e push para o repositório do portfólio

> **Histórico da evidência:** a recolha inicial abaixo registou UFW inativo e políticas `INPUT`/`OUTPUT` do iptables em `ACCEPT`. Foi recolhida posteriormente uma nova verificação no ambiente identificado como `BITYJR`, Ubuntu 24.04.5 LTS, na qual o UFW está ativo e a política `INPUT` é `DROP`. Os dois registos são mantidos com contexto; o output inicial não é apagado nem reinterpretado como se tivesse sido obtido depois. Ver secção 6.4 para a evidência posterior.

## 6. Outputs reais recolhidos no laboratório

### 6.1 Estado do UFW

Comando executado:

```bash
sudo ufw status verbose
```

Output real:

```text
Status: inactive
```

**Interpretação:** nessa recolha inicial, o UFW estava **inativo**. Uma recolha posterior mostrou um estado diferente no ambiente identificado como `BITYJR`; a evidência posterior está registada separadamente na secção 6.4.

### 6.2 Estado atual do iptables

Comando executado:

```bash
sudo iptables -L -v
```

Output real:

```text
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 DOCKER-USER  all  --  any    any     anywhere             anywhere            
    0     0 DOCKER-FORWARD  all  --  any    any     anywhere             anywhere            

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain DOCKER (1 references)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 DROP       all  --  !docker0 docker0  anywhere             anywhere            

Chain DOCKER-BRIDGE (1 references)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 DOCKER     all  --  any    docker0  anywhere             anywhere            

Chain DOCKER-CT (1 references)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 ACCEPT     all  --  any    docker0  anywhere             anywhere             ctstate RELATED,ESTABLISHED

Chain DOCKER-FORWARD (1 references)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 DOCKER-INTERNAL  all  --  any    any     anywhere             anywhere            
    0     0 DOCKER-BRIDGE  all  --  any    any     anywhere             anywhere            
    0     0 ACCEPT     all  --  docker0 any     anywhere             anywhere

Chain DOCKER-INTERNAL (1 references)
 pkts bytes target     prot opt in     out     source               destination         

Chain DOCKER-USER (1 references)
 pkts bytes target     prot opt in     out     source               destination         
```

**Interpretação:** a recolha inicial mostrou políticas padrão `ACCEPT` em `INPUT` e `OUTPUT`, além de chains relacionadas com Docker. A verificação posterior do ambiente `BITYJR` mostrou `INPUT DROP` e `OUTPUT ACCEPT`; ver secção 6.4.

### 6.3 Estado documental da tarefa

Os outputs reais exigidos pelo enunciado já foram recuperados e incluídos no portfólio.

**Importante:** os outputs são fotografias do estado observado em momentos diferentes. A nova recolha comprova que, no momento do teste em `BITYJR`, o UFW estava ativo com entrada negada por omissão e que o iptables tinha política `INPUT DROP`. A persistência após reinício e a conectividade SSH efetiva não foram testadas nesta recolha.

### 6.4 Verificação posterior — ambiente BITYJR

**Ambiente reportado pelo terminal:**

```text
hostname: BITYJR
Ubuntu 24.04.5 LTS
```

Comando:

```bash
sudo ufw status verbose
```

Output observado:

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
2222/tcp                   ALLOW IN    Anywhere
2222/tcp (v6)              ALLOW IN    Anywhere (v6)
```

Comandos:

```bash
sudo iptables -S INPUT
sudo iptables -S OUTPUT
```

Output observado:

```text
-P INPUT DROP
-A INPUT -j ufw-before-logging-input
-A INPUT -j ufw-before-input
-A INPUT -j ufw-after-input
-A INPUT -j ufw-after-logging-input
-A INPUT -j ufw-reject-input
-A INPUT -j ufw-track-input
-P OUTPUT ACCEPT
-A OUTPUT -j ufw-before-logging-output
-A OUTPUT -j ufw-before-output
-A OUTPUT -j ufw-after-output
-A OUTPUT -j ufw-after-logging-output
-A OUTPUT -j ufw-reject-output
-A OUTPUT -j ufw-track-output
```

**Interpretação técnica:** nesta recolha posterior, o UFW está ativo; a política por omissão nega ligações de entrada, permite saída e desativa encaminhamento. A única regra de entrada explicitamente listada é `2222/tcp`, para IPv4 e IPv6. As políticas nativas do iptables são coerentes com o UFW: `INPUT DROP` e `OUTPUT ACCEPT`, com chains de processamento geridas pelo UFW.

**Limites desta evidência:** o output confirma o estado no momento da recolha, mas não comprova persistência após reinício, nem que o serviço SSH esteja efetivamente a escutar e acessível em `2222/tcp`. Esses pontos exigem validação separada caso façam parte dos critérios de avaliação.

### 6.5 Verificação complementar posterior — ambiente BITYJR

Esta recolha foi feita posteriormente no ambiente pessoal `BITYJR` (Ubuntu 24.04.5/WSL). **Não representa a repetição do laboratório original nem significa que os entregáveis anteriores estejam por fazer.** Kris confirmou que os testes e exercícios entregáveis da Sessão 03 já tinham sido realizados e entregues anteriormente. Esta secção regista apenas o estado encontrado nesta verificação complementar.

Comandos executados:

```bash
sudo ufw status numbered
sudo ufw show added
sudo grep -nE '203\.0\.113\.50|2222|^ENABLED=' /etc/ufw/user.rules /etc/ufw/user6.rules /etc/ufw/ufw.conf
sudo ls -la /etc/ufw
```

Resultados relevantes observados nesta verificação do BITYJR:

- O UFW está `active`.
- A listagem numerada contém `2222/tcp ALLOW IN Anywhere` e `2222/tcp (v6) ALLOW IN Anywhere (v6)`.
- `ufw show added` apresenta `ufw allow 2222/tcp`.
- Os ficheiros `/etc/ufw/user.rules` e `/etc/ufw/user6.rules` contêm a regra `ALLOW` da porta TCP 2222 nas respetivas famílias IP.
- `/etc/ufw/ufw.conf` contém `ENABLED=yes`.
- `/etc/iptables/rules.v4` não existe neste ambiente.
- Nesta fotografia do BITYJR, não foi encontrada referência ao IP de teste `203.0.113.50` nas regras ativas nem nos ficheiros consultados.

**Interpretação e limite:** existe configuração do UFW guardada nos ficheiros próprios do serviço, incluindo a regra da porta 2222 e a opção para o UFW estar ativado. A ausência do IP de teste nesta configuração atual descreve apenas este ambiente e não invalida a execução e a entrega anteriores dos exercícios, conforme confirmado por Kris. A persistência após reinício não foi novamente testada nesta recolha complementar. Não foi adicionada nem alterada nenhuma regra nesta verificação.

---

# Atividade complementar — Forensics e Persistência via systemd

Este conteúdo já fazia parte do `sessao-03/README.md` e foi preservado como atividade complementar, para manter o histórico do percurso.

## Análise Forense de Servidores Linux e Persistência via systemd

## 1. Objetivo

Concluir a sala do TryHackMe **Linux Forensics** e investigar um mecanismo de persistência num servidor Linux.

## 2. Resultado da sala

- Plataforma: TryHackMe
- Sala: **Linux Forensics**
- Estado: **Concluída**
- Data da conclusão: **25/09/2026**
- Tarefas concluídas: **11**
- Pontos ganhos: **96**
- Streak/Onda: **3**

> Instância utilizada: `10.130.139.11`, utilizador de laboratório `fred`.

## 3. Situação inicial

A sessão SSH apresentava mensagens estranhas e repetitivas, indicando interferência de um processo em segundo plano.

Exemplos:

```text
ERROR!: THE STACK ARRANGER IS NOT ENABLED BEWARE OF STACK COLLISIONS
INFO!: RETICULATING SPLINES
INFO!: PURGING RAM BITS
WARNING!: DIHYDROGEN MONOXIDE DETECTED IN ATMOSPHERE
```

## 4. Enumeração de serviços

```bash
systemctl --type=service --state=active --no-pager
```

Entre os serviços observados estavam `badr.service` e `IpManager.service`.

## 5. Investigação de badr.service

```bash
cat /etc/systemd/system/badr.service
```

O serviço executava `/etc/badr/badr` e tinha um `ExecStartPost` que removia vários ficheiros relacionados com o próprio serviço.

Também foi verificado que `/var/log/badr.log` não existia e que `/etc/badr` estava vazio.

## 6. Processo com executável apagado

PID do serviço:

```bash
systemctl show -p MainPID --value badr.service
```

Resultado observado:

```text
1217
```

Executável do processo:

```bash
sudo readlink -f /proc/1217/exe
```

Resultado:

```text
/etc/badr/badr (deleted)
```

### Interpretação

O processo continuava a executar um binário que já tinha sido removido do sistema de ficheiros.

## 7. Identificação do mecanismo de persistência relevante

O serviço `IpManager.service` foi localizado com:

```bash
systemctl show -p FragmentPath --value IpManager.service
```

Resultado:

```text
/etc/systemd/system/IpManager.service
```

Conteúdo:

```bash
cat /etc/systemd/system/IpManager.service
```

```ini
[UNIT]
Description=Network Management Service

[Service]
Type=simple
Restart=always
User=root
ExecStart=/bin/bash /etc/network/ZGtsam5hZG1ua2Fu.sh

[Install]
WantedBy=multi-user.target
```

### Ponto crítico

O serviço executava, como `root`, o script:

```text
/etc/network/ZGtsam5hZG1ua2Fu.sh
```

## 8. Análise do script

```bash
sudo cat /etc/network/ZGtsam5hZG1ua2Fu.sh
```

Logo no início do script foi encontrada a flag:

```text
##[gh0st_1n_the_machine]
```

O script também continha uma função que repetia mensagens através de `wall` e outra função que simulava uma investigação falsa com comandos como `ls`, `ps` e leitura do crontab.

## 9. Confirmação final

```bash
systemctl status IpManager
```

O serviço apareceu como ativo e habilitado, executando:

```text
/bin/bash /etc/network/ZGtsam5hZG1ua2Fu.sh
```

Isso confirmou a persistência via systemd e a execução contínua do script.

## 10. Flag validada

```text
[gh0st_1n_the_machine]
```

**Nota:** nesta instância os colchetes fazem parte da resposta aceite pelo TryHackMe.

## 11. Cadeia forense

```text
systemd
   |
   +--> badr.service
   |       |
   |       +--> /etc/badr/badr (deleted)
   |
   +--> IpManager.service
           |
           +--> User=root
           |
           +--> /bin/bash
                  |
                  +--> /etc/network/ZGtsam5hZG1ua2Fu.sh
                          |
                          +--> wall
                          |
                          +--> mensagens falsas
                          |
                          +--> [gh0st_1n_the_machine]
```

## 12. Principais comandos praticados

```bash
systemctl --type=service --state=active --no-pager
cat /etc/systemd/system/badr.service
systemctl show -p MainPID --value badr.service
sudo readlink -f /proc/1217/exe
systemctl show -p FragmentPath --value IpManager.service
cat /etc/systemd/system/IpManager.service
sudo cat /etc/network/ZGtsam5hZG1ua2Fu.sh
systemctl status IpManager
```

## 13. Lições aprendidas

### Persistência via systemd

Serviços systemd podem iniciar processos automaticamente, reiniciá-los e permanecer habilitados para arrancar com o sistema.

### Artefactos apagados

Um ficheiro removido do disco pode continuar em uso por um processo. O link `/proc/<PID>/exe` pode mostrar `(deleted)` enquanto o processo continua ativo.

### Seguir a cadeia de execução

```text
serviço
  ↓
ExecStart
  ↓
processo/script
  ↓
comportamento
  ↓
artefacto
  ↓
flag
```

### Distinguir ruído de evidência

As mensagens falsas eram parte do comportamento do laboratório. A evidência decisiva estava na configuração do serviço e no script executado por ele.

## 14. Estado da atividade

- [x] Conexão com a máquina do laboratório
- [x] Enumeração de serviços systemd
- [x] Identificação de serviços suspeitos
- [x] Análise de `badr.service`
- [x] Investigação de processo com executável apagado
- [x] Identificação de `IpManager.service`
- [x] Análise do script executado como root
- [x] Recuperação da flag
- [x] Validação da resposta no TryHackMe
- [x] Sala concluída

**Sala concluída com sucesso.**


## 15. Nota sobre o ambiente de estudo

Durante a formação, o computador pessoal foi utilizado como laboratório complementar para continuar os estudos quando o TryHackMe apresentava períodos de espera de até **24 horas** em determinados laboratórios gratuitos.

A máquina local foi utilizada para praticar comandos Linux, analisar conceitos de systemd e forensics e organizar a documentação. As evidências específicas da sala TryHackMe permanecem identificadas como resultados do laboratório remoto.


---

**Autor / Formando:** Marcos dos Santos
