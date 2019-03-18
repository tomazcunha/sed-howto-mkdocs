# 8. Conceitos avançados

Estes são conhecimentos necessários àqueles que fazem uso intensivo do _Sed_, fazendo programas grandes e/ou complexos.

## 8.1. Monitorando um arquivo

No _Sed_ da GNU, a partir da versão **3.02.80**(*), foi adicionada a opção **`-u`**, que significa "unbuffered", ou seja, faz um uso minimalista dos registradores, mostrando a saída o mais rápido possível, tornando possível editar um fluxo interminável como o gerado por um _tail -f_.

Um exemplo prático seria mostrar apenas as mensagens do sistema relativas às conexões _ssh_:

```shell
prompt$ tail -f /var/log/messages | sed -nu '/sshd/p'
```

Cuidado com **`-nu`** perto de crianças! :)

(*) veja o tópico [Nota sobre os adicionais GNU](https://aurelio.net/sed/sed-howto/#gnu)

## 8.2. Colocando comandos _Sed_ num arquivo

Como os comandos _Sed_ vão ficando extensos e complicados, é conveniente colocá-los **num arquivo**, com estruturação e comentários.

Você pode espalhar os comandos por várias linhas, trocando o **`;`** por quebras de linha e colocar comentários precedidos de **`#`**. O exemplo de apagar linhas ficaria:

```shell
# programa.sed: apaga algumas linhas

# apaga a 5ª linha
5d

# apaga a 10ª linha
10d

# apaga as linhas que contêm 'estorvo'
/estorvo/d
```

Para dizer ao _Sed_ para utilizar aquele arquivo como fonte de comandos, basta usar a opção **`-f`**

```shell
prompt$ sed -f programa.sed texto.txt
```

## 8.3. Tornando arquivos _Sed_ executáveis

O interpretador de comandos mais utilizado (bash) sempre procura na **primeira** linha de um arquivo instruções para executá-lo.

Se um arquivo é um programinha em _shell_, basta colocar

```shell
#!/bin/sh
```

Na primeira linha para que o _bash_ saiba que deve executá-lo com o comando **`/bin/sh`**. O mesmo funciona para qualquer outro interpretador, como o _Sed_. então para tornar um arquivos de comandos _Sed_ executável basta colocar como primeira linha:

```shell
#!/bin/sed -f
```

E é claro, torná-lo executável:

```shell
prompt$ chmod +x programa.sed
```

E na linha de comando, chame-o normalmente:

```shell
prompt$ ./programa.sed texto.txt
prompt$ cat texto.txt | ./programa.sed
```

## 8.4. Conhecendo os registradores internos

### 8.4.1. Apresentação

O _Sed_ possui 2 registradores ("buffers") internos, que são usados para a manipulação do texto.

Um deles é o _espaço padrão_ ("pattern space"), que é o registrador utilizado normalmente pelo _Sed_. É nele que a linha a ser processada é armazenada e manipulada.

O outro é o _espaço reserva_ ("hold space"), que é um registrador auxiliar, inicialmente vazio, que serve para guardar uma cópia da linha original, parte dela, ou agrupar dados diversos de várias linhas.

Há comandos para fazer a troca de dados entre os dois registradores:

    h      guarda no espaço reserva
    H      guarda (anexando) no espaço reserva

    g      pega o conteúdo do espaço reserva
    G      pega (anexando) o conteúdo do espaço reserva

    x      troca os conteúdos dos 2 registradores

O _anexando_ acima significa "não sobrescreve o conteúdo original", ou seja, ele mantém o que já tem, e adiciona um **`\n`** (quebra de linha), seguido do texto manipulado. Para entender melhor, veja o exemplo gráfico a seguir.

### 8.4.2. Exemplo

Um exemplo didático de uso do _espaço reserva_ é ir guardando nele algumas linhas do texto e mostrá-las depois no final do arquivo:

```shell
prompt$ sed '/root/H;$g' /etc/passwd
```

Ou seja, adicione no _espaço reserva_ (comando **`H`**), as linhas que contêm a palavra **`root`** e na última linha do arquivo (endereço **`$`**), recupere o conteúdo do _espaço reserva_ (comando **`g`**).

### 8.4.3. Exemplo gráfico

Como os registradores são a parte mais **obscura** do _Sed_ (mais por falta de documentação do que por complexidade), merecem uma explicação **bem** didática. Vamos lá.

Temos os dois registradores vazios: (que daqui pra frente serão chamados apenas de _padrão e reserva_)

```
     __________________                __________________
    |                  |              |                  |
    |                  |              |                  |
    |__________________|              |__________________|
       espaço padrão                     espaço reserva
```

E um arquivo hipotético com o conteúdo: (não são odiosos estes exemplos com frutas?)

```bash
laranja
uva
abacaxi
melancia
mimosa
```

E aplicaremos o comando:

```bash
sed '/laranja/h ; /uva/g ; /abacaxi/H ; /melancia/G ; /mimosa/x'
```

Obtendo como resultado:

```bash
laranja
laranja
abacaxi
melancia
laranja
abacaxi
laranja
abacaxi
```

Vejamos o que aconteceu. Lida a primeira linha **`laranja`**, ela é imediatamente colocada no _padrão_ para ser manipulada:

```
     __________________                __________________
    |                  |              |                  |
    |      laranja     |              |                  |
    |__________________|              |__________________|
       espaço padrão                     espaço reserva
```

O comando direcionado a ela é o **`h`**, que guarda uma cópia dela no _reserva_:

```
     __________________                __________________
    |                  |              |                  |
    |      laranja     |   -- h -->   |      laranja     |
    |__________________|              |__________________|
       espaço padrão                     espaço reserva
```

Como mais nenhum comando é relativo à linha **`laranja`**, o _Sed_ dá por encerrado o processamento dessa linha e imprime o conteúdo do _padrão_ na saída: "laranja".

Beleza, agora ele vai processar a segunda linha, novamente a primeira coisa é colocá-la no _padrão_, sobrescrevendo o que tinha antes:

```
     __________________                __________________
    |                  |              |                  |
    |        uva       |              |      laranja     |
    |__________________|              |__________________|
       espaço padrão                     espaço reserva
```

O _reserva_, enquanto nenhum outro comando escrever nele, permanecerá o mesmo. O comando direcionado à linha **`uva`** é o **`g`**, que pega o conteúdo do _reserva_ e o coloca no _padrão_, apagando o que estiver nele (neste caso: _uva_):

```
     __________________                __________________
    |                  |              |                  |
    |      laranja     |   <-- g --   |      laranja     |
    |__________________|              |__________________|
       espaço padrão                     espaço reserva
```

Novamente, não há mais comandos a ser executados, então imprime na saída o conteúdo do _padrão_: "laranja".

Indo para a terceira linha e colocando-a no _padrão_:

```
     __________________                __________________
    |                  |              |                  |
    |      abacaxi     |              |      laranja     |
    |__________________|              |__________________|
       espaço padrão                     espaço reserva
```

O comando dessa linha é o **`H`**, que tal como o **`h`**, guarda o conteúdo do _padrão_ no _reserva_, com diferença que ele preserva o conteúdo já existente dele, separando com um **`\n`**:

```
     __________________                __________________
    |                  |              |                  |
    |      abacaxi     |   -- H -->   | laranja\nabacaxi |
    |__________________|              |__________________|
       espaço padrão                     espaço reserva
```

Novamente, chegou ao fim, imprime o _padrão_: "abacaxi". a próxima linha é a da **`melancia`**:

```
     __________________                __________________
    |                  |              |                  |
    |     melancia     |              | laranja\nabacaxi |
    |__________________|              |__________________|
       espaço padrão                     espaço reserva
```

E agora vai ficar divertido, aplicando o comando **`G`**, que pega o conteúdo do _reserva_ e anexa ao _padrão_:

```
     ____________________________           __________________
    |                            |         |                  |
    | melancia\nlaranja\nabacaxi |  <-G--  | laranja\nabacaxi |
    |____________________________|         |__________________|
             espaço padrão                    espaço reserva
```

E a saída agora fica "melancia\nlaranja\nabacaxi", com o detalhe que o _Sed_ troca estes **`\n`** por quebras de linha na impressão. Então são 3 linhas na saída. Vá acompanhando com o resultado que já foi cantado antecipadamente lá em cima.

E finalmente, a última linha:

```
     __________________                __________________
    |                  |              |                  |
    |      mimosa      |              | laranja\nabacaxi |
    |__________________|              |__________________|
       espaço padrão                     espaço reserva
```

E para ela, o comando que troca o conteúdo dos 2 registradores, o **`x`**:

```
     __________________                __________________
    |                  |              |                  |
    | laranja\nabacaxi |  <-- x --->  |      mimosa      |
    |__________________|              |__________________|
       espaço padrão                     espaço reserva
```

E mostra na saída o _padrão_, com duas linhas: "laranja" e "abacaxi".

Ufa! Depois dessa não venha me dizer que não sabe como funcionam os _registradores internos_ do _Sed_ ;)

### 8.4.4. Resumão

- Cada linha nova lida é colocada (sobrescrevendo) no _espaço padrão_
- Uma vez colocado algo no _espaço reserva_, fica lá até ser sobrescrito
- O **`\n`** é o separador do conteúdo original com o anexo
- Na saída, o **`\n`** vira quebra de linha
- Registradores são simples! ;)

### 8.4.5. Fluxograma

Para uma representação gráfica dos fluxos e comandos que manipulam estes registradores, veja o tópico [Fluxos dos registradores internos](https://aurelio.net/sed/sed-howto/#fluxo-registradores).
