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
    + [Semana 2](#semana-2-explorando-os-dados)
      - [Página Inicial](#página-inicial)
    + [Semana 3](#semana-3-exterminando-o-futuro)
      - [Página Inicial](#página-inicial)
* [Entre em Contato](#entre-em-contato)

## Sobre o Challenge

O Alura Challenge constitui de 3 desafios semanais para que os participantes pudessem desenvolver novos conhecimentos, aprender novas ferramentas e criar um portifólio na área de Business Inteligence e ciência de dados, enquanto é simulado um fluxo de trabalho em uma empresa. Ao longo do challenge, serão enviados cards pelo meio da plataforma [Trello](https://trello.com) como forma de incentivar um sistema ágil de desenvolvimento, assim como informar as requisições da empresa.

Neste desafio, a empresa Alura Voz, uma empresa de telecomunicações, deseja reduzir a taxa de evasão de seus clientes. Para isso, a empresa disponibilizou uma base de dados no formato JSON com informações sobre seus clientes. Durante as próximas semanas, os dados serão tratados, explorados e utilizados como base para a criação de um modelo de machine learning, com o intuito de encontrar clientes com a possibilidade de fazer o cancelamento de seus contratos.

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
customerID| Código único para o cliente da empresa
Churn| se o cliente deixou ou não a empresa
gender| gênero (masculino e feminino)
SeniorCitizen| informação sobre um cliente ter ou não idade igual ou maior que 65 anos
Partner| se o cliente possui ou não um parceiro ou parceira
Dependents| se o cliente possui ou não dependentes
tenure| meses de contrato do cliente
PhoneService| assinatura de serviço telefônico
MultipleLines| assinatura de mais de uma linha de telefone
InternetService| assinatura de um provedor internet
OnlineSecurity| assinatura adicional de segurança online
OnlineBackup| assinatura adicional de backup online
DeviceProtection| assinatura adicional de proteção no dispositivo
TechSupport| assinatura adicional de suporte técnico, menos tempo de espera
StreamingTV| assinatura de TV a cabo
StreamingMovies| assinatura de streaming de filmes
Contract| tipo de contrato
PaperlessBilling| se o cliente prefere receber online a fatura
PaymentMethod| forma de pagamento
Charges.Monthly| total de todos os serviços do cliente por mês
Charges.Total| total gasto pelo cliente

#### Analisando os tipos de dados

Após a organização inicial, as colunas foram analisadas com mais clareza. Primeiramente foi verificado se haviam dados em branco com o método [`.info()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.info.html), o que não foi percebido. Após isso, foi definida uma função para a análise de colunas que retornava 3 informações: Valores únicos, contagem dos valores e tipo relacionado à coluna:

![Função](https://user-images.githubusercontent.com/6025360/168085389-be19e674-d888-4281-b1ee-3758625d6194.png)

Aplicando a função em todas as colunas do DataFrame, foi percebido que:

1. Há dados em branco na coluna 'Churn'. Eles não foram percebidos pelo primeiro método utilizado, mas esses dados devem ser tratados.

2. A coluna 'SeniorCitizen' possue valores '0' e '1', ao invés de 'Yes' e 'No' presentes nas outras colunas booleanas. Isso será unificado para depois poder ser feita uma tradução dos dados.

3. As colunas extraídas durante o processo de normalização da base de dados aparentam ter informações condizentes entre si. 

4. O tipo da coluna 'Charges.Total' deve ser alterado para float64.


#### Verificando e corrigindo as inconsistências

#### Traduzindo as colunas

#### Coluna de contas diárias

### Semana 2 Explorando os dados


### Semana 3 Exterminando o futuro

## Entre em contato

LinkedIn: [https://www.linkedin.com/in/viniciuspf/](https://www.linkedin.com/in/viniciuspf/)

E-mail: vinicius-pf@outlook.com






