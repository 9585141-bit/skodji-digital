# Sessão 03 — Análise Forense de Servidores Linux e Persistência via systemd

## 1. Objetivo

Concluir a sala do TryHackMe **Linux Forensics** e investigar um mecanismo de persistência num servidor Linux.

## 2. Resultado da sala

- Plataforma: TryHackMe
- Sala: **Linux Forensics**
- Estado: **Concluída**
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

Também foi verificado que `/var/log/badr.log` já não existia e que `/etc/badr` estava vazio.

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

O serviço apareceu como:

```text
Loaded: loaded (/etc/systemd/system/IpManager.service; enabled; vendor preset: enabled)
Active: active (running)
Main PID: ... (bash)
/bin/bash /etc/network/ZGtsam5hZG1ua5Fu.sh
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