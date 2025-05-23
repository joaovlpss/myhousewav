# Análise de dados em licitações públicas

Esse projeto é na verdade o início de uma iniciação científica que daria suporte à um
projeto de mestrado de um aluno do ICMC. A idéia é obter um dataset público da CGU e,
através de análise de grafos, chegar em comportamento considerado suspeito.

Acabei sendo movido para outro projeto pela minha orientadora, então não espero atualizar
esse no futuro próximo. Do jeito que se encontra, o projeto oferece uma ótima base com:

1. Design de arquitetura em camadas Raw, Bronze, Silver e Gold;
2. Processamento em etapas com validação de data types e uso de formato parquet;
3. Geração de grafos, plotagens de features de grafos 2 a 2 e algumas análises iniciais via notebook.

## Pré-processamento

Os dados vêm todos com headers em PT-BR e separação com ';'.

O primeiro trabalho aqui foi renomear todos os headers para inglês e remover espaços.
Considero isso boa prática que evita erros posteriores. Criei um arquivo de configuração com
um dicionário de renomeação para todos os headers das tabelas, e que também permite configurar
quais períodos serão processados quando o usuário rodar o workflow (assim como N outros parâmetros).
Depois disso, validar os tipos de dados das colunas e fazer as conversões para numéricos já nos dá o
suficiente para salvar na nossa camada bronze.

Da camada bronze, nós precisamos concatenar os períodos especificados nas configurações e fazer os joins.
Foi um pouco trabalhoso achar uma combinação de chaves que criasse unicidade entre as diferentes tabelas,
já que a controladoria não se deu ao trabalho de criar um ID por transação. Feito isso, pegamos as features
de interesse e salvamos na camada silver.

## Geração dos grafos

Com o pré-processamento realizado, utilizamos o pacote t-graph (que na época eu ainda não tinha publicado no PyPI)
para gerar grafos temporais e não-temporais. Salvamos as features na camada gold e já podemos utilizar o Matplotlib + Seaborn
para gerar hexplots de features dois a dois.

## Análises iniciais

Como dito antes, eu não tive muito tempo para realmente analisar essas features geradas. Os notebooks de análises iniciais
mostram alguns grupos de 'compradores' (órgaos públicos) e 'vendedores' (licitantes) que estão nas bordas dos gráficos gerados.

Alguns comportamentos inicialmente interessantes são que muitas licitações apresentam valor igual a zero. Não consegui identificar
uma motivação que explique isso, e o portal da CGU também não fornece nenhuma. Pelas análises, podemos ver que muitas das licitações
em 2023-01 são do exército brasileiro, comprando itens consumíveis de dia a dia.