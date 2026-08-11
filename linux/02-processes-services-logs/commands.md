# Comandos — Processos, Serviços e Logs

Este documento reúne os principais comandos executados durante o Laboratório 02 de Linux.

## 1. Processos

### Listar processos do terminal atual

```bash
ps
```

### Listar processos do sistema

```bash
ps -ef
```

### Exibir processos com informações detalhadas

```bash
ps -eux
```

### Localizar processos Bash

```bash
pgrep -a bash
```

### Descobrir o PID do Bash atual

```bash
echo $$
```

---

## 2. Processos em background

### Criar um processo em background

```bash
sleep 600 &
```

O caractere `&` envia o processo para background.

### Visualizar jobs do shell

```bash
jobs -l
```

### Localizar o processo sleep

```bash
pgrep -a sleep
```

Exemplo observado no laboratório:

```text
838 sleep 600
```

### Consultar um processo pelo PID

```bash
ps -fp 838
```

Durante o laboratório foi possível observar:

- PID — identificador do processo;
- PPID — identificador do processo pai;
- usuário proprietário do processo;
- comando executado.

---

## 3. Encerramento de processos

### Listar sinais disponíveis

```bash
kill -l
```

### Encerrar um processo utilizando SIGTERM

```bash
kill -15 838
```

O sinal `15` corresponde ao:

```text
SIGTERM
```

O SIGTERM solicita ao processo que termine de forma controlada.

Depois do encerramento, a validação pode ser feita com:

```bash
pgrep -a sleep
jobs -l
```

---

## 4. systemd

### Verificar o processo PID 1

```bash
ps -p 1 -o pid,ppid,user,comm,args
```

Resultado observado:

```text
PID 1 → systemd
```

### Verificar o estado geral do systemd

```bash
systemctl is-system-running
```

Resultado observado:

```text
running
```

---

## 5. Gerenciamento de serviços

O serviço `cron` foi utilizado para os testes.

### Verificar o status

```bash
systemctl status cron --no-pager
```

### Verificar se o serviço está ativo

```bash
systemctl is-active cron
```

### Verificar se está habilitado para inicialização automática

```bash
systemctl is-enabled cron
```

### Consultar propriedades específicas

```bash
systemctl show cron -p ActiveState -p SubState -p UnitFileState
```

---

## 6. Parar, iniciar e reiniciar serviços

### Parar

```bash
sudo systemctl stop cron
```

### Iniciar

```bash
sudo systemctl start cron
```

### Reiniciar

```bash
sudo systemctl restart cron
```

Uma conclusão importante do laboratório foi:

```text
active != enabled
```

`active` informa se o serviço está executando naquele momento.

`enabled` informa se o serviço está configurado para iniciar automaticamente.

---

## 7. Logs com journalctl

### Consultar as últimas 20 entradas do cron

```bash
journalctl -u cron --no-pager -n 20
```

### Consultar logs por período

```bash
journalctl -u cron --since "10 minutes ago" --no-pager
```

### Procurar inicializações do serviço

```bash
journalctl -u cron --since "today" --no-pager | grep -i "started"
```

### Procurar eventos importantes

```bash
journalctl -u cron --since "today" --no-pager | grep -Ei "stop|start|fail|error"
```

---

## 8. Investigação de erros

### Mostrar erros do boot atual

```bash
journalctl -p err -b --no-pager
```

### Mostrar unidades systemd que falharam

```bash
systemctl --failed --no-pager
```

No laboratório, o resultado foi:

```text
0 loaded units listed.
```

Ou seja, nenhuma unit estava em estado `failed`.

---

## 9. Investigação do WSL

### Procurar mensagens relacionadas ao WSL

```bash
journalctl -b --no-pager | grep -i "WSL"
```

### Procurar mensagens relacionadas ao PAM

```bash
journalctl -b --no-pager | grep -i "pam_lastlog"
```

### Verificar espaço utilizado pelos journals

```bash
journalctl --disk-usage
```

---

## 10. Fluxo rápido de diagnóstico

Uma sequência básica utilizada durante o troubleshooting pode ser:

```bash
systemctl status <servico> --no-pager
systemctl is-active <servico>
systemctl is-enabled <servico>
journalctl -u <servico> --no-pager -n 50
systemctl --failed --no-pager
```
