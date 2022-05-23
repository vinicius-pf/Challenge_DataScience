# Alura Challenge Data Science - 1ª Edição

Neste Repositório estão os meus projetos desenvolvidos nos meses de Maio e Junho de 2022 como parte do [Alura Challenge Data Science](https://www.alura.com.br/challenges/data-science). 

* [Sobre o Challenge](#sobre-o-challenge)
* [Etapas do projeto](#etapas-do-projeto)
    + [Semana 1](#semana-1-primeiros-passos-em-data-science)
      - [Entendendo as informações](#entendendo-as-informações)
      - [Analisando os tipos de dados](#analisando-os-tipos-de-dados)
      - [Verificando e corrigindo as inconsistências](#verificando-e-corrigindo-as-inconsistências)
      - [Traduzindo as colunas](#traduzindo-as-colunas)
      - [Coluna de contas diárias](#coluna-de-contas-diárias)
      - [Exportando os dados](#exportando-os-dados)
    + [Semana 2](#semana-2-explorando-os-dados)
      - [Página Inicial](#página-inicial)
    + [Semana 3](#semana-3-exterminando-o-futuro)
      - [Página Inicial](#página-inicial)
* [Entre em Contato](#entre-em-contato)

## Sobre o Challenge

O Alura Challenge constitui de 3 desafios semanais para que os participantes pudessem desenvolver novos conhecimentos, aprender novas ferramentas e criar um portifólio na área de ciência de dados enquanto é simulado um fluxo de trabalho em uma empresa. Ao longo do challenge, serão enviados cards pelo meio da plataforma [Trello](https://trello.com) como forma de incentivar um sistema ágil de desenvolvimento, assim como informar as requisições da empresa.

Neste desafio, a empresa Alura Voz, uma empresa de telecomunicações, deseja reduzir a taxa de evasão de seus clientes. Para isso, a empresa disponibilizou uma base de dados no formato *JSON* com informações sobre seus clientes. Durante as próximas semanas, os dados serão tratados, explorados e utilizados como base para a criação de um modelo de machine learning, com o intuito de encontrar clientes com a possibilidade de fazer o cancelamento de seus contratos.

## Etapas do projeto

### Semana 1 Primeiros passos em Data Science

[Link para o Notebook](https://github.com/vinicius-pf/Challenge_DataScience/blob/main/Semana%201/Tratamento_dos_Dados.ipynb)

Na primeira semana, foi disponibilizada a base de dados por meio do [Trello](https://trello.com/b/JdUXpLrp/challenge-ds-semana-1). No mesmo local, foram enviados algumas requisições da empresa:

- Entender quais informações a base de dados possui
- Analisar quais os tipos de dados
- Verificar quais são as inconsistências nos dados
- Corrigir as inconsistências nos dados

Além desses pedidos, houveram 2 pedidos extras:
- Traduzir as colunas
- Criar coluna de contas diárias

Para o desenvolvimento da solução, foi utilizada a linguagem [Python](https://www.python.org) e a biblioteca [Pandas](https://pandas.pydata.org). Como IDE foi utilizado o [Google Colab](https://colab.research.google.com).

#### Entendendo as informações

Com os [dados disponíveis](https://github.com/vinicius-pf/Challenge_DataScience/blob/main/Semana%201/dados/dados.json), eles foram importados para um [Jupyter Notebook](placement). Porém, foi percebido que as informações necessárias para as análises estavam contidas dentro das colunas do DataFrame original.

![Dados Iniciais](https://user-images.githubusercontent.com/6025360/168080005-8a5fa136-d21c-4da5-8c56-16e2ab033041.png)

Para resolver isso, foi utilizado o método [`json_normalize()`](https://pandas.pydata.org/docs/reference/api/pandas.json_normalize.html). Esse método teve que ser feito para 4 colunas. Para cada coluna, foi criado um novo DataFrame, que no final foi juntado com o DataFrame original com a função [`merge()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html). Para concluir, as colunas originais, que foram expandidas, foram retiradas do DataFrame, permitindo assim uma análise mais direta dos dados.

![Dados Normalizados](https://user-images.githubusercontent.com/6025360/168080046-7e1348fd-07dd-40a0-b47c-e518e199436f.png)

Além da base de dados, a empresa também disponibilizou um dicionário. Esse dicionário traz um comentário sobre cada coluna, explicando o seu significado. Ele será utilizado em conjunto com as análises para definir qual os tipos de cada variável e para ser feita a tradução dos nomes das colunas e dos dados.

Nome da coluna | Dicionário da empresa
-------|------------------
`customerID`| Código único para o cliente da empresa
`Churn`| se o cliente deixou ou não a empresa
`gender`| gênero (masculino e feminino)
`SeniorCitizen`| informação sobre um cliente ter ou não idade igual ou maior que 65 anos
`Partner`| se o cliente possui ou não um parceiro ou parceira
`Dependents`| se o cliente possui ou não dependentes
`tenure`| meses de contrato do cliente
`PhoneService`| assinatura de serviço telefônico
`MultipleLines`| assinatura de mais de uma linha de telefone
`InternetService`| assinatura de um provedor internet
`OnlineSecurity`| assinatura adicional de segurança online
`OnlineBackup`| assinatura adicional de backup online
`DeviceProtection`| assinatura adicional de proteção no dispositivo
`TechSupport`| assinatura adicional de suporte técnico, menos tempo de espera
`StreamingTV`| assinatura de TV a cabo
`StreamingMovies`| assinatura de streaming de filmes
`Contract`| tipo de contrato
`PaperlessBilling`| se o cliente prefere receber online a fatura
`PaymentMethod`| forma de pagamento
`Charges.Monthly`| total de todos os serviços do cliente por mês
`Charges.Total`| total gasto pelo cliente

#### Analisando os tipos de dados

Após a organização inicial, as colunas foram analisadas com mais clareza. Primeiramente foi verificado se haviam dados em branco com o método [`.info()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.info.html), o que não foi percebido. Após isso, foi definida uma função para a análise de colunas que retornava 3 informações: Valores únicos, contagem dos valores e tipo relacionado à coluna:

```python
def analisa_coluna(coluna):

    unicos = dados_normal[coluna].unique()
    contagem = dados_normal[coluna].value_counts()
    tipo = dados_normal[coluna].dtype

    print(f'Valores únicos:')
    print(unicos)
    print('----------------')
    print(f'Contagem de valores:')
    print(contagem)
    print('----------------')
    print(f'Tipo da coluna:')
    print(tipo)
    print('----------------')

```

Aplicando a função em todas as colunas do DataFrame, foi percebido que:

1. Há dados em branco na coluna `Churn`. Eles não foram percebidos pelo primeiro método utilizado, mas esses dados devem ser tratados.

2. A coluna `SeniorCitizen` possue valores `0` e `1`, ao invés de `Yes` e `No` presentes nas outras colunas booleanas. Isso será unificado para depois poder ser feita uma tradução dos dados.

3. As colunas extraídas durante o processo de normalização da base de dados aparentam ter informações condizentes entre si. 

4. O tipo da coluna `Charges.Total` deve ser alterado para `float64`.

#### Verificando e corrigindo as inconsistências

Os valores em branco da coluna `Churn` serão divididos em outro arquivo de dados. Esta é a coluna alvo para a empresa, com informações dos clientes que evadiram. Manter os dados em branco nos dados utilizados para exploração visual e criação de modelos de machine learning poderá trazer resultados equivocados. Apesar disso, essas informações podem ser utilizadas após a fase de testes e validação do modelo de machine learning, tendo em vista que o objetivo é encontrar os clientes com maior potencial de fazer o cancelamento do contrato.

Para a coluna `SeniorCitizen`, foi utilizado o método [`.replace()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.replace.html). Com a transformação feita, as traduções serão mais efetivas.

As colunas extraídas contendo as informações sobre linhas telefônicas e serviçõs de internet possuem informações extras: a coluna `MultipleLines` contém valores para `Yes`, `No` e `No Phone Service`. Já nas colunas `InternetService`, `OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`, `StreamingTV` e `StreamingMovies` também contém valores `Yes` e `No`, com o adicional de `No Internet Service`. Esses valores extras serão mantidos no momento: apesar de trazer informações repetidas, algumas análises podem sofrer alterações. Caso seja necessário, essas informações podem ser alteradas em um outro momento.

Ao tentar alterar o tipo de dados da coluna `Charges.Total` com o método [`.to_numeric()`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.to_numeric.html), ocorreu um erro. Assim como na coluna `Churn`, haviam valores faltantes que não foram percebidos inicialmente.

![Charges.Total Vazios](https://user-images.githubusercontent.com/6025360/168160233-f9837539-0d59-4bda-a066-de1fc62c6fcf.png)

<sub>O DataSet na imagem foi alterado para clareza.</sub>

Analisando com calma, foi percebido que os valores faltantes da coluna correspondem aos clientes que possuem 0 meses na coluna tenure. Para corrigir isso, os valores em branco foi substituido por `0`. 

Ao final das correções, as informações puderam ser traduzidas e divididas de acordo com a coluna `Churn`.

#### Traduzindo as colunas

A tradução das colunas levou em conta a tradução literal, as informações contidas na base de dados e também o dicionário disponibilizado pela empresa.

Nome da coluna | Tradução da coluna | Tipo de variável
-------|------------------ | ------------
`customerID`| ID_Cliente | categórica nominal
`Churn`| Evasao | categórica nominal
`gender`| Genero | categórica nominal
`SeniorCitizen`| Eh_Idoso | categórica nominal
`Partner`| Tem_Parceiro | categórica nominal
`Dependents`| Tem_Dependentes | categórica nominal
`tenure`| Tempo_Contrato | numérica discreta
`PhoneService`| Servico_Telefone | categórica nominal
`MultipleLines`| Linhas_Multiplas | categórica nominal
`InternetService`| Servico_Internet | categórica nominal
`OnlineSecurity`| Adiconal_Seguranca | categórica nominal
`OnlineBackup`| Adicional_Backup | categórica nominal
`DeviceProtection`| Adiconal_Protecao | categórica nominal
`TechSupport`| Adicional_Suporte | categórica nominal
`StreamingTV`| Streaming_TV | categórica nominal
`StreamingMovies`| Streaming_Filmes | categórica nominal
`Contract`| Tipo_Contrato | categórica nominal
`PaperlessBilling`| Conta_Digital | categórica nominal
`PaymentMethod`| Metodo_Pagamento | categórica nominal
`Charges.Monthly`| Valor_Mensal | numérica discreta<sup>1</sup>
`Charges.Total`| Valor_Total | numérica discreta<sup>1</sup>

([1](https://www150.statcan.gc.ca/n1/edu/power-pouvoir/ch8/5214817-eng.htm)) Apesar de teoricamente receber qualquer valor, tem o limite de 2 casas decimais.

Além dos nomes das colunas, também foram traduzidos os dados.

#### Coluna de contas diárias

A empresa pediu uma coluna extra, com o valor de gasto diário de cada cliente. Para a criação, foi utilizada a coluna `Charges.Monthly` como base e os valores foram dividos por 30, que é a média de dias por mês. O valor foi arredondado par 2 casas decimais, que é o padrão apresentado nas outras colunas de valores monetários presentes na base de dados.

Como o pedido da empresa informava a posição desejada, primeiramente foi criado uma [Series do pandas](https://pandas.pydata.org/docs/reference/api/pandas.Series.html#pandas.Series) para receber os valores. Depois, foi utilizado o método [insert()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.insert.html).

#### Exportando os dados

Ao final das análises e tratamentos, os dados foram exportados para arquivos csv. Antes da exportação, o dataset foi dividido em 2: o [primeiro](https://github.com/vinicius-pf/Challenge_DataScience/blob/main/Semana%201/dados/dados_evasao_vazio.csv) recebeu as linhas em que a coluna `Evasao` estava em branco e o [outro](https://github.com/vinicius-pf/Challenge_DataScience/blob/main/Semana%201/dados/dados_evasao_completos.csv) recebeu as linhas com informação da evasão ou não do cliente..


### Semana 2 Explorando os dados

[Link para o notebook](link)

Para a segunda semana, a empresa requisitou, por meio do [Trello](https://trello.com/b/uUsVCrPb/challenge-ds-semana-2), que algumas visualizações fossem criadas. O foco das vizualizações será a variável `Churn`, tendo em vista que é o foco do projeto. As visualizações foram criadas utilizando a biblioteca [Plotly](https://plotly.com/graphing-libraries/). Essa biblioteca permite criação de gráficos interativos. Porém, por se tratar de uma biblioteca escrita em Javascript, há erros de exibição pelo github. Para corrigir isso e visualizar o Jupyter Notebook com as análises, deve-se visualizar o mesmo pelo meio do Google Colab.

Antes disso, porém, há a necessidade de se corrigir alguns erros cometidos durante a primeira semana

#### Correção de erros

Durante a limpeza de dados, foi decidido separar os dados que estavam em branco na coluna `Churn`. Isso foi feito, porém não da maneira mais correta. Apesar de não aparecerem valores na coluna, os valores estão presentes como espaço em branco. Para análises futuras, pode ser necessário alterar para valores nulos.

Outro equivoco ocorreu no tratamento de dados faltantes da coluna `Charges.Total`. Em um primeiro momento, os valores faltantes foram substituidos por `0`. A empresa, no entanto, requisitou que os valores incluidos fossem o mesmo da coluna `Charges.Monthly`.

Após a correção desses problemas, a exploração dos dados com análises gráficas poderá seguir com mais clareza.

#### Visualizando a distribuição da variável target

Para visualizar a variável target, foram utilizados dois tipos de gráfico: um histograma e um gráfico de pizza.

O histograma é um bom gráfico para comparar distribuições e contagens entre valores das variáveis. Em variáveis numéricas, ele ajuda a entender a distribuição da variável e comparar alguns valores. Como esta é uma variável categórica que recebe apenas dois valores, o gráfico foi uma solução simples para a comparação das contagens e percentual dos valores.

<img src="https://user-images.githubusercontent.com/6025360/169667731-df677f15-ef2d-4e99-acfa-5badb6a33759.png" width = 500, height = 500/>

Com o histograma percebe-se que há mais clientes que não evadiram do que clientes evadidos. Para entender melhor a taxa de evasão, pode ser utilizado um gráfico de pizza. Apesar de não ser o mais indicado para análises gráficas, esse tipo de gráfico permite visualizar o percentual com mais cuidado e clareza.

<img src= "https://user-images.githubusercontent.com/6025360/169667946-75208814-9ef6-43e3-aa5e-6f87ca7f1807.png" width = 500, height = 500/>

Com esses gráficos, é percebido que: 26,5% dos clientes da empresa cancelaram os planos contratados. Apesar da empresa não apresentar metas para a taxa de evasão, a taxa se mostra alta em relação à outros casos.

Com a distribuição verificada, pode-se iniciar a próxima etapa e comparar como as outras variáveis do sistema se distribuiem em relação a variável target. 

#### Analisando a correlação entre as variáveis

Antes de fazer as análises com as outras variáveis, foi analisada a correlação entre as mesmas. Primeiro foi utilizado o método [.corr()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.corr.html) da biblioteca Pandas. Esse método calcula a correlação entre as variáveis numéricas de um dataset utilizando o método Pearson, podendo esse método ser alterado. Porém, as variáveis presentes no dataset não são apenas numéricas, mas também categóricas. Para contornar esse problema, foi utilizado o módulo python [Dython](http://shakedzy.xyz/dython/). Esse módulo foi criado para facilitar algumas etapas de análise de dados, incluindo um método para o cálculo de correlação entre variáveis numéricas e associação de variáveis categóricas. 

O método [associations()](http://shakedzy.xyz/dython/modules/nominal/) retorna duas informações: uma tabela com as associações e correlações, que a biblioteca calcula de acordo com o tipo de cada variável, e um mapa de calor com os valores da tabela, para fácil análise visual.

![image](https://user-images.githubusercontent.com/6025360/169669037-c00220b4-9206-4a53-8d24-d82016840422.png)

Com a correlação calculada, é possível perceber alguns pontos:

1 - As colunas `Servico_Internet`,`Adicional_Seguranca`,`Adicional_Backup`,`Adicional_Protecao`,`Adicional_Suporte`,`Streaming_TV`,`Streaming_Filmes` possuem forte correlação entre si. Há valores nas outras colunas que são dependentes da coluna `Servico_Internet`: o cliente que não contratou serviço de internet sempre terá o mesmo valor nas outras colunas.

2 - Há uma alta relação entre `Servico_Telefone` e `Linhas_Multiplas`. Isso pode ter o mesmo motivo que a relação das colunas a respeito de serviços online.

3 - A coluna *target*, `Evasao`, apresenta relação fraca com as colunas `Servico_Internet`,`Adicional_Seguranca`,`Adicional_Backup`,`Adicional_Protecao`,`Adicional_Suporte`,`Streaming_TV`, `Streaming_Filmes`, `Tipo_Contrato`, `Tempo_Contrato`, `Metodo_Pagamento` e `Valor_Total`. Com as outras colunas há uma relação muito fraca.

4 - A variável `Servico_Internet` tem uma relação forte com as colunas `Valor_Mensal` e `Valor_Dia`. As colunas relacionadas à `Servico_Internet` também possuem correlação com as variáveis financeiras, porém de menor intensidade.

5 - A variável `Tempo_Contrato` possue relação com as variáveis `Tipo_Contrato` e `Metodo_Pagamento`. Também possuem relação pouco forte com os serviços de internet e a coluna `Linhas_Multiplas`.

6 - As colunas `Tem_Parceiro` e `Tem_Dependentes` tem uma relação pouco forte entre si. As outras colunas não possuem relação com as variáveis.

#### Analisando a variável Churn em conjunto com outras variáveis

Para as análises extras da variável target, foram selecionadas algumas colunas que possuem relação forte com a mesma. Para isso, foram criadas visualizações de distribuição e frequência nas colunas `Valor_Mensal`, `Tempo_Contrato`, `Servico_Internet`, `Servico_Telefone`, `Tipo_Contrato`, `Conta_Digital` e `Metodo_Pagamento`.

Analisando a coluna `Valor_Mensal`, se percebe que a média de gastos mensais dos clientes que evadiram a empresa é maior do que aquelas que não cancelaram o contrato. Isso pode ser uma explicação do motivo do cancelamento: descontentamento com o valor pago.

img valor

Para a coluna `Tempo_Contrato`, oberva-se que as pessoas que evadiram possuem uma média menor de tempo de contrato, se comparado as pessoas que ficaram na empresa. Com essa informação, foi verificado também a distribuição de evasão de acordo com o tempo de contrato. Ao fazer isso, foi possível entender que para clientes com tempo de contrato menor que 10 meses, há uma taxa de evasão maior do que o resto do dataset. Outro ponto importante foi visualizado: entre os clientes com 1 e 2 meses de contrato, há mais clientes que evadiram a empresa do que estão no momento nela.

img tempo

A variável `Servico_Internet` é outra coluna que apresentou distribuição atípica. Os clientes que assinaram o serviço de `Fibra Óptica` da empresa possuem uma taxa de evasão maior que a dos clientes totais da empresa. Para entender melhor esse comportamento foram feitas análises subsequentes.

img fibra

Para a coluna `Servico_Telefone`, a análise não mostrou diferença de taxa de evasão em comparação com a taxa de todos os clientes. A maioria dos clientes possue serviço de telefonia contratado com a empresa.

img telefone

Durante a análise da coluna `Tipo_Contrato`, um comportamento parecido com a coluna `Servico_Internet` foi percebido. Clientes que optam por um plano mensal possuem taxa de evasão elevada. No entanto, foi percebido um comportamento diferente nas outras possibilidades também. Clientes com plano anual possuem uma taxa de evasão menor do que aqueles com plano mensal. Já clientes com plano bianual possuem a menor taxa de evasão: apenas 3% dos clientes com plano bianual evadiram da empresa.

img tipo

Na coluna `Conta digital`, se percebe uma taxa maior de evasão para clientes que possuem conta digital, enquanto clientes que não possuem o serviço aparentam uma taxa de evasão menor. 

img conta

Por último, na coluna `Metodo_Pagamento`, aqueles que optam por pagamento via `Cheque eletrônico` possuem uma taxa de evasão elevada em comparação aos outros clientes. Os outros métodos de pagamento possuem taxa de evasão parecida e menor que a taxa de evasão global.

img metodo

#### Análises extras

#### 


### Semana 3 Exterminando o futuro

## Entre em contato

LinkedIn: [https://www.linkedin.com/in/viniciuspf/](https://www.linkedin.com/in/viniciuspf/)

E-mail: vinicius-pf@outlook.com






