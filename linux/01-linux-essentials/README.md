# Linux Essentials

## Objetivo

Este laboratório tem como objetivo revisar e praticar fundamentos essenciais de Linux aplicados a ambientes de infraestrutura, DevOps e administração de sistemas.

Os exercícios serão documentados de forma prática, com foco em comandos, validação, troubleshooting e boas práticas.

## Ambiente

- Sistema operacional: Linux
- Acesso: Terminal
- Editor: Visual Studio Code
- Controle de versão: Git
- Repositório: Infrastructure Labs

## Conteúdo do laboratório

Este laboratório abordará:

- Navegação no sistema de arquivos
- Manipulação de arquivos e diretórios
- Permissões
- Usuários e grupos
- Processos
- Serviços
- Logs
- Rede
- Armazenamento
- Comandos administrativos
- Troubleshooting básico

## Estrutura

```text
01-linux-essentials/
├── README.md
├── commands.md
├── troubleshooting.md
└── images/

## Ambiente utilizado

O laboratório foi executado em Ubuntu 24.04 LTS utilizando WSL 2 sobre Windows 11 Pro.

### Informações do sistema

| Item | Valor |
|---|---|
| Distribuição | Ubuntu 24.04.4 LTS |
| Codename | Noble Numbat |
| Ambiente | WSL 2 |
| Arquitetura | x86_64 |
| Hostname | PK |
| Usuário | paulokildery_ubuntu |
| Interface de rede | eth0 |
| IPv4 | 172.26.8.139/20 |
| Gateway | 172.26.0.1 |
| DNS | 10.255.255.254 |

## Validação de conectividade

Foram realizados testes de interface, roteamento, resolução DNS e acesso HTTPS.

A resolução DNS IPv4 foi validada com:

```bash
getent ahostsv4 google.com

