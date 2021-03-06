## 5. Conceitos básicos

Aqui vão conhecimentos que você **precisa** assimilar para fazer um bom uso do _Sed_.

## 5.1. Suprimindo a saída padrão

### 5.1.1. Saída normal

Normalmente o _Sed_ sempre imprime na **saída padrão** todas as linhas do arquivo, modificadas ou não.

Veja o primeiro exemplo citado:

```shell
prompt$ sed 'p' texto.txt
```

O comando **`p`** imprime a linha na saída padrão. Este exemplo **duplica** todas as linhas do arquivo pois, além da impressão normal de cada linha, ainda é aplicado o comando **`p`** em cada uma, que diz "imprima esta linha", fazendo com que ela apareça duas vezes.

### 5.1.2. Saída suprimida

Temos como modificar este comportamento com a opção **`-n`**, que significa "não imprima na saída, a não ser quando especificado com o comando **`p`** ou o **`l`**".

Assim sendo, colocando o **`-n`**, eliminamos o comportamento padrão de "imprimir sempre na saída":

```shell
prompt$ sed -n 'p' texto.txt
```

Que resulta no conteúdo do arquivo, pois cada linha é impressa apenas uma vez, devido ao comando **`p`**. Assim fica fácil entender como funciona o exemplo já citado que imprime apenas a **linha 5** de um arquivo:

```shell
prompt$ sed -n 5p texto.txt
```

Ok, a explicação daquele **5** ali perdido vem logo a seguir no tópico O endereço :)

## 5.2. O endereço

O endereço serve para você dizer ao _Sed_ para aplicar um determinado comando **apenas**     nas linhas informadas. Este endereço pode ser descrito direto como o **número** da linha, ou por **parte** de seu conteúdo (entre _/barras/_).

Caso o endereço não seja informado, o comando _Sed_ será aplicado para **todas** as linhas.

### 5.2.1. Endereço simples

Por exemplo, referenciando a linha pelo seu número, como já foi visto anteriormente:

```shell
prompt$ sed '5d' texto.txt
```

Mas também poderia ser uma linha que tivesse uma palavra qualquer:

```shell
prompt$ sed '/estorvo/d' texto.txt
```

O comando **`d`** apaga linhas segundo o endereço, então este comando apagará todas as linhas que tiverem a palavra `estorvo`. Este exemplo tem o funcionamento idêntico ao comando:

```shell
prompt$ grep -v estorvo texto.txt
```

### 5.2.2. Intervalo

Como endereço, ainda se pode especificar um **intervalo**, como da linha 5 até a linha 10, ou da linha 5 até a linha que tiver a palavra `estorvo`:

```shell
prompt$ sed '5,10d' texto.txt
prompt$ sed '5,/estorvo/d' texto.txt
```

No endereço, temos um caractere especial, o **`$`** que referencia à **última** linha do texto. Assim sendo, para apagar da **linha 10** até o **final** do texto, o comando é:

```shell
prompt$ sed '10,$d' texto.txt
```

### 5.2.3. Outros

No _Sed_ da GNU, a partir da versão **3.02a**(*), foi adicionada uma maneira nova de especificar um endereço:

```shell
prompt$ sed '/estorvo/,+3d' texto.txt
```

Que referencia a linha que contém a palavra estorvo e mais as **3** linhas seguintes.

E pra finalizar, como já dito anteriormente, quando o comando **não** tem endereço, é aplicado para todas as linhas:

```shell
prompt$ sed 'd' texto.txt
```

(*) veja o tópico [Nota sobre os adicionais GNU](https://aurelio.net/sed/sed-howto/#gnu)

## 5.3. Interrompendo o processamento

A qualquer hora você pode **abortar** o comando _Sed_ com o comando **`q`**.

Isso é útil no nosso exemplo anterior de emular o comando _head_, imprimindo apenas as 10 primeiras linhas do arquivo:

```bash
sed '10q'         # ao chegar na linha 10, pare.
```

Ou ainda, para obter apenas os cabeçalhos de uma mensagem de e-mail, que são separados do corpo da mensagem por uma linha em branco:

```bash
sed '/^$/q'       # pare na primeira linha em branco que achar
```

## 5.4. Invertendo a lógica

No _Sed_ temos o modificador **`!`** que inverte a lógica do comando, ou seja `!comando` significa "não execute o comando". É meio estranho a primeira vista, mas você tem que começar a pensar como o _Sed_, e tudo se esclarece :)

Temos o comando _head_ que imprime as 10 primeiras linhas de um arquivo. Com as dicas já vistas, podemos fazer esta tarefa com o _Sed_ assim:

```bash
sed -n '1,10p'    # imprima apenas da linha 1 até a 10
sed '11,$d'       # apague da linha 11 até o final
```

Ou ainda, podemos inverter a lógica e fazer:

```bash
sed '1,10!d'      # NÃO apague da linha 1 até a 10 (ou seja, apague as outras)
sed -n '11,$!p'   # NÃO imprima da linha 11 até o final (ou seja, imprima as outras)
```

A dica é sempre complementar a leitura mental com o inverso (entre parênteses nos exemplos), ou seja, se o _Sed_ NÃO vai aplicar um comando em determinadas linhas, isso quer dizer implicitamente que este comando **será aplicado** em todas as outras linhas. É estranho, mas acostuma :)

## 5.5. Aplicando vários comandos de uma vez

### 5.5.1. Comandos normais

É possível aplicar vários comandos _Sed_, em **seqüência**. Basta separá-los por ponto-e-vírgula.

```shell
prompt$ sed '5d;10d;/estorvo/d' texto.txt
```

Este comando apaga as linhas 5, 10 e as que têm `estorvo` do arquivo _texto.txt_.

### 5.5.2. Comandos com parâmetros

Os comandos que recebem parâmetros **`(r, w, i, a, c)`**, não aceitam o ponto-e-vírgula como separador, pois este pode ser parte integrante do parâmetro esperado.

Estes comandos devem ser separados dos restantes, sendo passados como comandos isolados, pela opção de linha de comando **`-e`**:

```shell
prompt$ sed -e '1i começo de tudo' -e '5d' texto.txt
```

Este comando insere a frase começo de tudo antes da primeira linha e apaga a quinta linha do arquivo texto.txt.

### 5.5.3. Terceira via

Outra maneira de especificar vários comandos (e a mais consistente e garantida) é colocá-los num arquivo, um por linha. Veja o tópico [Colocando comandos Sed num arquivo](https://aurelio.net/sed/sed-howto/#comandos-em-arquivo).
