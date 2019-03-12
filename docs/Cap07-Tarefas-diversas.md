# 7. Tarefas diversas

## 7.1. Como substituir alguma coisa por uma quebra de linha

No _Sed_ da GNU, a partir da versão **3.02.80**(*), foi adicionado o **`\n`** como escape válido dos dois lados do comando **`s///`**. Com isso a tarefa de colocar cada palavra numa linha isolada, ou seja, trocar espaços em branco por quebras de linha, fica trivial:

```
prompt$ sed 's/ /\n/g' texto.txt
```

Mas com outras versões do _Sed_ que não entendem este escape, a quebra de linha deve ser inserida **literalmente** e deve ser escapada:

```
prompt$ sed 's/ /\
prompt$ /g' texto.txt
```

Como curiosidade, a operação inversa, de colocar todas as linhas de um arquivo numa linha só, já é mais trabalhosa e utiliza o conceito de _laço_:

```
prompt$ sed ':a;$!N;s/\n/ /g;ta'
```

(*) veja o tópico [Nota sobre os adicionais GNU](https://aurelio.net/sed/sed-howto/#gnu)

## 7.2. Apagando linhas específicas

O comando para apagar linhas é o **`d`**.

O único detalhe nesta tarefa é especificar **quais** linhas você vai querer apagar. Isso está completamente coberto no tópico [O endereço](https://aurelio.net/sed/sed-howto/#endereco).

## 7.3. Como ignorar maiúsculas e minúsculas

O jeito padrão do _Sed_ ser "ignore-case", é dizendo literalmente todas as possibilidades, como em:

```
prompt$ sed '/[Rr][Oo][Oo][Tt]/d' texto.txt
```

Para apagar todas as linhas que contêm a palavra **`root, ROOT, RooT`** etc.

No _Sed_ da GNU, a partir da versão **3.01-beta1**(*), foi adicionado o modificador **`I`** no endereço e no comando **`s///`**, fazendo com que o comando acima fique mais simples:

```
prompt$ sed '/root/Id' texto.txt
```

Ou ainda:

```
prompt$ sed 's/root/administrador/Ig' texto.txt
```

(*) veja o tópico [Nota sobre os adicionais GNU](https://aurelio.net/sed/sed-howto/#gnu)

## 7.4. Lendo e gravando em arquivos externos

### 7.4.1. Lendo arquivos

Uma tarefa comum é incluir cabeçalho e rodapé num arquivo qualquer. O _Sed_ possui um comando específico para ler arquivos, o **`r`**, então basta(*):

```
prompt$ sed -e '1r cabecalho.txt' -e '$r rodape.txt' texto.txt
```

Para incluir o cabeçalho após a linha **1** e incluir o rodapé após a **última** linha.

(*) a explicação do porquê das opções **`-e`** está no tópico [Aplicando vários comandos de uma vez](https://aurelio.net/sed/sed-howto/#aplicar-varios-comandos).

7.4.2. Gravando arquivos

O comando **`w`** grava num arquivo a linha atual, ou melhor, o conteúdo do _espaço padrão_. Por exemplo, você quer gravar num arquivo o resultado de uma busca por linhas que contêm a palavra `estorvo`. A solução não-_Sed_ seria:

```
prompt$ grep 'estorvo' texto.txt > estorvos.txt
```

Nosso similar em _Sed_ seria:

```
prompt$ sed '/estorvo/w estorvos.txt' texto.txt
```

Gravar dados num arquivo também pode servir de **espaço auxiliar** caso o espaço _reserva_ não seja suficiente. Mas esta é uma opção drástica, não tão flexível. Mais informações sobre o _espaço reserva_ no tópico [Conhecendo os registradores internos](https://aurelio.net/sed/sed-howto/#registradores-internos).

## 7.5. Trocando um trecho de texto por outro

Uma tarefa que parece simples mas confunde, é trocar um trecho de texto, como um parágrafo inteiro por exemplo, por outro trecho, independente do número de linhas de ambos.

### 7.5.1. Trocar várias linhas por uma

Essa é simples, basta usar o comando **`c`**, que "Coloca" um texto no lugar da linha atual. A única complicação é definir o _endereço_, para aplicar o comando apenas nas linhas desejadas. Por exemplo, vamos colocar uma frase no lugar de uma área de texto pré-formatado num documento HTML. Esta área é delimitada pelos identificadores **`<pre>`** e **`</pre>`**:

```
prompt$ sed '/<pre>/,/<\/pre>/c \
prompt$ aqui tinha texto pré-formatado' texto.html
```

Note que o comando **`c`** (assim como o **`a`** e o **`i`**) **exige** que o texto que ele recebe como parâmetro esteja na linha seguinte, estando a quebra de linha escapada com a barra invertida **`\`**

No _Sed_ da GNU, a partir da versão **3.02a**(*), é permitido que se coloque o texto na mesma linha:

```
prompt$ sed '/<pre>/,/<\/pre>/c aqui tinha texto pré-formatado' texto.html
```

(*) veja o tópico [Nota sobre os adicionais GNU](https://aurelio.net/sed/sed-howto/#gnu)

### 7.5.2. Trocar várias linhas por outras

Similarmente a trocar por apenas uma linha, pode-se usar o comando **`c`** e passar várias linhas para ele. O único detalhe é que todas as linhas devem ser **escapadas** no final, menos a última:

```
prompt$ sed '/<pre>/,/<\/pre>/c \
prompt$ aqui tinha texto pré-formatado,\
prompt$ mas eu resolvi tirar.\
prompt$ porque?\
prompt$ porque sim' texto.html
```

É claro, quando o comando começa a ficar grande desse jeito, é melhor colocá-lo num arquivo. Saiba mais detalhes sobre isso no tópico [Colocando comandos sed num arquivo](https://aurelio.net/sed/sed-howto/#comandos-em-arquivo).

Mas melhor ainda é separar o comando _Sed_ do texto, colocando-o num arquivo separado. Assim, quando se precisar alterar este texto, basta editá-lo, sem mudar o comando _Sed_, e sem precisar ficar colocando **`\`** no final de cada linha.

Supondo que nosso texto explicativo do porquê da retirada do texto pré-formatado foi gravado no arquivo _desculpa.txt_, utilizaremos o comando **`r`** para lê-lo e o comando **`d`** para apagar o texto antigo:

```
prompt$ sed -e '/<\/pre>/r desculpa.txt' -e '/<pre>/,/<\/pre>/d' texto.html
```

Então acompanhe o que acontece: o primeiro comando será executado apenas na linha **`</pre>`** que é o fechamento do trecho, então vamos esquecer dele por enquanto. O segundo comando diz para apagar o trecho desde **`<pre>`** até **`</pre>`**, então assim que começar o trecho, ele vai apagando, linha por linha.

Ao chegar na linha que contém o **`</pre>`**, o primeiro comando _Sed_ entra em ação e lê o arquivo _desculpa.txt_, colocando seu conteúdo imediatamente após a linha atual. Em seguida, o segundo comando apaga a linha **`</pre>`**, completando a tarefa.

Esta segunda solução é mais difícil de entender e implementar, mas é muito mais prática caso a alteração do texto a ser colocado seja freqüente, além destas alterações poderem ser feitas por alguém que nem saiba o que é _Sed_, pois será apenas um texto normal.

Note que sempre que o **`</pre>`** foi referenciado nos _endereços_, a barra foi escapada, ficando **`<\/pre>`**. A explicação desse escape está em [Usando outros delimitadores](https://aurelio.net/sed/sed-howto/#outros-delimitadores).

**obs.:** talvez o **`<pre></pre>`** não seja um exemplo dos mais didáticos, mas não me veio algo mais comum à mente...
7.6. Emulando outros comandos

Aqui vão alguns exemplos de emulações de outros comandos usando-se o _Sed_:

    comando       emulação
    cat           sed :
    head          sed 10q
    grep          sed /padrão/!d
    grep -v       sed /padrão/d
    tac 	      sed 1!G;h;$!d
    tail -1       sed $!d
    tr A-Z a-z    sed y/ABCDEF...UVWXYZ/abcdef...uvwxyz/
    wc -l         sed -n $=

A lista completa e atualizada pode ser encontrada em: http://sed.sourceforge.net/local/docs/emulating_unix.txt
