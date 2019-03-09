# 3. Perdendo o medo do sed

## 3.1. Como ele funciona

O _Sed_ funciona como um filtro, por onde você passa um texto `X` e ele joga na saída um texto `Y`.

O texto `X` virou `Y` seguindo algumas regrinhas que você determinou.

Pense no _Sed_ como um processador de alimentos, dependendo da lâmina utilizada, a batata sai cortada de uma maneira diferente :)

- o _Sed_ funciona como um filtro, ou conversor.
- o _Sed_ é orientado a linha, de cima para baixo, da esquerda para a direita.
- o _Sed_ lê uma linha da entrada padrão (STDIN) ou de um arquivo especificado, aplica os comandos de edição e mostra o resultado na saída padrão (STDOUT). vai para a próxima linha e repete o processo.
- o _Sed_ aceita endereços para os comandos.
- o _Sed_ aplica os comandos para todas as linhas caso um endereço não seja especificado.
- o _Sed_ faz uso intensivo de expressões regulares.
- o _Sed_ é macho :)

## 3.2. Sua sintaxe

A sintaxe genérica de um comando _Sed_ é:

`sed [opções] regras [arquivo]`

Sendo que `regras` tem a forma genérica de:

`[endereço1 [, endereço2]] comando [argumento]`

### 3.2.1. Exemplo

Como notação tradicional, o que está `[entre colchetes]` é opcional, então a sintaxe _Sed_ mais simples que existe é `sed regra` como em:

`prompt$ cat texto.txt | sed p`

Ou seja, o _Sed_ lendo da entrada padrão o conteúdo do arquivo _texto.txt_ via duto `|`, aplica o comando **p** para todas as linhas do arquivo, ou seja, as duplica.


### 3.2.2. Outros exemplos

Um outro exemplo do _Sed_ com `opções` e recebendo um arquivo como parâmetro seria:

`prompt$ sed -n p texto.txt`

E ainda, agora especificando um **endereço** para o comando **p**:

`prompt$ sed -n 5p texto.txt`

Ou seja, este comando imprime apenas a **linha 5** do _texto.txt_


## 3.3. Como executá-lo

A execução do _Sed_ é igual a de outro aplicativo qualquer de manipulação de texto, aceitando como parâmetro um nome de arquivo, ou na falta deste, lê o texto da **entrada padrão**, via duto `|` ou redirecionamento `<`.

E como dica geral **SEMPRE** coloque os comandos do _Sed_ entre aspas simples `''`, para evitar que o _shell_ os interprete erroneamente. Veja mais detalhes no tópico [Sed e shell](https://aurelio.net/sed/sed-howto/#sed-e-shell).

`prompt$ sed 'p' texto.txt`
`prompt$ cat texto.txt | sed 'p'`
`prompt$ sed 'p' < texto.txt`

Com outra opção ainda, pode-se executar diretamente um arquivo com comandos _Sed_. Para mais informações, veja o [Tornando arquivos Sed executáveis](https://aurelio.net/sed/sed-howto/#arquivos-executaveis).
