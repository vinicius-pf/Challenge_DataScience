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

Para a coluna de gênero, também foi aplicado o Label Encoding. A categoria `Masculino` recebeu valor `0` e a `Feminino` recebeu `1`.

```python
mapa = {'Feminino': 1, 'Masculino': 0}

dados['Genero'].replace(mapa, inplace = True)
```

Apesar de eficaz, a técnica anterior pode trazer comportamentos indesejados. Ao se aplicar alguma técnica de *label encoding* em colunas com mais de 2 valores distintos, [o modelo de machine learning pode capturar algum comportamento que não existe na realidade](https://www.analyticsvidhya.com/blog/2020/03/one-hot-encoding-vs-label-encoding-using-scikit-learn/). 

Para evitar esse comportamento indesejado, será utilizada outra técnica de *encoding*, chamada de *One-Hot Encoding*. Essa técnica consiste em criar colunas novas para cada valor distinto de texto. Esse método, no entanto, pode apresentar Dummy Variable Trap. Isso acontece quando duas variáveis novas criadas pela técnica são altamente correlacionadas. 

A técnica foi utilizada para as outras variáveis categóricas presentes nos dados. Para isso, foi utilizado o método [.get_dummies()](https://pandas.pydata.org/docs/reference/api/pandas.get_dummies.html).

```python
categoricos = ['Linhas_Multiplas', 'Servico_Internet',
               'Adiconal_Seguranca', 'Adicional_Backup', 'Adiconal_Protecao',
               'Adicional_Suporte', 'Streaming_TV', 'Streaming_Filmes',
               'Tipo_Contrato', 'Metodo_Pagamento']


dados = pd.get_dummies(data = dados, columns = categoricos)
```

### Balanceando os dados

Após a aplicação das técnicas de encoding nos dados, é possível verificar se a variável target está balanceada. Caso a variável esteja desbalanceada, o modelo de machine learning pode apresentar métricas boas, apesar de estar errando mais do que o esperado. 

img desbalanceamento

Como a variável está desbalanceada, é necessário balancear os dados. Existem duas principais técnicas de balanceamento de dados: *Undersampling* e *Oversampling*. Para esse projeto foi escolhido o método *Oversampling*. Esse método consiste em aumentar a contagem da categoria com menor contagem, até que esta esteja com a mesma quantidade da categoria majoritária. Isso resolve o problema de balanceamento, mas pode gerar overfitting em modelos de machine learning.

Para o balanceamento, foi utilizado o método [SMOTE()](https://imbalanced-learn.org/stable/references/generated/imblearn.over_sampling.SMOTE.html) da biblioteca Imbalanced Learn. Esse método cria novos registros sintéticos, com base nas informações existentes.

```python
from imblearn.over_sampling import SMOTE

SEED = 42

smote = SMOTE(random_state = SEED)

X = dados.drop('Evasao', axis = 1)
y = dados['Evasao']

x_resampled, y_resampled = smote.fit_resample(X,y)

dados_balanceados = pd.concat([y_resampled,x_resampled], axis = 1)
```

Após o balanceamento dos dados, a última etapa de preparação dos dados é normalizar as variáveis numéricas.

### Normalizando os dados numéricos.

As variáveis numéricas possuem grandezas diferentes, além de assumir uma gama de valores muito grande. Para corrigir isso, 

```python
numericas = ['Tempo_Contrato', 'Valor_Mensal']

for coluna in numericas:

    minimo = dados_balanceados[coluna].min()
    maximo = dados_balanceados[coluna].max()

    dados_balanceados[coluna] = (dados_balanceados[coluna] - minimo)/(maximo-minimo)

dados_balanceados.head()
```

## Criação e avaliação dos modelos

Após as técnincas aplicadas na fase de pré-processamento dos dados, foram criados modelos de machine learning, os quais foram comparados de acordo com alguma métricas e o mais bem avaliado foi escolhido para otimização e aplicação prática com os dados faltantes descobertos na primeira análise. Para essa etapa, utilizou-se a biblioteca [Scikit-Learn](https://scikit-learn.org/stable/index.html), que possue módulos e métodos para pré-processamento dos dados e criação, avaliação e otimização de modelos de machine learning.

Antes da criação dos modelos, os dados foram separados entre dados de treino e teste utilizando o método [train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html). Os dados foram divididos em 75% para treino e 25% para testes.

```python
from sklearn.model_selection import train_test_split

SEED = 42

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=SEED)
```


### Criação

Foram escolhidos 5 modelos de machine learning para serem criados e comparados entre si. Primeiramente foi criado um modelo [Dummy Classifier] para utilizar como baseline do sistema. Além dele, foram criados modelos do tipo [Logistic Regression], [Random Forest Classifier], [Suport Vector Classifier] e [K-Neighbors Classifier].
Para o treinamento dos modelos, definiu-se uma função, que também retorna os valores previstos de acordo com os dados de teste.

```python
def executa_modelo(modelo):

    modelo.fit(X_train,y_train)
    y_pred = modelo.predict(X_test)

    return y_pred
```

Após a definição da função, os modelos foram criados e os resultados foram guardados em um dicionário python.

```python
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier
from sklearn.dummy import DummyClassifier
from sklearn.svm import SVC
from sklearn.neighbors import KNeighborsClassifier

SEED = 42

dummy = DummyClassifier(random_state = SEED)
lr = LogisticRegression(max_iter = 1000, random_state = SEED)
rf = RandomForestClassifier(random_state = SEED)
svc = SVC(random_state = SEED, probability = True)
knc = KNeighborsClassifier()

modelos = [dummy, lr, rf, svc, knc]
resultados = {}

for modelo in modelos:
    y_pred = executa_modelo(modelo)
    resultados[modelo] = y_pred
```
### Avaliação

Com os modelos criados, treinados e com os resultados calculados, a etapa de avaliação consistiu em definir as principais métricas para análise. Com as métricas calculadas, foi possível definir modelho que melhor se adequou aos dados desse projeto.

#### Métricas acurácia, precisão, recall e f1

Para a avaliação inicial dos modelos, foram calculadas as métricas de acurácia, precisão, recall e f1 para cada modelo.

```python
from sklearn import metrics

def valida_modelo(modelo, y_test, y_pred):
    acuracia = metrics.accuracy_score(y_test, y_pred).round(4)
    precisao = metrics.precision_score(y_test, y_pred).round(4)
    recall = metrics.recall_score(y_test, y_pred).round(4)
    f1 = metrics.f1_score(y_test, y_pred).round(4)

    metricas = [acuracia, precisao, recall, f1]

    matriz = metrics.confusion_matrix(y_test, y_pred)

    return metricas, matriz
```

```python
index = ['Acurácia', 'Precisão', 'Recall', 'F1']
df = pd.DataFrame(index = index)

for modelo, resultado in resultados.items():
    df[modelo], matriz = valida_modelo(modelo, y_test, resultado)
    disp = metrics.ConfusionMatrixDisplay(confusion_matrix = matriz)
    disp.plot()
```

#### Curva Roc

## Otimizando e validando o melhor modelo

### Validação cruzada

### RandomizedSearchCV

### GridSearchCV
