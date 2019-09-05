# 1. Sobre este documento

## 1.1. Descrição

Este documento se propõe a ser um tutorial e um guia de consulta de _Sed_ ao mesmo tempo.

- **Tutorial** porque ele vai lhe apresentando o _Sed_ aos poucos, explicando seu funcionamento.
- **Guia de consulta** porque ele tem dicas avançadas e descreve truques específicos que só serão assimilados e compreendidos executando-os na prática.

Este documento **NÃO** abordará as [Expressões Regulares](https://aurelio.net/regex/), que são um tema complexo, e embora façam parte da essência do _Sed_, seu funcionamento independe delas.

Resumindo, o _Sed HOWTO_ fala sobre _Sed_.

Este documento pode (deve) ser distribuído à vontade.


## 1.2. Anúncio

Este documento é algo que eu estava me devendo há séculos: uma documentação decente em português sobre o _Sed_ e seus detalhes.

É o _Sed HOWTO_, um misto de tutorial e guia de referência, com exemplos práticos. A idéia é que sirva tanto aos principiantes quanto aos iniciados, abrangendo conceitos básicos e complexos.

**[https://aurelio.net/sed/sed-HOWTO/](https://aurelio.net/sed/sed-HOWTO/)**

Convido todos a visitarem e dar uma lida.

Além de uma explicação bem detalhada, "gráfica" e didática dos registradores internos e seus comandos

Com certeza, ainda tem **MUITA** coisa a melhorar/acrescentar. Qualquer sugestão é bem-vinda.


## 1.3. Onde encontrá-lo

A casa oficial deste documento é na seção _Sed_ do Site do Aurelio. Você pode consultá-lo _on-line_ ou baixá-lo para leitura local:

- [https://aurelio.net/sed/sed-HOWTO/](https://aurelio.net/sed/sed-HOWTO/)


## 1.4. Registro de mudanças

2017

- Removidas as versões TXT e HTML multipáginas deste documento, para facilitar sua manutenção
- Melhorados os nomes das âncoras HTML, de toc1, toc2, ... pra nome-do-topico

2009-06-26 — v0.6

- Documento convertido para UTF-8
- A versão HTML de uma única página agora vem sem formatação (CSS) para facilitar a impressão
- Removidas as versões PDF e SGML deste documento, para facilitar sua manutenção
- Todos os links externos foram verificados e atualizados
- Agora as referências internas para outros tópicos são links
- Padronizado o nome "Sed" em vez de "sed" e "SED" ao referir-se ao programa
- Adicionado link para o manual do GNU Sed, em _Onde obter mais informações_.
- Adicionado link e melhorada a tabela em _Emulando outros comandos_
- Melhorias nos desenhos ASCII (curvas com + e alinhamento)
- Melhorias de formatação na lista de agradecimentos

2003-04-15 — v0.5

- Listagem dos comandos adicionada em Descrição de todos os comandos
- Mudanças cosméticas, URLs atualizadas, s/endereçamento/endereço/g
- Adicionada versão HTML (tudo em uma página) e retirada a versão em PostScript (basta fazer `sgml2latex -o ps sed-HOWTO.sgml`)
- Retirado também o `.tgz` do ar
- O [txt2tags](http://txt2tags.org/) agora é o conversor utilizado para gerar o Sed HOWTO - acabaram os títulos em CAPSLOCK

2001-02-02 — v0.4

- Documento disponibilizado agora também em ps e pdf
- Mais info na seção _Anúncio_, sobre os formatos novos

2000-12-03 — v0.3

- Criada seção _Trocando um trecho de texto por outro_
- Criada seção _Fluxos da execução dos comandos_
- Criada seção _Onde encontrá-lo_
- Exemplo gráfico didático em _Conhecendo os registradores internos_
- Informações mais didáticas na seção _Como ele funciona_
- Documento disponibilizado em txt e sgml
- Várias correções pequenas nos textos

2000-09-13 — v0.2

- Criada seção _Anúncio_
- Criada seção _Registro de mudanças_
- Criada seção _Agradecimentos_
- Disponibilizado este documento em HTML compactado
- Retirada entrada duplicada na seção _Emulando outros comandos_

2000-08-2 — v0.1

- 1ª versão
- Disponibilização na internet

## 1.5. Agradecimentos

Meus agradecimentos sinceros àqueles que comentaram, enviaram sugestões e correções, ou ajudaram na divulgação, via e-mail ou internet.

- .*@conectiva
- .*@lista_sed-br
- .*@lista_sed-users
- Carlos Alvsan
- Eduardo Mendes
- Rafael Steil
- Rodrigo Bernardo Pimentel
- Rubens Queiroz de Almeida & Dicas-l
- Sérgio Bruder & .BR
- Thobias Salazar Trevisan
- Tiago Barros & senha.org
