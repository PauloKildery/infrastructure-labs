# Laboratório 03 — Git Merge

## Objetivo

Estudar e praticar o processo de integração de branches utilizando o comando `git merge`.

Neste laboratório serão abordados:

- Fast-forward merge
- Merge commit
- Three-way merge
- Conflitos de merge
- Resolução manual de conflitos
- Análise do histórico com `git log`

## Observações

Primeira versão do Laboratório 03.

## Fast-Forward Merge

O fast-forward ocorre quando a branch principal não recebeu novos commits após a criação da branch de trabalho. Nesse caso, o Git apenas avança o ponteiro da branch principal.

## Merge Commit

O Merge Commit ocorre quando a branch principal e a branch de trabalho recebem commits diferentes após o ponto em comum.

Nesse cenário, o Git não pode apenas avançar o ponteiro da branch principal. Ele precisa combinar os dois históricos e criar um novo commit com dois commits-pai.

Exemplo executado neste laboratório:

```bash
git switch main
git merge docs/exemplo-merge-commit

## Merge Commit

O Merge Commit ocorre quando a branch principal e a branch de trabalho recebem commits diferentes após o ponto em comum.

Nesse cenário, o Git não pode apenas avançar o ponteiro da branch principal. Ele precisa combinar os dois históricos e criar um novo commit com dois commits-pai.

Exemplo executado neste laboratório:

```bash
git switch main
git merge docs/exemplo-merge-commit

