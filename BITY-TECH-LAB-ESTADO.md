# BITY TECH LAB — Estado e Continuidade

**Atualizado:** 13/10/2026  
**Objetivo de Kris:** transformar o computador BITY JR. num laboratório de aprendizagem hacker ética e reprodução de tecnologias, reutilizando instalações existentes, evitando duplicações e preservando o projeto Bity Jr. Robô.

## Regra de retoma

Ler este ficheiro antes de continuar. Retomar pelo próximo passo pendente; não repetir inventários já concluídos, não reinstalar Ubuntu sem motivo e não modificar configurações sem explicar a necessidade. Distinguir sempre factos observados de hipóteses.

## Computador anfitrião e WSL

Dados recolhidos por PowerShell:

- Windows: 10.0.26200.9457.
- WSL: 2.7.11.0; kernel WSL 6.18.33.2-2.
- RAM física reportada: 23.2 GB.
- CPU reportada: AMD Ryzen 7 170 with Radeon Graphics, 8 núcleos e 16 processadores lógicos.
- Unidade C: 475.1 GB total, 381.5 GB livres no momento da consulta.
- wsl -l -v mostrou duas distribuições, ambas WSL 2: Ubuntu e Ubuntu-24.04, inicialmente paradas.

## Distribuição Ubuntu-24.04

- Ubuntu 24.04.5 LTS; utilizador bitylab; home /home/bitylab.
- Diretórios existentes: /home/bitylab/cyber-lab e /home/bitylab/sessao-06-lab.

Conteúdo listado em cyber-lab:
- aircube-main.dec.js
- ubus-zero-session.json
- prelogin.txt
- descoberta.txt
- router-scan.txt
- ubus-zero-headers.txt
- dns-recursive.txt
- dns-norecurse.txt
- aircube-main.js
- host-58-scan.txt
- root-headers.txt
- samsung-83-scan.txt

Estes nomes sugerem materiais e resultados prévios de experiências com routers/AirCube, HTTP/headers e DNS. Não repetir testes automaticamente; primeiro ler e compreender os ficheiros e confirmar o âmbito/autorização de qualquer teste futuro.

Conteúdo listado em sessao-06-lab:
- updates-after.txt
- lynis-final.txt
- sshd_config
- dossantos92
- dossantos92.pub
- ss.txt
- nmap.txt
- ufw-rules.txt
- updates-before.txt

O ficheiro dossantos92 pode ser uma chave privada SSH; isso ainda não foi confirmado. Não imprimir o conteúdo, partilhar ou publicar esse ficheiro. A verificação pendente deve limitar-se a permissões/metadados e tipo de ficheiro.

## Distribuição Ubuntu

- Ubuntu 26.04 LTS; utilizador dsnts; home /home/dsnts.
- Projeto existente: /home/dsnts/bityjr.
- O inventário mostrou .git, .venv, README.md, WAVs de áudio/teste, backups memoria*.json, pastas memory/ e security/, authorization.py, authorized_targets.json, src/ com módulos de agente, memória, aprendizagem, decisão, perceção, segurança, sensores e visão, e tests/.
- Preservar projeto, Git, backups, memórias e .venv. Não limpar versões antigas nem reconstruir o projeto antes de uma análise.

Aviso observado: algumas chamadas WSL mostraram “Failed to start the systemd user session for 'root'”, mas comandos de leitura executados depois conseguiram apresentar informação dos sistemas. O aviso não foi diagnosticado nem reparado; não inferir perda de dados e não reinstalar com base apenas nele.

## Arquitetura proposta

- BITY CYBER LAB: Linux/redes, protocolos e tráfego, Nmap, serviços, hardening e forense.
- BITY TECH LAB: reprodução de tecnologias, APIs, aplicações web, bases de dados e serviços locais.
- BITY JR. ROBÔ: projeto existente de robótica, a manter separado para preservar código e dados.
- IoT/eletrónica: integrar gradualmente ESP32-S3, sensores, motores e simulação.
- Usar ambientes isolados para alvos vulneráveis. Duas distribuições WSL não equivalem automaticamente a duas máquinas virtuais independentes.
- Instalar apenas o que estiver em falta após inventário. Não criar outra instalação Ubuntu por rotina.

