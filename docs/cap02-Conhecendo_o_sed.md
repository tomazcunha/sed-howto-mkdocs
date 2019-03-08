
# 2. Conhecendo o sed

Vamos conhecer um pouco o _Sed_, mostrar que ele não é o bicho de _Sed_ cabeças que aparenta :)

## 2.1. Descrição do Sed

O _Sed_ é um editor de textos **não interativo**.

Ele pode editar automaticamente, sem interação do usuário, vários arquivos seguindo um conjunto de regras especificadas.

## 2.2. O que significa a palavra Sed

Vem do inglês "Stream EDitor", ou seja, editor de fluxos (de texto).

## 2.3. Como saber se devo usar o Sed

Sendo um editor de textos não interativo, o _Sed_ é excelente para desempenhar algumas tarefas, mas em outras seu uso não é aconselhado.

### 2.3.1. Quando usar o Sed

A característica principal do _Sed_ é poder editar arquivos automaticamente.

Então sempre que você precisar fazer alterações sistemáticas em **vários** arquivos, o _Sed_ é uma solução eficaz.

Por exemplo, você tem um diretório cheio de relatórios de vendas, e descobriu que por um erro na geração, todas as datas saíram erradas, com o ano de **1999** onde era para ser **2000**. Num editor de textos normal, você tem que abrir os relatórios um por um e alterar o ano em todas as ocorrências.

Certo, isso não é tão complexo se o editor de textos possuir uma ferramenta de procura e troca, também chamado de substituição.

Mas então suponhamos que o erro da data não seja o ano, e sim o **formato**, tendo saído como **mm/dd/aaaa** quando deveria ser **dd/mm/aaaa**. Aqui não é uma substituição e sim uma troca de lugares, e uma ferramenta simples de procura e troca não poderá ajudar.

Esse é um caso típico onde o _Sed_ mostra seu poder: alterações complexas em vários arquivos.

Utilizando o _Sed_, a solução para este problema (que veremos adiante) é até simples, bastando definir uma série de regras de procura e troca, e o programa se encarregará de executá-las e arrumar os relatórios.


### 2.3.2. Quando não usar o Sed

**Nenhuma** ferramenta é ideal para todas as tarefas, e o Sed não é uma exceção à regra.

**Edição genérica de textos**

Ele não é prático para ser utilizado como editor de textos de uso genérico.

Para escrever textos, ou alterar coisas simples, é mais rápido e fácil abrir um editor de textos interativo como o _vi_ ou o _emacs_ e fazer a alteração "na mão".

**Programação avançada**

O _Sed_ não é uma linguagem de programação completa, pois não possui variáveis, funções matemáticas, interação com o sistema operacional, entre outras limitações. Mas bem, ele é um **manipulador de texto** e não uma linguagem de uso geral.

Algumas estruturas complexas podem ser simuladas com alguma técnica, mas se o seu programa em _Sed_ começou a inchar muito, é aconselhável reescrevê-lo numa linguagem com mais recursos, como o _perl_.
