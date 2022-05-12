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

O Alura Challenge constitui de 3 desafios semanais para que os participantes pudessem desenvolver novos conhecimentos, aprender novas ferramentas e criar um portifólio na área de Business Inteligence e ciência de dados, enquanto é simulado um fluxo de trabalho em uma empresa. Ao longo do challenge, serão enviados cards pelo meio da plataforma [Trello](https://trello.com) como forma de incentivar um sistema ágil de desenvolvimento, assim como informar as requisições da empresa.

Neste desafio, a empresa Alura Voz, uma empresa de telecomunicações, deseja reduzir a taxa de evasão de seus clientes. Para isso, a empresa disponibilizou uma base de dados no formato *JSON* com informações sobre seus clientes. Durante as próximas semanas, os dados serão tratados, explorados e utilizados como base para a criação de um modelo de machine learning, com o intuito de encontrar clientes com a possibilidade de fazer o cancelamento de seus contratos.

## Etapas do projeto

### Semana 1 Primeiros passos em Data Science

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

Ao final das análises e tratamentos, os dados foram exportados para arquivos csv. Antes da exportação, o dataset foi dividido em 2: o [primeiro](https://github.com/vinicius-pf/Challenge_DataScience/blob/main/Semana%201/dados/dados_evasao_vazio.csv) recebeu as linhas em que a coluna `Evasao` estava em branco e o [outro](https://github.com/vinicius-pf/Challenge_DataScience/blob/main/Semana%201/dados/dados_evasao_completos.csv) sem esses valores.


### Semana 2 Explorando os dados


### Semana 3 Exterminando o futuro

## Entre em contato

LinkedIn: [https://www.linkedin.com/in/viniciuspf/](https://www.linkedin.com/in/viniciuspf/)

E-mail: vinicius-pf@outlook.com