## Próximos passos para a próxima sessão

### Passo 1 — verificar os ficheiros relacionados com SSH sem revelar chaves

No PowerShell, executar um comando de cada vez:

    wsl -d Ubuntu-24.04 -u root -- ls -l /home/bitylab/sessao-06-lab/dossantos92 /home/bitylab/sessao-06-lab/dossantos92.pub

    wsl -d Ubuntu-24.04 -u root -- file /home/bitylab/sessao-06-lab/dossantos92 /home/bitylab/sessao-06-lab/dossantos92.pub

Não usar cat, type, Get-Content ou outros comandos que mostrem o conteúdo de uma possível chave privada.

### Passo 2 — verificar ferramentas existentes sem instalar nada

Executar as consultas seguintes individualmente na distribuição Ubuntu-24.04:

    wsl -d Ubuntu-24.04 -u root -- git --version
    wsl -d Ubuntu-24.04 -u root -- python3 --version
    wsl -d Ubuntu-24.04 -u root -- nmap --version
    wsl -d Ubuntu-24.04 -u root -- curl --version
    wsl -d Ubuntu-24.04 -u root -- jq --version
    wsl -d Ubuntu-24.04 -u root -- tcpdump --version
    wsl -d Ubuntu-24.04 -u root -- tshark --version
    wsl -d Ubuntu-24.04 -u root -- docker --version
    wsl -d Ubuntu-24.04 -u root -- podman --version
    wsl -d Ubuntu-24.04 -u root -- gcc --version
    wsl -d Ubuntu-24.04 -u root -- make --version

Se uma ferramenta não existir, registar o resultado; não instalar tudo indiscriminadamente.

### Passo 3 — decidir arquitetura e cópias de segurança

Depois dos passos 1 e 2:
1. Rever o conteúdo existente de cyber-lab e sessao-06-lab.
2. Decidir uma estrutura de pastas comum sem mover ficheiros antes de fazer backup.
3. Escolher ferramentas por módulo e instalar apenas as necessárias.
4. Definir estratégia de backups/snapshots.
5. Começar por redes e serviços locais; depois expandir para segurança web/APIs, forense, IoT e simulação.

## Computador remoto via Chrome Remote Desktop

Kris referiu que acede a outro computador através do Chrome Remote Desktop, mas respondeu “talvez” quanto à autorização para instalar ferramentas e realizar experiências. Não instalar, configurar nem testar esse computador até existir autorização explícita e clara. O plano atual usa o próprio BITY JR.

## Disco externo Hitachi — prioridade de preservação de dados

Modelo indicado: Hitachi HTS543225L9A300. Kris relatou que o disco gira, produz ruído descrito como raspagem metálica e não é reconhecido pelo Windows. O Windows só enumerou o SSD interno Samsung como disco físico confirmado, embora tenha mostrado um dispositivo genérico de armazenamento USB/UAS. Kris quer recuperar os ficheiros.

**Manter o Hitachi desligado.** Não voltar a alimentá-lo para testar; não executar CHKDSK, inicialização, formatação, testes de superfície ou software de recuperação; não abrir o mecanismo. Dado o ruído de raspagem e a importância dos ficheiros, a opção mais segura é avaliação por especialista em recuperação de dados.

## Princípios operacionais

- Não instalar ou atualizar ferramentas antes do inventário.
- Preferir comandos de leitura até ficar claro o ambiente e o objetivo.
- Fazer backups antes de alterações de sistema, sempre que viável e seguro.
- Manter cibersegurança, reprodução de tecnologias e Bity Jr. Robô em áreas separadas.
- Só testar routers, dispositivos, serviços e computadores para os quais Kris tenha autorização.
- Documentar objetivo, ambiente, dependências, passos reproduzíveis, outputs, limitações e recuperação.
- Na retoma, usar os resultados já recolhidos como ponto de partida; não repetir o inventário já feito.

**Pedido de Kris:** guardar o estado atual e continuar amanhã, sem perder contexto nem voltar a mandar executar tarefas já concluídas.
