# Laboratório 02 — Git Branches

## Objetivo

Aprender a criar, utilizar, integrar e excluir branches no Git, aplicando um fluxo de trabalho seguro e semelhante ao utilizado por equipes de desenvolvimento e DevOps.

## Conceito de Branch

Uma branch é uma linha independente de desenvolvimento.

Ela permite realizar alterações, testes e novas implementações sem modificar diretamente a branch principal do projeto.

Neste laboratório, a branch `main` representa a versão estável do repositório, enquanto uma branch separada foi utilizada para desenvolver e documentar as alterações.

## Fluxo executado

```text
main
  |
  +-- criação de nova branch
          |
          +-- edição de arquivo
          +-- git add
          +-- git commit
          |
          +-- retorno para main
          +-- git merge
          +-- git push
          +-- exclusão da branch concluída
```

## Comandos utilizados

### Verificar o estado do repositório

```bash
git status
```

### Listar branches

```bash
git branch
```

### Criar e acessar uma nova branch

```bash
git switch -c 02-git-branches
```

### Adicionar um arquivo à Staging Area

```bash
git add README.md
```

### Criar um commit

```bash
git commit -m "docs: adiciona histórico do laboratório 02"
```

### Voltar para a branch principal

```bash
git switch main
```

### Integrar uma branch à main

```bash
git merge 02-git-branches
```

### Enviar a main para o GitHub

```bash
git push origin main
```

### Excluir uma branch local já integrada

```bash
git branch -d 02-git-branches
```

## Staging Area

A Staging Area é a área de preparação do Git.

Ela permite selecionar quais alterações farão parte do próximo commit.

Fluxo básico:

```text
Arquivo modificado
        |
        v
git add
        |
        v
Staging Area
        |
        v
git commit
        |
        v
Histórico local
        |
        v
git push
        |
        v
GitHub
```

## Fast-forward Merge

Neste laboratório, o merge ocorreu no modo `fast-forward`.

Isso aconteceu porque a branch `main` não recebeu novos commits enquanto a branch de trabalho estava sendo desenvolvida.

O Git apenas avançou o ponteiro da `main` até o commit mais recente.

## Boas práticas aprendidas

- Não trabalhar diretamente na `main`.
- Criar uma branch para cada tarefa.
- Utilizar nomes claros e sem espaços ou acentos.
- Executar `git status` frequentemente.
- Criar commits pequenos e objetivos.
- Utilizar mensagens de commit descritivas.
- Integrar à `main` somente alterações concluídas e verificadas.
- Excluir branches locais que já foram integradas.

## Resultado

Foi criado e executado um fluxo completo com branches:

```text
Criar branch
→ editar
→ verificar
→ adicionar
→ criar commit
→ voltar para main
→ realizar merge
→ enviar ao GitHub
→ excluir branch concluída
```

## Lições aprendidas

Neste laboratório foi possível compreender que uma branch não é uma cópia completa do projeto, mas um ponteiro para uma linha de commits.

Também foi possível verificar que alterações realizadas em uma branch permanecem isoladas até serem integradas à `main`.

