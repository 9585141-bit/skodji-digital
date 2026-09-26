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
- [ ] Confirmar/documentar a aplicação final da política de hardening solicitada
- [x] Fazer commit e push para o repositório do portfólio

> **Estado da evidência:** os outputs reais foram agora recuperados e inseridos abaixo. Eles mostram o estado observado no momento da verificação: UFW inativo e políticas `INPUT`/`OUTPUT` do iptables em `ACCEPT`. A aplicação final da política defensiva permanece separada dessa evidência e não é declarada como concluída sem o respetivo output.

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

**Interpretação:** no momento desta verificação, o UFW estava **inativo**. Portanto, este output regista o estado observado antes da ativação/configuração de uma política UFW.

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

**Interpretação:** a verificação mostra que a chain `INPUT` estava com política padrão `ACCEPT`, assim como `OUTPUT`. Também existiam chains relacionadas com Docker, incluindo `DOCKER-USER`, `DOCKER-FORWARD`, `DOCKER-BRIDGE`, `DOCKER-CT` e `DOCKER-INTERNAL`.

### 6.3 Estado documental da tarefa

Os outputs reais exigidos pelo enunciado já foram recuperados e incluídos no portfólio.

**Importante:** estes outputs comprovam o **estado observado no momento da verificação**, mas não comprovam, por si só, a aplicação posterior das regras de hardening solicitadas. A documentação mantém essa distinção para preservar a precisão técnica.


> **Nota de histórico:** a prática foi realizada anteriormente. Nesta versão, os procedimentos e a execução já realizada ficam registados; os outputs completos ainda precisam apenas de ser recuperados do registo original para cumprir a evidência documental.

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
