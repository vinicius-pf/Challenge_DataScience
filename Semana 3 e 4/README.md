# [Semana 3]

Após a conclusão da análise exploratória, a empresa requisitou que fosse desenvolvido, otimizado e avalidao um modelo de Machine Learning, com o intuito de classificar e encontrar os clientes com maior possibilidade de cancelarem o contrato com a empresa. Os principais pontos a serem desenvolvidos foram disponibilizados novamente via [Trello](https://trello.com/b/3HddKpUa/challenge-ds-semana-3).

## Preparando os dados

Após a importação dos dados corrigidos ao londo da segunda semana, a biblioteca [Pandas Profilling](https://pandas-profiling.ydata.ai/docs/master/index.html) foi utilizada para gerar um relatório em HTML com informações a respeito das variáveis, além de correlação entre as variáveis do sistema. 

Com as informações do relatório, se percebeu a necessidade de excluir algumas informações dos dados que não serão utlizadas no modelo de machine learning, assim como algumas variáveis que possuem correlação forte entre si. Além disso, foi percebido que a variável target não está balanceada, o que será necessário ser feito para que os modelos de machine learning tenham resultados melhores. 

### Removendo as variáveis indesejadas

Para remover as variáveis desnecessárias, foi utilizado o método [.drop()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop.html) da biblioteca Pandas. A variável `ID_Cliente` contém apenas valores únicos, então pode ser removida do sistema. Além disso, as variáveis `Valor_Dia` e `Valor_Total` possuem uma alta correlação com a variável `Valor_Mensal`, por isso também foram exluidas. A variável `Valor_Mensal` foi a mantida pois não possue relação com outras variável, enquanto a varíavel `Valor_Total` também possue forte correlação com a variável `Tempo_Contrato`.

Com as variáveis removidas, a base de dados pode receber técnicas de encoding, balanceamento e normalização.

### Aplicando Encoding nos dados

COmo a base de dados em texto, que não são entendidos corretamente pelos modelos de machine learning, será necessário aplicar técnicas de encoding para que os dados textuais sejam classificados e organizados como numéricos.

Para isso, foram utilizadas duas técnicas de encoding diferentes. A primeira, chamada de *Label Encoding*, tem como o objetivo designar um número sequencial para cada valor textual diferente dentro da variável. Essa técnica foi aplicada nas variáveis que contém apenas valores `Sim` e `Não`: `Evasao`, `Eh_Idoso`, `Tem_Parceiro`, `Tem_Dependentes`, `Servico_Telefone` e `Conta_Digital`.

```python
mapa = {'Sim': 1, 'Não': 0}

colunas = ['Evasao', 'Eh_Idoso', 'Tem_Parceiro', 'Tem_Dependentes', 'Servico_Telefone', 'Conta_Digital']

for coluna in colunas: 
    dados[coluna].replace(mapa, inplace = True)
```

Apesar de eficaz, a técnica anterior pode trazer comportamentos indesejados. Ao se aplicar alguma técnica de *label encoding* em colunas com mais de 2 valores distintos, [o modelo de machine learning pode capturar algum comportamento que não existe na realidade](https://www.analyticsvidhya.com/blog/2020/03/one-hot-encoding-vs-label-encoding-using-scikit-learn/). 

Para evitar esse comportamento indesejado, será utilizada outra técnica de *encoding*, chamada de *One-Hot Encoding*. Essa técnica consiste em criar colunas novas para cada valor distinto de texto. Esse método, no entanto, pode apresentar Dummy Variable Trap. Isso acontece quando duas variáveis novas criadas pela técnica são altamente correlacionadas. 

A técnica foi utilizada para as outras variáveis categóricas presentes nos dados. Para isso, foi utilizado o método [.get_dummies()](https://pandas.pydata.org/docs/reference/api/pandas.get_dummies.html).

```python
categoricos = ['Genero', 'Linhas_Multiplas', 'Servico_Internet',
               'Adiconal_Seguranca', 'Adicional_Backup', 'Adiconal_Protecao',
               'Adicional_Suporte', 'Streaming_TV', 'Streaming_Filmes',
               'Tipo_Contrato', 'Metodo_Pagamento']


dados = pd.get_dummies(data = dados, columns = categoricos)
```

### Balanceando os dados

O balanceamento nos dados é necessário



### Normalizando os dados numéricos.

## Criação e avaliação dos modelos

## Otimizando e validando o melhor modelo
