# Sobre IO assíncrono

Cai numa discução sobre IO assíncrono e qual a melhor metáfora para descrever
essa forma de operar para alguém que não é da área. Chegamos na seguinte
metáfora:

IO assíncrono é como o drive thru de lanchonete, o cliente faz um pedido
(requisição) e entra numa fila de pedidos (de requisições) que serão atendidos
pela lanchonete (servidor), enquato ela prepara o lanche (processa a
requisição) outro cliente faz outro pedido.

A lanchonete pode atender mais pedidos (escalar) das seguintes maneiras,
criando novas lanchonetes (escalar horizontamente com mais processos, ex Rails +
unicorn), criando mais cabines de drive thru (escalar horizontamente com mais
threads, ex Rails + puma), colocando mais gente pra trabalhar no restaurante
(aumentar o thread pool) ou atendendo os pedidos mais rapidamente (escalar
verticalmente).
