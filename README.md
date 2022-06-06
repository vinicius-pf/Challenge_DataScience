# Alura Challenge Data Science - 1ª Edição

Neste Repositório estão os meus projetos desenvolvidos nos meses de Maio e Junho de 2022 como parte do [Alura Challenge Data Science](https://www.alura.com.br/challenges/data-science). 

* [Sobre o Challenge](#sobre-o-challenge)
* [Etapas do projeto](#etapas-do-projeto)
    + [Semana 1: Primeiros passos em Data Science](#semana-1-primeiros-passos-em-data-science)
    + [Semana 2: Explorando os dados](#semana-2-explorando-os-dados)
    + [Semana 3: Exterminando o futuro](#semana-3-exterminando-o-futuro)
* [Entre em Contato](#entre-em-contato)

## Sobre o Challenge

O Alura Challenge constituiu de 3 desafios semanais para que os participantes pudessem desenvolver novos conhecimentos, aprender novas ferramentas e criar um portifólio na área de ciência de dados enquanto é simulado um fluxo de trabalho em uma empresa. Ao longo do challenge, foram enviados cards pelo meio da plataforma [Trello](https://trello.com) como forma de incentivar um sistema ágil de desenvolvimento, assim como informar as requisições da empresa.

Neste desafio, a empresa Alura Voz, uma empresa de telecomunicações, deseja reduzir a taxa de evasão de seus clientes. Para isso, a empresa disponibilizou uma base de dados no formato *JSON* com informações sobre seus clientes. Durante semanas do desafio, os dados foram tratados, explorados e utilizados como base para a criação de um modelo de machine learning, com o intuito de encontrar clientes com a possibilidade de fazer o cancelamento de seus contratos.

## Etapas do projeto

### Semana 1 Primeiros passos em Data Science

[Link para o Notebook](https://github.com/vinicius-pf/Challenge_DataScience/blob/main/Semana%201/Tratamento_dos_Dados.ipynb)

Na primeira semana, foi disponibilizada a base de dados por meio do [Trello](https://trello.com/b/JdUXpLrp/challenge-ds-semana-1). No mesmo local, foram enviados algumas requisições da empresa. Dentre os pedidos, estavam análises de informações e tipos de dados da base de dados, além de verificação e correção de inconsistências. Também foi requisitada uma tradução das informações presentes e criação de uma nova coluna, com valores gastos diariamente pelos clientes com base no valor pago mensalmente.

Para o desenvolvimento da solução, foi utilizada a linguagem [Python](https://www.python.org) e a biblioteca [Pandas](https://pandas.pydata.org). Como IDE foi utilizado o [Google Colab](https://colab.research.google.com).

Ao longo das análises, foram percebidos que a coluna `Evasao`, a variável *target* e que informa se o cliente evadiu a empresa ou não, continha valores em branco. O mesmo aconteceu para a variável `Charges.Total`, que apresentava valores em branco para os clientes com menos de 1 mês de contrato. Para este caso, os valores em branco foram substituídos por 0. No caso da variável target do projeto, os registros foram removidos para as análises exploratória e gráficas. No entanto eles foram salvos em uma outra base de dados, para que o modelo de machine learning que será desenvolvido possa prever essas informações, depois enviando a empresa os clientes que possam evadir.

### Semana 2 Explorando os dados

[Link para o notebook](https://colab.research.google.com/github/vinicius-pf/Challenge_DataScience/blob/Semana-2/Semana%202/%20Analises_Graficas.ipynb)

Para a segunda semana, a empresa solicitou, por meio do [Trello](https://trello.com/b/uUsVCrPb/challenge-ds-semana-2), que algumas visualizações fossem criadas. O foco das vizualizações foi a variável `Churn`, para que padrões e possíveis correlações pudessem ser percebidas. As visualizações foram criadas utilizando a biblioteca [Plotly](https://plotly.com/graphing-libraries/), que permite a criação de gráficos interativos. Porém, por se tratar de uma biblioteca escrita em Javascript, há erros de exibição pelo github. Para corrigir isso e visualizar o Jupyter Notebook com as análises, o link fornecido abre a visualização do notebook pelo meio do google colab.

Durante as análises, foi percebido um desbalanceamento da variável target, assim como uma alta taxa de evasão da empresa, com mais de 25% de clientes da base de dados cancelando o contrato. Além disso, percebeu-se que algumas características apresentam taxa de evasão maior, como tipo de contrato, de internet ou de pagamento. Também foi analisado a taxa de evasão por tempo de contrato, onde se percebeu uma alta taxa de evasão nos primeiros meses de contrato, em contraste com os clientes com maior tempo de contrato.

### Semana 3 Exterminando o futuro

[Link para o notebook](https://colab.research.google.com/github/vinicius-pf/Challenge_DataScience/blob/Semana-2/Semana%202/%20Analises_Graficas.ipynb)

Na terceira e última semana, a empresa demandou que fosse criado, otimizado e validado um modelo de machine learning que pudesse prever se um cliente irá ou não cancelar seu contrato com a empresa. Para que a demanda pudesse ser alcançada, foi utilizada a biblioteca [Pandas-profilling]() para análise rápida dos dados. Com isso se percebeu a necessidade da remoção das colunas `ID_Cliente`, por haver apenas valores únicos, e `Valor_Dia` e `Valor_Total` por terem alta correlação com a variável `Valor_Mensal`. Após isso, foram aplicadas técnicas de *encoding* e balanceamento dos dados com *oversampling*.

Após o pré-processamento dos dados, foram criados 5 modelos de machine learning, sendo um Dummy Classifier para controle e 4 modelos com abordagens diferentes para classificação. Esses modelos foram treinados e avaliados utilizando métricas de avaliação matemáticas e gráficas, para a escolha do melhor modelo. O melhor modelo então passou por uma validação cruzada e otimização dos hiperparâmetros para que o modelo pudesse ser posto em prática.

## Entre em contato

LinkedIn: [https://www.linkedin.com/in/viniciuspf/](https://www.linkedin.com/in/viniciuspf/)

E-mail: vinicius-pf@outlook.com
