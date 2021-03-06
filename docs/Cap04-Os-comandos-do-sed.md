# 4. Os comandos do sed


## 4.1. Descrição de todos os comandos

```shell
prompt$ man sed
prompt$ pinfo sed
```

Ou num resumo rápido:

Legenda:

     [ARQUIVO]       arquivo ou fluxo de texto (via pipe) original a ser modificado
     [TEXTO]         trecho de texto. pode ser uma palavra, uma linha,
                     várias separadas por \n, ou mesmo um vazio.
     [PADRÃO]        [TEXTO] contido no ESPAÇO PADRÃO


     =       imprime o número da linha atual do [ARQUIVO]
     #       inicia um comentário
     !       inverte a lógica do comando
     ;       separador de comandos
     ,       separador de faixas de endereço
     {       início de bloco de comandos
     }       fim de bloco de comandos

     s       substitui um trecho de texto por outro
     y       traduz um caractere por outro

     i       insere um texto antes da linha atual
     c       troca a linha atual por um texto
     a       anexa um texto após a linha atual

     g       restaura o [TEXTO] contido no ESPAÇO RESERVA (sobrescrevendo)
     G       restaura o [TEXTO] contido no ESPAÇO RESERVA (anexando)
     h       guarda o [PADRÃO] no ESPAÇO RESERVA (sobrescrevendo)
     H       guarda o [PADRÃO] no ESPAÇO RESERVA (anexando)
     x       troca os conteúdos dos ESPAÇO PADRÃO e RESERVA

     p       imprime o [PADRÃO]
     P       imprime a primeira linha do [PADRÃO]
     l       imprime o [PADRÃO] mostrando caracteres brancos

     r       inclui conteúdo de um arquivo antes da linha atual
     w       grava o [PADRÃO] num arquivo

     :       define uma marcação
     b       pula até uma marcação
     t       pula até uma marcação, se o último s/// funcionou (condicional)

     d       apaga o [PADRÃO]
     D       apaga a primeira linha do [PADRÃO]
     n       vai para a próxima linha
     N       anexa a próxima linha no [PADRÃO]
     q       finaliza o Sed imediatamente

## 4.2. Lista de todos os comandos por categoria

                   informações =
                    marcadores :
                   comentários #
            comandos de edição s i c a y
     comandos de registradores g G h H x
         comandos de impressão p P l
           comandos de arquivo r w
                 modificadores g i !
                   separadores ; -e \n
             controle de fluxo b t d D n N q
                      endereço // ,
                   limitadores {} \(\)
       registradores dinâmicos \1 \2 ... \9
