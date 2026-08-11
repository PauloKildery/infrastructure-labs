# Troubleshooting — Processos, Serviços e Logs Linux

Este documento registra problemas e situações reais encontrados durante o Laboratório 02, incluindo diagnóstico, causa identificada e validação da solução.

---

## Caso 01 — Unidade D: deixou de estar acessível no WSL2

### Sintoma

Ao acessar o repositório localizado em:

```text
/mnt/d/DEV/GitHub/infrastructure-labs
```

comandos Git começaram a retornar:

```text
fatal: failed to stat '/mnt/d/DEV/GitHub/infrastructure-labs': No such device
```

A tentativa de acessar diretamente `/mnt/d` também falhou:

```bash
ls -la /mnt/d
```

Resultado:

```text
ls: cannot access '/mnt/d': No such device
```

---

### Diagnóstico

Primeiro foi verificado o conteúdo de `/mnt`:

```bash
ls -la /mnt
```

Depois:

```bash
df -h | grep -E '/mnt/[cd]'
```

Foi observado que:

```text
/mnt/c
```

estava disponível, enquanto `/mnt/d` apresentava problema.

Para verificar se a unidade continuava disponível no Windows, foi executado:

```bash
cmd.exe /c "dir D:\DEV\GitHub\infrastructure-labs"
```

O Windows conseguiu acessar normalmente o repositório.

Isso indicou que o problema estava relacionado à montagem da unidade no ambiente WSL, e não à perda dos arquivos na unidade D:.

---

### Solução aplicada

No PowerShell do Windows foi verificado o estado das distribuições:

```powershell
wsl -l -v
```

Depois o WSL foi completamente encerrado:

```powershell
wsl --shutdown
```

A distribuição Ubuntu foi iniciada novamente:

```powershell
wsl -d Ubuntu-24.04
```

---

### Validação

Após reiniciar o WSL:

```bash
ls -la /mnt/d
```

voltou a apresentar o conteúdo da unidade.

Também foi validada a montagem:

```bash
df -h | grep -E '/mnt/[cd]'
```

Resultado observado:

```text
C:\  → /mnt/c
D:\  → /mnt/d
```

Finalmente:

```bash
cd /mnt/d/DEV/GitHub/infrastructure-labs
git status
```

O Git voltou a funcionar normalmente.

---

### Aprendizado

O erro inicialmente apareceu durante a utilização do Git, mas o Git não era a causa.

O problema estava em uma camada inferior:

```text
Git
 ↓
Sistema de arquivos
 ↓
Montagem /mnt/d
 ↓
Integração WSL ↔ Windows
```

Antes de alterar configurações do Git, foi necessário verificar se o caminho utilizado pelo repositório estava realmente disponível.

---

## Caso 02 — Diferença entre serviço parado e serviço desabilitado

### Cenário

O serviço `cron` estava inicialmente:

```text
active
enabled
```

Foi executado:

```bash
sudo systemctl stop cron
```

Depois:

```bash
systemctl is-active cron
systemctl is-enabled cron
```

Resultado:

```text
inactive
enabled
```

---

### Análise

O teste demonstrou que `active` e `enabled` representam estados diferentes.

`active` indica se o serviço está executando atualmente.

`enabled` indica se o serviço está configurado para iniciar automaticamente.

Portanto:

```text
stop != disable
```

e:

```text
start != enable
```

---

### Validação

O serviço foi iniciado novamente:

```bash
sudo systemctl start cron
```

e posteriormente reiniciado:

```bash
sudo systemctl restart cron
```

A validação foi realizada com:

```bash
systemctl status cron --no-pager
systemctl is-active cron
systemctl is-enabled cron
```

O estado retornou para:

```text
active
enabled
```

---

## Caso 03 — Investigação dos logs do cron

### Objetivo

Confirmar através dos logs as operações realizadas sobre o serviço.

Foi executado:

```bash
journalctl -u cron --no-pager -n 20
```

Os registros apresentaram eventos como:

```text
Stopping cron.service
cron.service: Deactivated successfully.
Stopped cron.service
Started cron.service
```

Também foi utilizada uma consulta baseada em período:

```bash
journalctl -u cron --since "10 minutes ago" --no-pager
```

Isso permitiu reconstruir cronologicamente as operações realizadas no serviço.

---

### Evidência adicional

Após o serviço voltar a funcionar, o journal registrou uma execução do cron:

```text
(root) CMD (cd / && run-parts --report /etc/cron.hourly)
```

Isso forneceu uma evidência adicional de que o serviço não estava apenas marcado como `active`, mas também executando suas tarefas.

---

## Caso 04 — Análise de erros do boot

Foi utilizado:

```bash
journalctl -p err -b --no-pager
```

Entre as mensagens observadas estavam registros relacionados a:

```text
PCI
dxg
pam_lastlog.so
WSL
```

Também foi executado:

```bash
systemctl --failed --no-pager
```

Resultado:

```text
0 loaded units listed.
```

Portanto, apesar da existência de mensagens classificadas como erro no journal, nenhuma unit do systemd estava em estado `failed`.

---

### Aprendizado

Uma mensagem de erro no log não deve ser interpretada automaticamente como uma falha atual do sistema.

É necessário correlacionar:

```text
mensagem
+
horário
+
serviço
+
estado atual
+
sintoma
```

antes de aplicar qualquer alteração.

---

## Caso 05 — Erros de digitação durante investigação de processos

Durante os testes foram digitados comandos como:

```bash
pggrep -a bash
```

em vez de:

```bash
pgrep -a bash
```

Também foram utilizados nomes incorretos:

```bash
pgrep sllep
pgrep slepp
```

em vez de:

```bash
pgrep sleep
```

Como nenhum PID foi retornado, uma tentativa como:

```bash
ps -fp $(pgrep sllep)
```

resultou em erro porque `ps -p` não recebeu nenhum PID.

---

### Aprendizado

Antes de interpretar uma saída vazia como ausência de processo, é importante verificar:

- sintaxe;
- nome do processo;
- retorno do comando anterior;
- PID encontrado.

Uma investigação segura pode ser realizada em etapas:

```bash
pgrep -a sleep
```

e somente depois:

```bash
ps -fp <PID>
```

Isso evita encadear comandos utilizando uma variável ou substituição vazia.

---

## Fluxo de troubleshooting praticado

```text
Identificar o sintoma
        |
        v
Confirmar o estado atual
        |
        v
Coletar evidências
        |
        v
Verificar processos e serviços
        |
        v
Consultar logs
        |
        v
Correlacionar horário e eventos
        |
        v
Identificar a camada responsável
        |
        v
Aplicar a correção
        |
        v
Validar novamente
```

## Conclusão

Os exercícios demonstraram que troubleshooting não consiste apenas em executar comandos de correção.

O processo deve priorizar coleta de evidências, identificação da camada responsável, análise dos logs e validação posterior da solução.
