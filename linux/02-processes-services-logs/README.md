# Laboratório 02 — Processos, Serviços e Logs no Linux

## Objetivo

Este laboratório tem como objetivo praticar a administração e o troubleshooting de processos, serviços e logs em Linux.

Os testes foram realizados em Ubuntu 24.04 LTS executado através do WSL2 no Windows 11.

O laboratório aborda conceitos utilizados em administração Linux, infraestrutura e ambientes DevOps, incluindo identificação de processos, gerenciamento de serviços com systemd e análise de logs com journalctl.

---

## Ambiente utilizado

- Windows 11 Pro
- WSL2
- Ubuntu 24.04 LTS
- systemd
- Bash
- Git
- Visual Studio Code
- GitHub

---

## 1. Processos Linux

Inicialmente foram utilizados comandos para visualizar os processos em execução.

Exemplos:

```bash
ps
ps -ef
ps -eux
pgrep -a bash
```

Também foi utilizado:

```bash
echo $$
```

para identificar o PID do shell Bash atual.

Durante o laboratório foi possível observar a relação entre:

- PID — Process ID
- PPID — Parent Process ID
- processo pai
- processo filho

---

## 2. Processos em background

Foi criado um processo temporário utilizando:

```bash
sleep 600 &
```

O caractere `&` faz com que o processo seja executado em background.

O processo foi identificado através dos comandos:

```bash
jobs -l
pgrep -a sleep
ps -fp <PID>
```

Isso permitiu observar o Job ID do Bash, o PID do processo e o PPID correspondente ao shell que iniciou o processo.

---

## 3. Sinais e encerramento de processos

O processo `sleep` foi encerrado utilizando:

```bash
kill -15 <PID>
```

O sinal 15 corresponde ao:

```text
SIGTERM
```

O SIGTERM solicita que o processo seja encerrado de forma controlada.

Após o encerramento, a inexistência do processo foi validada novamente com:

```bash
pgrep -a sleep
jobs -l
```

---

## 4. systemd

Foi verificado que o Ubuntu utilizado no laboratório está executando o `systemd` como PID 1.

Comando utilizado:

```bash
ps -p 1 -o pid,ppid,user,comm,args
```

Resultado observado:

```text
PID 1 → systemd
```

Também foi validado o estado geral do systemd:

```bash
systemctl is-system-running
```

Resultado:

```text
running
```

---

## 5. Gerenciamento de serviços

O serviço `cron` foi utilizado como exemplo para estudar gerenciamento de serviços.

Foram utilizados:

```bash
systemctl status cron --no-pager
systemctl is-active cron
systemctl is-enabled cron
```

O laboratório demonstrou uma diferença importante:

```text
active
```

indica se o serviço está executando naquele momento.

Enquanto:

```text
enabled
```

indica se o serviço está configurado para iniciar automaticamente.

Portanto:

```text
active != enabled
```

---

## 6. Stop, Start e Restart

O serviço cron foi controlado utilizando:

```bash
sudo systemctl stop cron
sudo systemctl start cron
sudo systemctl restart cron
```

Após o `stop`, foi observado:

```text
inactive
enabled
```

Isso demonstrou que parar um serviço não significa desabilitar sua inicialização automática.

Após iniciar novamente o serviço:

```text
active
enabled
```

O `restart` também resultou na criação de um novo processo principal para o serviço.

---

## 7. Logs com journalctl

Os logs do serviço cron foram analisados através do journal do systemd.

Exemplos:

```bash
journalctl -u cron --no-pager -n 20
```

e:

```bash
journalctl -u cron --since "10 minutes ago" --no-pager
```

Os registros permitiram acompanhar eventos como:

```text
Stopping cron.service
Deactivated successfully
Stopped cron.service
Started cron.service
```

---

## 8. Investigação de erros

Também foram consultadas mensagens de erro do boot atual:

```bash
journalctl -p err -b --no-pager
```

E unidades do systemd em estado de falha:

```bash
systemctl --failed --no-pager
```

No momento do laboratório, nenhuma unidade estava em estado `failed`.

---

## 9. Fluxo básico de troubleshooting

O laboratório permitiu praticar o seguinte fluxo:

```text
Serviço ou aplicação apresenta problema
            |
            v
systemctl status
            |
            v
Verificar Active / Enabled
            |
            v
Identificar PID e processos
            |
            v
Consultar journalctl
            |
            v
Filtrar horário e mensagens
            |
            v
Identificar causa
            |
            v
Aplicar correção
            |
            v
Validar novamente
```

---

## Principais conceitos praticados

- Processos Linux
- PID
- PPID
- Processos pai e filho
- Background jobs
- Sinais Linux
- SIGTERM
- systemd
- systemctl
- Serviços Linux
- start
- stop
- restart
- active
- enabled
- journalctl
- Logs do sistema
- Troubleshooting

---

## Conclusão

Este laboratório apresentou fundamentos importantes para administração Linux e troubleshooting de serviços.

A prática permitiu relacionar processos, serviços e logs em um fluxo único de diagnóstico, criando uma base para estudos posteriores envolvendo servidores Linux, automação, containers, Docker, CI/CD e ambientes DevOps.
