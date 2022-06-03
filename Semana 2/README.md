# [Semana 2](https://colab.research.google.com/github/vinicius-pf/Challenge_DataScience/blob/Semana-2/Semana%202/%20Analises_Graficas.ipynb)

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

<img src = "https://user-images.githubusercontent.com/6025360/169864616-527ea57e-c3e9-4d73-b850-7e478ea63c3a.png" width = 500, height = 500/>

Para a coluna `Tempo_Contrato`, oberva-se que as pessoas que evadiram possuem uma média menor de tempo de contrato, se comparado as pessoas que ficaram na empresa. Com essa informação, foi verificado também a distribuição de evasão de acordo com o tempo de contrato. Ao fazer isso, foi possível entender que para clientes com tempo de contrato menor que 10 meses, há uma taxa de evasão maior do que o resto do dataset. Outro ponto importante foi visualizado: entre os clientes com 1 e 2 meses de contrato, há mais clientes que evadiram a empresa do que estão no momento nela.

<img src = "https://user-images.githubusercontent.com/6025360/169865056-2ec71e1a-985e-4f64-be7e-64d00ac9d387.png" width = 500, height = 500/>

A variável `Servico_Internet` é outra coluna que apresentou distribuição atípica. Os clientes que assinaram o serviço de `Fibra Óptica` da empresa possuem uma taxa de evasão maior que a dos clientes totais da empresa. Para entender melhor esse comportamento foram feitas análises subsequentes.

<img src = "https://user-images.githubusercontent.com/6025360/169865140-16f1a3fd-3628-4384-b743-4c22be852ff2.png" width = 500, height = 500/>

Para a coluna `Servico_Telefone`, a análise não mostrou diferença de taxa de evasão em comparação com a taxa de todos os clientes. A maioria dos clientes possue serviço de telefonia contratado com a empresa.

<img src = "https://user-images.githubusercontent.com/6025360/169865211-4d1f7887-88ca-4ec9-901b-acf15639b036.png" width = 500, height = 500/>

Durante a análise da coluna `Tipo_Contrato`, um comportamento parecido com a coluna `Servico_Internet` foi percebido. Clientes que optam por um plano mensal possuem taxa de evasão elevada. No entanto, foi percebido um comportamento diferente nas outras possibilidades também. Clientes com plano anual possuem uma taxa de evasão menor do que aqueles com plano mensal. Já clientes com plano bianual possuem a menor taxa de evasão: apenas 3% dos clientes com plano bianual evadiram da empresa.

<img src = "https://user-images.githubusercontent.com/6025360/169865280-be3224d5-a9a6-446e-8385-9ef5ed6f12db.png" width = 500, height = 500/>

Na coluna `Conta digital`, se percebe uma taxa maior de evasão para clientes que possuem conta digital, enquanto clientes que não possuem o serviço aparentam uma taxa de evasão menor. 

<img src = "https://user-images.githubusercontent.com/6025360/169865352-7b3a4a47-dc27-4ab9-8f81-3c4ea09bc96b.png" width = 500, height = 500/>

Por último, na coluna `Metodo_Pagamento`, aqueles que optam por pagamento via `Cheque eletrônico` possuem uma taxa de evasão elevada em comparação aos outros clientes. Os outros métodos de pagamento possuem taxa de evasão parecida e menor que a taxa de evasão global.

<img src = "https://user-images.githubusercontent.com/6025360/169865418-1e03e5de-27ce-4e09-93f1-ba4653404877.png" width = 500, height = 500/>

#### Análises extras

Além das análises com a variável target, também foi efetuada uma análise dos clientes que optaram por `Fibra óptica`. Por se tratar de uma tecnologia de mais qualidade do que o outro serviço de internet, se esperava uma taxa de evasão menor por parte desses clientes.

Para essa análise, foi criado um dataset temporário, apenas com os clientes que optaram pelo serviço. Depois, foi calculada novamente a correlação entre as variáveis.
A correlação não apresentou valores para duas variáveis: `Servico_Internet` e `Servico_Telefone`. Esse comportamento se deu pela quantidade de valores únicos nas categorias: apenas 1. A coluna `Servico_Internet` possuía apenas o valor do filtro, o que era esperado. Já a conta `Servico_Telefone` possuía apenas o valor `Sim`. Isso significa que todos os clientes que possuem serviço de fibra óptica também possuem serviço de telefone.

Com a correlação calculada, se selecionou as colunas que possuíam correlação forte com a variável target. Por isso as colunas `Tipo_Contrato`, `Tempo_Contrato` e `Valor_Total` foram escolhidas.

Para a coluna `Tipo_Contrato`, se percebe que a maioria dos clientes desse plano possue contrato mensal. Há também mais clientes evadidos nessa categoria do que clientes atualmente na empresa.

<img src = "https://user-images.githubusercontent.com/6025360/169868543-89d1e284-494b-453e-9f26-192c90b4e952.png" width = 500, height = 500/>

O mesmo comportamento visto na variável `Tempo_Contrato` foi percebido quando aplicado apenas para clientes de fibra óptica, porém mais acentuado. Clientes com menos de 10 meses de contrato tendem mais a cancelar o contrato do que se manter na empresa.

<img src = "https://user-images.githubusercontent.com/6025360/169868506-889e1188-7f16-4a0c-8592-41369993c8d2.png" width = 500, height = 500/>

Por conta da característica `Tempo_Contrato`, a variável `Valor_Total` mostra que clientes que evadiram a empresa pagam um valor menor do que aqueles que se mantém na empresa.

<img src = "https://user-images.githubusercontent.com/6025360/169868454-ba12ec32-0425-4297-aa9f-feabaa765d59.png" width = 500, height = 500/>

#### Concluindo

Com as análises completas, pode-se passar para a próxima etapa do projeto: criação, calibração, treinamento e validação de um modelo de machine learning, utilizando esses dados para se encontrar potenciais clientes evadidos da empresa.




