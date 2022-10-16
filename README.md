# Caching em Redes com Perdas e Atrasos

![GitHub repo size](https://img.shields.io/github/repo-size/siliconemonster/AD-TrabalhoFinal?color=a21360&style=for-the-badge)
![GitHub language count](https://img.shields.io/github/languages/count/siliconemonster/AD-TrabalhoFinal?color=a21360&style=for-the-badge)
![Made With](https://img.shields.io/badge/Made%20With-Python-lightgrey?color=a21360&style=for-the-badge)
![GitHub repo file count](https://img.shields.io/github/directory-file-count/siliconemonster/AD-TrabalhoFinal?color=a21360&style=for-the-badge)
![GitHub last commit](https://img.shields.io/github/last-commit/siliconemonster/AD-TrabalhoFinal?color=a21360&style=for-the-badge)

# Índice

- [Introdução](#introdução)
- [Cenário I - Sem perdas e sem atrasos](#cenário-i)
  - [Simulador Cenário I](#simulador-cenário-i)
- [Cenário II – Com perdas e sem atrasos](#cenário-ii)
  - [Simulador Cenário II](#simulador-cenário-ii)
- [Cenário III – Com perdas e com atrasos](#cenário-iii)
  - [Simulador Cenário III](#simulador-cenário-iii)
- Aplicação dos Simuladores e Conclusão(#aplicação-dos-simuladores-e-conclusão)
- [Colaboradores](#colaboradores)
- [Licença](#licença)

# Introdução

O objetivo deste projeto é construir um simulador de um sistema de 2 caches em redes (FIFO, LRU, Random e Estáticas) com perdas e atrasos, de acordo com 4 cenários possíveis. São eles: canais sem perdas e sem atrasos, canais com perdas e sem atrasos, e dois canais com perdas e com atrasos.
Também devemos efetuar os cálculos utilizando nossos conhecimentos sobre cadeias de Markov, e compararmos com o intervalo de confiança obtido em cada simulação quando possível. Calcularemos as probabilidades de sucesso, user miss, tempo médio de serviço de requisições, conforme o que for solicitado em cada cenário. 

Optamos pela linguagem Python, pois fornece uma gama de funções pré-existentes para o nosso auxílio, entre elas funções para plotagem de gráficos.

# Cenário I

O cenário I (sem perdas e sem atrasos) é composto por duas caches, cada uma com tamanho igual à dois, não há atrasos
nos canais de comunicação e é considerado o caso especial onde não há chances de um canal falhar.
Dessa forma, a cada requisição feita pelo cliente, apenas é avaliado se as caches possuem ou não o
conteúdo requisitado e uma mudança é realizada nos conteúdos da cache de acordo com seu modelo.
Nosso propósito nesse cenário é identificar qual a probabilidade de uma requisição ser atendida com
sucesso, ou seja, pelo menos uma das duas caches possuir o conteúdo requisitados para cada uma das
seguintes composições:

- 2 caches FIFO; 
- 2 caches LRU; 
- 2 caches Random; 
- 2 caches Estáticas

## Simulador Cenário I

Para simularmos o cenário I, criamos um simulador de eventos de tempo contínuo onde
requisições de cada conteúdo chegam numa taxa igual a probabilidade do conteúdo ser requisitado dada
e verificamos quantas delas foram atendidas por pelo menos uma das duas caches. Abaixo, temos uma
melhor descrição do funcionamento do simulador e da função que o implementa.
O simulador possui um sistema com duas caches onde podemos definir o tamanho delas, o
número de conteúdos possíveis de serem requisitados em cada experimento, a probabilidade de
requisição de cada conteúdo e os estados iniciais das caches. Inicialmente, as caches são inicializadas
com os estados passados, caso não sejam passados, elas são inicializadas de forma aleatória com os
conteúdos possíveis, que nesse caso serão representados por números inteiros de 1 ao “numConteudos”
dado.

Após isso, agenda-se um evento de uma requisição para cada conteúdo na lista de eventos, esse
evento indica qual o tempo em que a requisição irá acontecer e qual conteúdo estará sendo requisitado.
O tempo do agendamento é gerado de forma aleatória seguindo a distribuição exponencial tendo como
parâmetro a taxa de chegada do conteúdo, que aqui adotamos como 1 * probabilidade do conteúdo ser
requisitado. A cada vez que um evento é adicionado, a lista é ordenada em função dos tempos
apresentados em cada evento.

Com as caches inicializadas e uma requisição agendada para cada conteúdo, começa-se a
realização dos eventos, ou seja, as requisições agendadas começam a ser a realizadas. Em termos de
código, inicia-se um loop em cima da lista de eventos, onde em cada iteração, a requisição com menor
tempo presente na lista é encaminhada paras as duas caches e para cada cache é realizado o seguinte
processo: analisa-se se a cache possui o conteúdo requisitado e assim marca-se se a requisição foi
atendida ou não, em ambos os casos a cache muda de estado seguindo o padrão do tipo de cache
especificado. Após ser encaminhada para as duas caches, verifica-se a requisição foi atendida por pelo
menos uma das caches, se sim, é marcado como sucesso. Após uma requisição ser feita, é agendada
uma nova requisição para o mesmo conteúdo onde o tempo do agendamento é gerado de forma aleatória
seguindo a distribuição exponencial com parâmetro igual a probabilidade do conteúdo, dessa forma
garantimos que cada conteúdo seja requisitado seguindo sua taxa de chegada.

Após o número de requisições pedidas serem feitas, verifica-se o número de sucessos e
finalmente calcula-se a probabilidade de sucesso fazendo o seguinte cálculo:
𝑛ú𝑚𝑒𝑟𝑜 𝑑𝑒 𝑠𝑢𝑐𝑒𝑠𝑠𝑜𝑠/𝑛ú𝑚𝑒𝑟𝑜 𝑑𝑒 𝑟𝑒𝑞𝑢𝑖𝑠𝑖çõ𝑒𝑠 𝑟𝑒𝑎𝑙𝑖𝑧𝑎𝑑𝑎𝑠

O simulador do cenário I é dado pela função “simulacaoCenario1()” que tem os seguintes
parâmetros de entrada:
* numRequisicoes - Número de requisições que serão realizadas no experimento. Deve ser um
número inteiro maior que 0;
* caso - qual é o caso/tipo das caches. Os possíveis valores de entrada são: "FIFO", "LRU",
"Random" ou "Estatica";
* numConteudos - número de conteúdos que podem ser requisitados. Deve ser um número
inteiro maior que 0;
* tamCache - tamanho das caches. Deve ser um número inteiro maior que 0;
* probabilidades - probabilidade de cada conteúdo ser requisitado. Deve ser passado uma lista
de tamanho igual ao número de conteúdos. Por exemplo, se são três conteúdos e possuem probabilidade
de requisição uniforme, a lista passada deverá ser [1/3,1/3,1/3];
* depurar - Booleano para indicar se deseja imprimir a depuração do código ou não. O default
é o valor "False", onde não se depura;
* cache1_inicial - Estado inicial de uma cache. Se nada for passado, ela irá ser inicializada
aleatoriamente;
* cache2_inicial - Estado inicial da outra cache. Se nada for passado, ela irá ser inicializada
aleatoriamente.
* cachesDiferentes - Booleano para indicar se deseja que as caches comecem em estados
diferentes ou iguais. O default é o valor "True", onde começam com em estados diferentes.


[⬆ Voltar ao topo](#caching-em-redes-com-perdas-e-atrasos)

# Cenário II

O cenário II (com perdas e sem atrasos) também é composto por duas caches, cada uma com tamanho igual à dois, não há
atrasos nos canais de comunicação, contudo há a probabilidade de um canal falhar. Dessa forma, a cada
requisição feita pelo cliente, primeiramente é avaliado se o canal funcionou e só assim é avaliado se a
cache possui ou não o conteúdo requisitado. Nosso propósito nesse cenário é identificar qual a
probabilidade de ocorrer um user miss que em outras palavras é a probabilidade de uma requisição não
ser atendida, para cada uma das seguintes composições:
- 2 caches FIFO; 
- 2 caches LRU; 
- 2 caches Random; 
- 2 caches Estáticas

## Simulador Cenário II

Para simularmos o cenário II, criamos um simulador de eventos de tempo contínuo muito
semelhante ou simulador do cenário I onde requisições de cada conteúdo chegam a uma taxa igual a
probabilidade do conteúdo ser requisitado dada e verificamos quantas delas foram atendidas por pelo
menos uma das duas caches. A única diferença nesse caso é que calcula-se se o canal falhou ou não,
com probabilidade igual a p = 0.9 de funcionar, antes encaminhar para a cache, Abaixo, temos uma
melhor descrição do funcionamento do simulador e da função que o implementa.

Como o cenário II se assemelha muito ao cenário I, a descrição do simulador é bastante
semelhante. Temos então que inicialmente, as caches são inicializadas com os estados passados, caso
não sejam passados, elas são inicializadas de forma aleatória com os conteúdos possíveis, que nesse
caso serão representados por números inteiros de 1 ao numConteudos dado.

Após isso, agenda-se um evento de uma requisição para cada conteúdo na lista de eventos, esse
evento indica qual o tempo em que a requisição irá acontecer e qual conteúdo estará sendo requisitado.
O tempo do agendamento é gerado de forma aleatória seguindo a distribuição exponencial tendo como
parâmetro a taxa do conteúdo e a cada vez que um evento é adicionado, a lista é ordenada em função
dos tempos apresentados em cada evento.

Com as caches inicializadas e uma requisição agendada para cada conteúdo, começa-se a
realização dos eventos, ou seja, as requisições agendadas começam a ser realizadas. Em termos de
código, inicia-se um loop em cima da lista de eventos, onde em cada iteração, a requisição com menor
tempo presente na lista é encaminhada para as duas caches. Neste ponto que o cenário II se diferencia
do cenário I, neste caso há a probabilidade do canal que leva a requisição até a cache falhar, e com isso
não tem a possibilidade da requisição ser atendida pela cache que por conta disso não mudará de estado.

Então para cada cache é realizado o seguinte processo: tenta-se encaminhar a requisição para
cache, para isso gera-se um número entre 0 e 1, se o número for menor ou igual ao parâmetro $p$ (que
por default é $0.9$, o que implica que o canal tem 90% de chance de funcionar), o canal funciona e a
requisição alcança a cache e assim analisa-se se a cache possui o conteúdo requisitado e marca-se se a
requisição foi atendida ou não, em ambos os casos em que o canal funciona, a cache muda de estado
seguindo o padrão do tipo de cache especificado. Após a tentativa de se encaminhar a requisição para
as duas caches, verifica-se se a requisição foi atendida por pelo menos uma das caches, se sim, é marcado
como sucesso. Após uma requisição ser feita, é agendada uma nova requisição para o mesmo conteúdo
onde o tempo do agendamento é gerado de forma aleatória seguindo a distribuição exponencial com
parâmetro igual a probabilidade do conteúdo, dessa forma garantimos que cada conteúdo seja
requisitado seguindo sua taxa de chegada.

Após o número de requisições pedidas serem feitas, verifica-se o número de casos em que não
ocorreu user miss, ou seja, os eventos onde a requisição foi atendida e finalmente calcula-se a
probabilidade de user miss fazendo o seguinte cálculo: 

(𝑛ú𝑚𝑒𝑟𝑜 𝑑𝑒 𝑟𝑒𝑞𝑢𝑖𝑠𝑖çõ𝑒𝑠 𝑟𝑒𝑎𝑙𝑖𝑧𝑎𝑑𝑎𝑠 − 𝑛ú𝑚𝑒𝑟𝑜 𝑑𝑒 𝑐𝑎𝑠𝑜𝑠 𝑑𝑒 𝑛ã𝑜 𝑢𝑠𝑒𝑟)/ 𝑛ú𝑚𝑒𝑟𝑜 𝑑𝑒 𝑟𝑒𝑞𝑢𝑖𝑠𝑖çõ𝑒𝑠 𝑟𝑒𝑎𝑙𝑖𝑧𝑎𝑑𝑎𝑠

O simulador do cenário II é dado pela função “simulacaoCenario2()” que tem os seguintes
parâmetros de entrada:

* numRequisicoes - Número de requisições que serão realizadas no experimento. Deve ser um
número inteiro maior que 0.
* caso - qual é o caso/tipo das caches. Os possíveis valores de entrada são: "FIFO", "LRU",
"Random" ou "Estatica"
* numConteudos - número de conteúdos que podem ser requisitados. Deve ser um número
inteiro maior que 0.
* tamCache - tamanho das caches. Deve ser um número inteiro maior que 0.
* probabilidades - probabilidade de cada conteúdo ser requisitado. Deve ser passado uma lista
de tamanho igual ao número de conteúdos. Por exemplo, se são três conteúdos e possuem probabilidade
de requisição uniforme, a lista passada deverá ser [1/3,1/3,1/3]
* p - probabilidade de um canal falhar. Deve ser um float entre 0 e 1 e tem como default o valor
0.9.
* depurar - Booleano para indicar se deseja imprimir a depuração do código ou não. O default
é o valor "False", onde não se depura.
* cache1_inicial - Estado inicial de uma cache. Se nada for passado, ela irá ser inicializada
aleatoriamente.
* cache2_inicial - Estado inicial da outra cache. Se nada for passado, ela irá ser inicializada
aleatoriamente.
* cachesDiferentes - Booleano para indicar se deseja que as caches comecem em estados
diferentes ou iguais. O default é o valor "True", onde começam com em estados diferentes.


[⬆ Voltar ao topo](#caching-em-redes-com-perdas-e-atrasos)

# Cenário III

O cenário III (com perdas e com atrasos) também é composto por duas caches, cada uma com tamanho igual à dois,
contudo apresenta atrasos nos canais de comunicação e também há a probabilidade de um canal falhar.
Dessa forma, a cada requisição feita pelo cliente, primeiramente é avaliado se o canal funcionou e só
assim é avaliado se a cache possui ou não o conteúdo requisitado e tem o tempo para a requisição chegar
nas caches e nos servidores. Nosso propósito nesse cenário é identificar qual o tempo médio para uma
requisição ser atendida nas seguintes combinações:
- 2 caches FIFO; 
- 2 caches LRU; 
- 2 caches Random; 
- 2 caches Estáticas

## Simulador Cenário III

Para simularmos o cenário III, criamos um simulador de eventos de tempo contínuo muito
mais complexo que os cenários anteriores, onde temos vários eventos para simular todo o processo que
uma requisição passa até ser atendida, seja por uma das duas caches ou pelo servidor alternativo. Abaixo,
temos uma melhor descrição do funcionamento do simulador e da função que o implementa.

O cenário III apresenta um funcionamento mais próximo da realidade. Nossa modelagem do
problema funciona como descrito abaixo:

Temos que inicialmente, as caches são inicializadas com os estados passados, caso não sejam
passados, elas são inicializadas de forma aleatória com os conteúdos possíveis, que nesse caso serão
representados por números inteiros de 1 ao numConteudos dado.

Após isso, agenda-se um evento de uma requisição para cada conteúdo na lista de eventos, esse
evento indica qual o tempo em que a requisição irá acontecer e qual conteúdo estará sendo requisitado.
O tempo do agendamento é gerado de forma aleatória seguindo a distribuição exponencial tendo como
parâmetro a taxa do conteúdo e a cada vez que um evento é adicionado, a lista é ordenada em função
dos tempos apresentados em cada evento.

Com as caches inicializadas e uma requisição agendada para cada conteúdo, inicia-se o
processo. Em termos de código, inicia-se um loop em cima da lista de eventos, onde para cada
requisição, novos eventos são gerados para simular seu atendimento. Primeiramente, tenta-se
encaminhar a requisição com menor tempo presente na lista inicial de eventos para as duas caches. Aqui
temos um novo evento e mais dois possíveis novos eventos para acrescentar na lista, pois tenta-se encaminhar a requisição para cada cache, para isso gera-se um número entre 0 e 1, se o número for
menor ou igual ao parâmetro p (que por default é 0.9, o que implica que o canal tem 90% de chance de
funcionar), o canal funciona e cria-se um novo evento da requisição chegando na cache, repete-se isso
para as duas. Assim, há a possibilidade da criação de um evento onde a requisição chega na cache 1
e/ou outro evento onde a requisição chega na cache 2. Além disso, é gerado um número aleatório
seguindo a distribuição exponencial com parâmetro 1/𝛼 a para indicar o timer da requisição, assim, criase um novo evento que será o timeout da requisição.

Quando o evento da requisição chegando na cache acontecer, é verificado se a cache possui o
conteúdo requisitado. Se sim, cria-se um novo evento da requisição sendo atendida pela cache agendado
para acontecer em n segundos, onde n é calculado gerando um número aleatório seguindo a distribuição
exponencial com parâmetro 1/𝜇. Já se a cache não possuir, primeiro cria-se um novo evento onde cache
receberá o conteúdo requisitado do servidor para assim poder atender a requisição, o evento é agendado
para acontecer em m segundos, onde m é calculado gerando um número aleatório seguindo a
distribuição exponencial com parâmetro 1/𝜃 (𝜃 = ∞ no caso desse cenário) e só quando esse novo evento
criado acontecer, que cria-se o evento da requisição sendo atendida.

Já se o evento timeout da requisição chegar a acontecer, da forma que modelamos, ele cancela
todos os outros eventos referentes a requisição que sofreu o timeout, isso para simular o cancelamento
do processo de tentativa de atender a requisição pela caches, e cria um evento da requisição sendo
atendida pelo servidor alternativo, o evento é agendado para acontecer em l segundos, onde l é calculado
gerando um número aleatório seguindo a distribuição exponencial com parâmetro 1/𝛾, onde nesse cenário.

Por último, quando o evento requisição atendida acontece, seja pela cache 1, cache 2 ou pelo
servidor alternativo por conta do timeout, verifica-se se há mais requisições abertas para o mesmo
conteúdo da que está sendo atendida, e cancela todos os processos referentes a essas requisições junto
com os processos referentes à que está sendo atendida, com exceção dos eventos intitulados “Sistema”,
o que representa quando uma requisição será feita por um usuário e garante a taxa de chegada de cada
tipo de conteúdo. Após isso, anota-se qual foi o tempo de atendimento de cada requisição fazendo
tempo do evento requisição atendida - tempo de chegada da requisição.

Todos esses eventos acontecem para cada requisição e se misturam, ou seja, pode ter um evento
da requisição "req2" chegando na cache 1 enquanto e logo em seguida outra requisição "req1" sendo
atendida pelo servidor alternativo.

Após termos o número especificado pelo parâmetro de requisições atendidas, seja pelo jeito que
for, as iterações param e calcula-se o tempo de atendimento médio das requisições fazendo-se a soma
do tempo de atendimento de cada requisição/número de requisições.

Lembrando que sempre que uma nova requisição chega, outra requisição para o mesmo
conteúdo é agendada onde o tempo do agendamento é gerado de forma aleatória seguindo a distribuição
exponencial com parâmetro igual a probabilidade do conteúdo, dessa forma garantimos que cada
conteúdo seja requisitado seguindo sua taxa de chegada. A depuração apresentada mais abaixo deve
ajudar a entender o funcionamento da simulação aqui descrito que imaginamos que possa ser confusa
já que muitos eventos são gerados. Após o número de requisições pedidas serem feitas, verifica-se o
número de casos em que não ocorreu user miss, ou seja, os eventos onde a requisição foi atendida e
finalmente calcula-se a probabilidade de user miss fazendo o seguinte cálculo: 

(𝑛ú𝑚𝑒𝑟𝑜 𝑑𝑒 𝑟𝑒𝑞𝑢𝑖𝑠𝑖çõ𝑒𝑠 𝑟𝑒𝑎𝑙𝑖𝑧𝑎𝑑𝑎𝑠 − 𝑛ú𝑚𝑒𝑟𝑜 𝑑𝑒 𝑐𝑎𝑠𝑜𝑠 𝑑𝑒 𝑛ã𝑜 𝑢𝑠𝑒𝑟)/𝑛ú𝑚𝑒𝑟𝑜 𝑑𝑒 𝑟𝑒𝑞𝑢𝑖𝑠𝑖çõ𝑒𝑠 𝑟𝑒𝑎𝑙𝑖𝑧𝑎𝑑𝑎𝑠

O simulador do cenário III é dado pela função abaixo que tem os seguintes parâmetros de
entrada:
* numRequisicoes - Número de requisões que serão realizadas no experimento. Deve ser um
número inteiro maior que 0.
* caso - qual é o caso/tipo das caches. Os possíveis valores de entrada são: "FIFO", "LRU",
"Random" ou "Estatica"
* numConteudos - número de conteúdos que podem ser requisitados. Deve ser um número
inteiro maior que 0.
* tamCache - tamanho das caches. Deve ser um número inteiro maior que 0.


[⬆ Voltar ao topo](#caching-em-redes-com-perdas-e-atrasos)

# Aplicação dos Simuladores e Conclusão

Por serem muito extensos, os resultados das simulações aplicadas e nossa conclusão se encontram no documento [Relatório Trabalho Final - AD.pdf](https://github.com/siliconemonster/AD-TrabalhoFinal/blob/main/Relat%C3%B3rio%20Trabalho%20Final%20-%20AD.pdf)

# Colaboradores

<table>
  <tr>
    <td align="center">
      <a href="#">
        <img src="https://avatars.githubusercontent.com/u/20328857?v=4" width="100px;" alt="Foto de Aline Freie no GitHub"/><br>
        <sub>
          <a href="https://github.com/siliconemonster"><b>Aline Freire</b></a>
        </sub>
      </a>
    </td>
    <td align="center">
      <a href="#">
        <img src="https://avatars.githubusercontent.com/u/34246743?v=4" width="100px;" alt="Foto de Letícia Tavares no GitHub"/><br>
        <sub>
          <a href="https://github.com/leticiatavaresds"><b>Letícia Tavares</b></a>
        </sub>
      </a>
    </td>
</table>


[⬆ Voltar ao topo](#caching-em-redes-com-perdas-e-atrasos)

# Licença

The MIT License (MIT) 2022 - Aline Freire e Letícia Tavares. Leia o arquivo [LICENSE.md](https://github.com/siliconemonster/AD-TrabalhoFinal/blob/master/LICENSE.md) para mais detalhes.


[⬆ Voltar ao topo](#caching-em-redes-com-perdas-e-atrasos)
