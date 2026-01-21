---
title: "Editor Vim no dia a dia: motions e melhor usabilidade"
description: "Comandos essenciais para utilizar o vim (vim motions) e configuração do vimrc para melhor usabilidade"
date: 2026-01-20
tags: ["vim", "linux", "editor", "cli", "productivity", "vimrc", "motions", "produtividade"]
categories: ["Vim", "Linux", "DevOps"]
draft: false
---

# Editor Vim no dia a dia: motions práticos e um vimrc para melhorar sua usabilidade

## Introdução

Se você é usuário de Linux ou possui um contato mais próximo com o sistema, com certeza já sofreu em algum grau com a utilização do [vim](https://www.vim.org/). E, pensando nisso, escrevi este artigo a fim de ajudar os colegas com a sua utilização! =)

Primeiro, vamos melhorar a utilização do vim, ele vem muito cru e isso dificulta a usabilidade!

#### A configuração abaixo irá:

* Exibir o número das linhas

* Ignorar case-sensitive 

* Deixar uma linha no rodapé do arquivo no padrão

#### Padrão de como ficará o rodapé:

> /caminho/do/arquivo/em_edicao [NUM_LINHA:POSICAO_COLUNA] [/caminho/do/diretorio_atual]


Exemplo, se eu estou no meu diretório home (PWD) e edito o arquivo sshd_config e modifico a linha 66, no rodapé do arquivo ele exibirá [+], indicando que ele foi modificado:

> /etc/ssh/sshd_config [66:1] [/home/marcelo]

![config-vimrc](./img/vim-motions/config-vimrc.png)

---

## Editando o vimrc

Ok, pra isso, edite o `/etc/vim/vimrc` e adicione as seguintes linhas ao final do arquivo (comentários são feitos com aspas):

```
" Exibe o número da linha à direita
set nu

" Ignora case-sensitive nos patterns
set ignorecase

" --- Cores ---
highlight StatusFile     ctermfg=12   ctermbg=NONE " azul
highlight StatusDir      ctermfg=10   ctermbg=NONE " verde
highlight StatusRO       ctermfg=11   ctermbg=NONE " amarelo
highlight StatusMod      ctermfg=9    ctermbg=NONE " vermelho
highlight StatusPos      ctermfg=14   ctermbg=NONE " ciano

" --- Statusline ---
set statusline=                                        " reinicia a definicao da statusline
set laststatus=2                                       " mantem a statusline sempre visivel
set statusline+=%#StatusFile#%F\                       " caminho do arquivo que estah sendo editado
set statusline+=%#StatusMod#%{&modified?'[+]\ ':''}    " exibe [+] se modificado algo no arquivo
set statusline+=%#StatusRO#%{&readonly?'[RO]\ ':''}    " exibe [RO] se eh somente leitura ou sem permissao de escrita
set statusline+=%#StatusPos#[%l:%c]\                   " exibe linha:coluna
set statusline+=%#StatusDir#[%{getcwd()}]              " exibe diretorio atual, o PWD do meu usuario
```

Ótimo, agora o seu vim já tá bem melhor de usar!

---

## Vim motions

Vamos pr'os vim motions agora (atente-se porque alguns começam com dois-pontos!):

### Navegação

- `gg` — vai para a primeira linha
- `G` — vai para a última linha
- `:[num_linha]<Enter>` — vai para a linha **num_linha** (ex.: `:42<Enter>`)
- `w` — pula para a próxima palavra
- `b` — puala para a palavra anterior

### Inserção e desfazer/refazer

- `i` ou `Tecla Insert` — entra no modo *insert* (inserção de texto); ao teclar 2x Insert, entra no modo *replace*, para substituição de texto
- `u` — desfaz a(s) última(s) alteração(ões) (*undo*); a cada `u` ele volta uma alteração
- `Ctrl+r` — refaz (*redo*) o que foi desfeito com o `u`

### Busca

- `/texto` — busca o pattern para frente
- `?texto` — busca o pattern para trás (ou apenas volte até a primeira linha e utiliza a barra mesmo rsrs)
- `n` — próximo resultado (next pattern)
- `Shift + N` — resultado anterior

### Substituição

- `:s/antigo/novo/g` — substitui a string *antigo* para a string *novo* da linha atual
- `:%s/antigo/novo/g` — substitui no arquivo todo

### Edição e clipboard

- `dw` — apaga do cursor até o fim da palavra
- `diw` — apaga a palavra inteira (independente da posição do cursor)
- `yy` — copia a linha (*yank*)
- `d` — recorta/apaga (*delete*)
- `p` — cola depois do cursor
- `Shift + P` — cola antes do cursor
- `v` — habilita o *modo visual* (seleção); você faz a seleção do texto até onde quiser, e combina direto com outro comando, como o de copiar, cortar, etc.


Para executar algum comando num range de linhas, é só separar por vírgula. Por exemplo, para substituir todas as strings "windows" para "debian" das linhas 26 a 34, faça:

- `:26,34s/windows/debian/g` — substitui todas as strings *windows* para *debian* entre as linhas 26 e 34

Isso também se aplica aos outros comandos, como o de cópia e recorte.

### Comandos essenciais: salvar e sair

Ah! E, claro, depois de editar todo o arquivo, você precisa salvar e sair, né? O clássico problema de quem usa vim pela primeira vez... rsrs

- `esc` — "volta ao controle", sai do modo insert ou visual
- `w` — salva o arquivo (*write*)
- `q` — sai do arquivo (*quit*)
- `wq` ou `x` — salva e sai do arquivo
- `wq!` ou `x!` — salva e sai do arquivo forçando

Pronto, agora você já consegue usar o vim com mais tranquilidade :)
