# 9. Fluxogramas

## 9.1. Fluxos do texto

                                      _________
                                w    |         |   r
                            +------->| ARQUIVO |------->
                            |        |_________|
                      ______|______                       SAÍDA
                n    |             |           p P l
    ENTRADA  ------->|    S E D    |------------------->
                N    |_____________|           a i c
                            |
                            |   d
                            +-------> /dev/null
                                D

## 9.2. Fluxos da execução dos comandos

               _________
              |         |
        ______| próxima |<---+
       /      |  linha  |    |
       |      |_________|    |
       |                     |
       v                 d D | n N
                        _____|______
                       |            |
    COMEÇO   --------->|  COMANDOS  |----------> FIM DO PROGRAMA
                       |____________|  q
                         |       ^
                       b | t     |
                         |       |
                         |    ___|______
                         |   |          |
                         +-->| marcação |
                             |__________|


## 9.3. Fluxos dos registradores internos

     _________________               __________________
    |                 |---- h H --->|                  |
    |  ESPAÇO PADRÃO  |<---  x  --->|  ESPAÇO RESERVA  |
    |_________________|<--- g G ----|__________________|



Veja explicação sobre estes registradores no tópico [Conhecendo os registradores internos](https://aurelio.net/sed/sed-howto/#registradores-internos).
