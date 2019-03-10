# 6. Conceitos complementares

Estes são conhecimentos que possivelmente surgirão como dúvidas em sua cabeça após utilizar o _Sed_ por um tempo.

## 6.1. _Sed_ e shell

Com o _Sed_ sendo invocado na linha de comando, deve-se ter alguns cuidados para evitar transtornos. O interpretador de comandos (shell), interpreta a linha de comando antes de processá-la, então alguns caracteres especiais como **`$`**, **`\`** e **`!`**, são interpretados pelo shell **antes** de chegarem ao _Sed_, modificando o comportamento esperado.

Para evitar isso coloque os comandos _Sed_ **sempre** entre aspas simples:

```
prompt$ sed 's/isso/aquilo/' texto.txt
```

Salvo quando no meio do comando _Sed_, existir algo que deva ser interpretado, como uma variável por exemplo. Neste caso coloque os comandos entre aspas duplas:

```
prompt$ sed "s/$HOME/aquilo/" texto.txt
```

Ou ainda, para evitar completamente a interpretação do shell, sem se preocupar com aspas, coloque os comandos _Sed_ num arquivo. Veja o tópico [Colocando comandos Sed num arquivo](https://aurelio.net/sed/sed-howto/#comandos-em-arquivo).

## 6.2. Usando outros delimitadores

### 6.2.1. No comando s

É comum ao fazer um comando de substituição **`s///`** conter uma **`/`** num dos dois lados do comando, como quando querendo substituir **`/usr/local/bin`** por **`/usr/bin`**.

Sendo a barra o delimitador do comando **`s`** as outras barras comuns devem ser escapadas com a barra invertida **`\`**, para não serem confundidas com os delimitadores normais, ficando o monstro a seguir:

```
prompt$ sed 's/\/usr\/local\/bin/\/usr\/bin/' texto.txt
```

Para evitar ter que ficar se escapando todas estas barras, basta lembrar que o comando **`s`** aceita **qualquer** delimitador, sendo a barra apenas um padrão de referências históricas. Então, neste caso, poderíamos escolher outro delimitador como por exemplo a vírgula:

```
prompt$ sed 's,/usr/local/bin,/usr/bin,' texto.txt
```

Evitando-se de ter que ficar escapando as barras. A mesma dica vale para o comando **`y`**.

### 6.2.2. No endereço

E se precisássemos apagar as linhas que contém o **`/usr/local/bin`**? Teríamos que colocar o nome do diretório no endereço do comando **`d`**, voltando com a festa dos escapes:

```
prompt$ sed '/\/usr\/local\/bin/d' texto.txt
```

Para usarmos outro delimitador no endereço, basta escaparmos o primeiro, que no caso abaixo é a vírgula:

```
prompt$ sed '\,/usr/local/bin,d' texto.txt
```

Confusão de delimitadores com o texto a ser procurado é muito comum de acontecer, então se algo não está funcionando como deveria, olhe com cuidado para ver se não há conflitos entre eles.

## 6.3. Escapes para caracteres especiais

No _Sed_ da GNU, a partir da versão **3.02.80**(*), vários escapes novos foram adicionados e podem ser usados nas duas partes do comando **`s///`**:

    \a      beep             (apito)
    \f      form-feed        (avança linha)
    \n      newline          (quebra de linha)
    \r      carriage-return  (retorno de carro)
    \t      hTAB             (tabulação horizontal)
    \v      vTAB             (tabulação vertical)
    \oNNN   o caractere de valor octal NNN
    \dNNN   o caractere de valor decimal NNN
    \xNN    o caractere de valor hexadecimal NN

(*) veja o tópico [Nota sobre os adicionais GNU](https://aurelio.net/sed/sed-howto/#gnu)

## 6.4. Gravando o resultado no mesmo arquivo

### 6.4.1. Problema inicial

O procedimento comum quando se quer gravar num arquivo o resultado de um comando _Sed_, é o redirecionamento:

```
prompt$ sed 'comando' texto.txt > texto-alterado.txt
```

Mas é muito comum, ao alterarmos um arquivo, queremos gravar estas alterações no **próprio** arquivo original. A tentativa intuitiva seria:

```
prompt$ sed 'comando' texto.txt > texto.txt
```

Mas é só fazer para ver. Além de não dar certo, você ainda perderá **todo** o conteúdo do arquivo.

Isso acontece porque ao fazer o redirecionamento **`>`**, o `shell` abre imediatamente o arquivo referenciado, antes mesmo de começar a executar o comando _Sed_. E como este é um redirecionamento destrutivo **`>`** e não incremental **`>>`**, se o arquivo já existir, ele será truncado, e seu conteúdo perdido. A essa altura, o _Sed_ começará seu processamento já lendo um arquivo _texto.txt_ vazio, e aplicados qualquer comandos _Sed_ num arquivo vazio, o resultado será o próprio arquivo vazio.

### 6.4.2. Solução genérica

Para evitar isso, voltamos a primeira tática de gravar o resultado num outro arquivo, e depois o mais natural é **mover** o arquivo novo sobre o original:

```
prompt$ sed 'comando' texto.txt > texto-alterado.txt
prompt$ mv texto-alterado.txt texto.txt
```

Para a grande maioria dos casos, isso é suficiente, mas convém aqui lembrar que caso o arquivo 'texto.txt' possua atributos especiais, grupo diferente do padrão do usuário, ou referências (links, simbólicos ou não) para outros arquivos, tudo isso **será perdido**. Ao mover o arquivo recém-criado, com os atributos padrão do sistema, sobre o original, este perderá seus atributos e ficará com os padrões do sistema, **herdado** do arquivo novo.

### 6.4.3. Solução segura

Para evitar isso, a abordagem mais ortodoxa e segura seria aplicar o comando _Sed_ numa cópia e gravar o resultado no arquivo original via redirecionamento:

```
prompt$ cp -a texto.txt texto-tmp.txt
prompt$ sed 'comando' texto-tmp.txt > texto.txt
prompt$ rm texto-tmp.txt
```

Novamente, isso só é necessário com arquivos especiais, senão a solução com o mv pode ser usada. Mas é importante ter em mente esta outra maneira e principalmente saber o porque de utilizá-la, sendo este conhecimento aplicável a qualquer outro comando do sistema que leia e grave arquivos.
