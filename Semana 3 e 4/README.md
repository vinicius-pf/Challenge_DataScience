# [Semana 3]

Após a conclusão da análise exploratória, a empresa requisitou que fosse desenvolvido, otimizado e avalido um modelo de Machine Learning, com o intuito de classificar e encontrar os clientes com maior possibilidade de cancelarem o contrato. Os principais pontos a serem desenvolvidos foram disponibilizados novamente via [Trello](https://trello.com/b/3HddKpUa/challenge-ds-semana-3).

## Preparando os dados

Após a importação dos dados corrigidos ao longo da segunda semana, a biblioteca [Pandas Profilling](https://pandas-profiling.ydata.ai/docs/master/index.html) foi utilizada para gerar um relatório em HTML com informações a respeito das variáveis, além de correlação entre as variáveis do sistema. 

Com as informações do relatório, se percebeu a necessidade de excluir algumas informações dos dados que não serão utlizadas no modelo de machine learning, assim como algumas variáveis que possuem correlação forte entre si. Além disso, foi percebido que a variável target não está balanceada, o que será necessário ser feito para que os modelos de machine learning tenham resultados melhores. 

### Removendo as variáveis indesejadas

Para remover as variáveis desnecessárias, foi utilizado o método [.drop()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop.html) da biblioteca Pandas. A variável `ID_Cliente` contém apenas valores únicos, então pode ser removida do sistema. Além disso, as variáveis `Valor_Dia` e `Valor_Total` possuem uma alta correlação com a variável `Valor_Mensal`, por isso também foram exluidas. A variável `Valor_Mensal` foi a mantida pois não possue relação com outras variável, enquanto a varíavel `Valor_Total` também possue forte correlação com a variável `Tempo_Contrato`.

Com as variáveis removidas, a base de dados pode receber técnicas de encoding, balanceamento e normalização.

### Aplicando Encoding nos dados

Como a base de dados em texto, que não são entendidos corretamente pelos modelos de machine learning, será necessário aplicar técnicas de encoding para que os dados textuais sejam classificados e organizados como numéricos.

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

Para evitar essa captura, será utilizada outra técnica de *encoding*, chamada de *One-Hot Encoding*. Essa técnica consiste em criar colunas novas para cada valor distinto de texto. Esse método, no entanto, pode apresentar Dummy Variable Trap. Foram executados testes para entender e encontrar esse comportamento. Apenas a variável `Valor_Mensal` teve resultado expressivo. Como foi apenas essa variável, e com as outras não apresentando problemas, a variável foi mantida no sistema.

A técnica foi utilizada para as outras variáveis categóricas presentes nos dados. Para isso, foi utilizado o método [.get_dummies()](https://pandas.pydata.org/docs/reference/api/pandas.get_dummies.html).

```python
categoricos = ['Linhas_Multiplas', 'Servico_Internet',
               'Adiconal_Seguranca', 'Adicional_Backup', 'Adiconal_Protecao',
               'Adicional_Suporte', 'Streaming_TV', 'Streaming_Filmes',
               'Tipo_Contrato', 'Metodo_Pagamento']


dados = pd.get_dummies(data = dados, columns = categoricos)
```

### Balanceando os dados

Após a aplicação das técnicas de encoding nos dados, em conjunto com as análises do Pandas-Profilling, foi percebido que a variável target está desbalanceada. Isso é um problema, pois o modelo de machine learning pode apresentar métricas boas, apesar de estar errando mais do que o esperado. 

![image](https://user-images.githubusercontent.com/6025360/172242364-b498e6e3-e4da-4ebf-b8d4-89fe70cd1099.png)

Como a variável está desbalanceada, é necessário balancear os dados. Existem duas principais técnicas de balanceamento de dados: *Undersampling* e *Oversampling*. Para esse projeto foi escolhido o método *Oversampling*. Essa técnica consiste em aumentar a contagem da categoria com menor contagem, até que esta esteja com a mesma quantidade da categoria majoritária. Isso resolve o problema de balanceamento, mas pode gerar overfitting em modelos de machine learning.

Para o balanceamento, foi utilizado o método [SMOTE()](https://imbalanced-learn.org/stable/references/generated/imblearn.over_sampling.SMOTE.html) da biblioteca Imbalanced Learn. Esse método cria novos registros sintéticos, com base nas informações existentes. Esse método tem, intríseco ao modelo, uma aleatoriedade que não permite que os resultados sejam replicados com facilidade. Para isso, foi definido o paramêtro *random_state* em *42*. Outros métodos utilizados em conjunto com o modelo de machine learning tem o mesmo problema, que foi corrigido da mesma forma.

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

Antes da criação dos modelos, os dados foram separados entre dados de treino, que servem para adequar o modelo aos dados, e teste, que servem para que o modelo faça as previsões de classificação, utilizando o método [train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html). Por padrão, é [utilizado 30% para dados de teste e 70% para dados de treino](https://www.geeksforgeeks.org/how-to-split-a-dataset-into-train-and-test-sets-using-python/).

```python
from sklearn.model_selection import train_test_split

SEED = 42

X = dados_balanceados.drop('Evasao', axis = 1)
y = dados_balanceados['Evasao']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=SEED, stratify = y)
```

Após a separação dos dados, pode-se dar início a criação dos modelos de machine learning.

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

Para a avaliação inicial dos modelos, foram calculadas as métricas de acurácia, precisão, recall e f1 para cada modelo. Além disso, também foi criada e exibida a matriz de confusão de cada modelo.

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

Após a definição, a função foi executada para cada modelo já treinado. Os resultados das métricas foram inseridos em um DataFrame do Pandas para melhor visualização, e depois comparados.

```python
index = ['Acurácia', 'Precisão', 'Recall', 'F1']
df = pd.DataFrame(index = index)

for modelo, resultado in resultados.items():
    df[modelo], matriz = valida_modelo(modelo, y_test, resultado)
    disp = metrics.ConfusionMatrixDisplay(confusion_matrix = matriz)
    disp.plot()
```

![image](https://user-images.githubusercontent.com/6025360/172242419-3b9a4aca-2f92-4589-aa32-4ca163085cf2.png)

Com os resultados, foi possível perceber que o melhor modelo foi o Random Forest Classifier, seguido pelo Logistic Regression e SVC. Para confirmar os testes e escolher o melhor modelo, a curva ROC dos dois melhores modelos foi inserida em um gráfico.

![image](https://user-images.githubusercontent.com/6025360/172242428-45710a54-1690-4acc-9b70-b8709696b909.png)

Com a curva ROC calculada, percebeu-se que o modelo Random Forest Classifier obteve os melhores resultados com esse dataset. Com o modelo definido, a etapa seguinte foi validar o modelo utilizando o cross-validate e otimizar o modelo, afim de melhorar os resultados obtidos e evitar o *overfitting*.

## Otimizando e validando o melhor modelo

Após a definição do melhor modelo, o primeiro passo foi efetuar uma validação cruzada utilizando o método [StratifiedKFold](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedKFold.html). Depois da validação cruzada, foi necessário ajustar os hiperparâmetros do modelo.

### Validação cruzada

Para a validação cruzada, foram definidas duas funções. Uma para calcular as métricas utilizando a validação cruzada e outra para exibir a média aritimética dos resultados de acurácia dos dados de treino e teste.

```python
from sklearn.model_selection import StratifiedKFold
from sklearn.model_selection import cross_validate

def validacao_cruzada(modelo):
    cv = StratifiedKFold(n_splits = 10)
    resultados = cross_validate(modelo, X, y, cv = cv, return_train_score=True)
    imprime_resultado(resultados)
```

```python
def imprime_resultado(resultados):

    acuracia_teste = resultados['test_score'].mean() * 100
    acuracia_treino = resultados['train_score'].mean() * 100

    print(f'Acurácia: teste = {acuracia_teste:.2f}, treino = {acuracia_treino:.2f}')
```

![image](https://user-images.githubusercontent.com/6025360/172242465-2bff9d9d-de09-4bb3-aa0d-fc57fa31b22d.png)

Ao ser efetuada, a validação cruzada mostrou que a acurácia do modelo para dados de treino está em 99,80%. Isso pode ser um sinal de *overfitting*, afinal para os dados de teste a acurácia é de 85,24%. Para resolver esse problema, é necessário ajustar os hiperparâmetros do modelo. Apesar da possibilidade dos ajustes serem feitos manualmente, foram utilizados modelos autônomos para encontrar os melhores valores.

Antes de se utilizar os modelos, necessitou-se definir quais os hiperparâmetros seriam otimizados. Os parâmetros foram selecionados de acordo com um [artigo escrito por Tavish Srivastava](https://www.analyticsvidhya.com/blog/2015/06/tuning-random-forest-model/) e também com a [documentação do modelo](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html).

Testes conduzidos com o número de estimadores definidos e os métodos [RandomizedSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RandomizedSearchCV.html#sklearn.model_selection.RandomizedSearchCV) e [GridSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html#sklearn.model_selection.GridSearchCV) não mostrara resultados que trouxeram melhora significativa ao modelo. Além disso, o tempo de execução também foi maior para esses modelos. Por isso, foi utilizado o método experimental [HalvingGridSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.HalvingGridSearchCV.html#sklearn.model_selection.HalvingGridSearchCV).


### HalvingGridSearchCV

Como o método está em fase experimental, é notado que o método pode sofrer alterações mais rapidamente. No entanto, em testes ele se mostrou mais rápido que o método GridSearchCV, tendo uma eficácia parecida.


```python
from sklearn.experimental import enable_halving_search_cv 
from sklearn.model_selection import HalvingGridSearchCV
import numpy as np

SEED = 42

modelo = RandomForestClassifier(random_state = SEED)

espaco_de_parametros = {
    'max_features' : ['auto', 'sqrt', 'log2'],
    'n_estimators' : np.arange(50,400,step=50),
    'min_samples_leaf' : list(np.arange(2,516,step=32)),
    'max_depth' : np.arange(2,21,step=1)
}

busca = HalvingGridSearchCV(modelo,
                     param_grid = espaco_de_parametros,
                     cv = StratifiedKFold(n_splits = 10),
                     n_jobs = -1,
                     factor = 3,
                     random_state = SEED,
                     scoring = 'accuracy')

busca.fit(X, y)
```

## Usando o modelo com dados faltantes

Na primeira semana do projeto, foi percebido que alguns clientes não possuíam valor para a coluna `Churn`, que é a variável target do sistema. Esses dados foram separados, com o intuito de, após o modelo de machine learning estar calibrado e validado, fosse possível prever essas informações e entregar a empresa. Com essas informações, a empresa poderá entrar em contato com os clientes que o modelo aponta que irão cancelar, para evitar que isso aconteça.

Para incluir as informações no modelo, os dados passaram pelos mesmos processos de *encoding* e normalização dos dados, além também de remoção de colunas indejesadas. Após isso, os dados foram incluidos no modelo, que trouxe a informação dos clientes que poderão cancelar o contrato. Essa informação foi então salva em um arquivo csv e será encaminhada a empresa.






