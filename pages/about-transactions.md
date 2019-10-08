# Sobre transações

Trabalhei algum tempo em projetos usando bancos de dados relacionais, lembro que
era muito conveniente poder usar transações nesses sistemas. Bastava um,
`BEGIN TRANSACTION` e eu não precisava me preocupar mais com a consistencia dos
dados, tudo funcionava como esperado. Ou deveria...

Lembro de ter alguns casos que eu não sabia o por quê de alguns dados estarem
inconsistentes, por isso comecei a pesquisar sobre o tema, até que cheguei na
seguinte palestra:

<iframe width="560" height="315" src="https://www.youtube.com/embed/tRc0O9VgzB0"
frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope;
picture-in-picture" allowfullscreen></iframe>

Nesta, [@aphyr](github.com/aphyr) apresenta alguns sistemas e seus modos de
falhas, comentando casos estranhos sobre esses sistemas e como a documentação
dos mesmos não condiz necessariamente com a realidade. Isso foi um alerta,
provavelmente não tinha me aprofundado sobre o funcionamento do banco e acabei
assumindo um comportamento ideal. Procurei outras referências sobre o assunto
e me deparei com a seguinte palestra:

<iframe width="560" height="315" src="https://www.youtube.com/embed/5ZjhNTM8XU8"
frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope;
picture-in-picture" allowfullscreen></iframe>

Essa palestra é genial! [@martinkl](https://twitter.com/martinkl) explica de
maneira didática como transações funcionam nos bancos de dados, começando pelas
propriedades das transações, ou mais conhecido como
[ACID](https://en.wikipedia.org/wiki/ACID).

<div style="width:100%;height:0;padding-bottom:67%;position:relative;"><iframe
src="https://giphy.com/embed/xT0xeJpnrWC4XWblEk" width="100%" height="100%"
style="position:absolute" frameBorder="0" class="giphy-embed"
allowFullScreen></iframe></div><p><a
href="https://giphy.com/gifs/whoa-hd-tim-and-eric-xT0xeJpnrWC4XWblEk">via
GIPHY</a></p>

Ele começa explicando como o conceito de
[Durability](https://en.wikipedia.org/wiki/Durability_(database_systems)) mudou
com o passar do tempo, sendo primeiramente interpretado como salvar os dados em
fita, depois salvar os dados em disco, para, atualmente, ter os dados replicados
em mais de um local. 

Seguindo com o conceito de
[Consistency](https://en.wikipedia.org/wiki/Consistency_(database_systems)), que
por muitas vezes é confundido com Atomic Consistency do [teorema
CAP](https://en.wikipedia.org/wiki/CAP_theorem), e está mais relacionado com
como as aplicações usam os bancos de dados do que com o banco propriamente dito.

O próximo tópico que ele aborda brevemente é o da
[Atomicity](https://en.wikipedia.org/wiki/Atomicity_(database_systems)), o qual
é frequentemente confundido com 
[Isolation](https://en.wikipedia.org/wiki/Isolation_(database_systems)).
Ele deixa claro que Atomicity está relacionado ao modo de falha da transação,
sendo que, caso ela falhe, o estado do banco não deve mudar.

Sobre Atomicity, é interessante entender que essa característica da transação já
cobre uma série de problemas como *crash, falha de energia, falha de rede,
deadlock e falha condicional*, todas essa família de problemas já está tratada
por essa característica da transação.

Depois de passar por quase todo o acrônimo, ele se aprofunda no tema de isolação
das transações, que está muito relacionado a concorrência e como isso é tratado
nos bancos. Você pode ter os seguintes níveis de isolamento:

1. **Serializable**: Melhor nível de isolamento
1. **Snapshot Isolation**
1. **Read Committed**: [Default no
   PostgresSQL](https://www.postgresql.org/docs/12/sql-set-transaction.html)
1. **Read Uncommitted**: Pior nível de isolamento, a.k.a `¯\_(ツ)_/¯`

Ele também ilustra cada um problemas de concorrência que podem afetar cada nível de isolação:

|**Problema** | **Read Uncommitted** | **Read Committed** | **Snapshot Isolation** | **Serializable** |
|=============|======================|====================|========================|==================|
|Dirty reads  | [x]                  |[ ]                 |[ ]                     |[ ]               |
|Dirty writes | [x]                  |[ ]                 |[ ]                     |[ ]               |
|Read skew    | [x]                  |[x]                 |[ ]                     |[ ]               |
|Write skew   | [x]                  |[x]                 |[x]                     |[ ]               |

Visto tudo isso, eu entendi que o buraco que eu estava me metendo era bem maior
que imaginava, e que era uma boa alterar o a configuração padrão do PostgresSQL,
para evitar os aqueles problemas.

Esse [rabbit hole](https://en.wikipedia.org/wiki/Wiki_rabbit_hole) de
transações, sistemas distribuídos e concorrência é muito divertido seguir, mas
fica confuso muito rápido. Ainda mais quando não se tem contato direto com o
tema.

## Material extra

- [An introduction to distributed
  systems](https://github.com/aphyr/distsys-class) escrito pelo @aphyr
