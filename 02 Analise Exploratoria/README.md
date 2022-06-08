# [Semana 2](https://colab.research.google.com/github/vinicius-pf/Challenge_DataScience/blob/Correções/02%20Analise%20Exploratoria/Analises%20Exploratorias.ipynb)

## Correção de erros

Durante a limpeza de dados, foi decidido separar os dados que estavam em branco na coluna `Churn`. Isso foi feito, porém não da maneira mais correta. Apesar de não aparecerem valores na coluna, os valores estão presentes como espaço em branco. Para análises futuras, pode ser necessário alterar para valores nulos.

Outro equivoco ocorreu no tratamento de dados faltantes da coluna `Charges.Total`. Em um primeiro momento, os valores faltantes foram substituidos por `0`. A empresa, no entanto, requisitou que os valores incluidos fossem o mesmo da coluna `Charges.Monthly`.

Após a correção desses problemas, a exploração dos dados com análises gráficas poderá seguir com mais clareza.

## Visualizando a distribuição da variável target

Para visualizar a variável target, foram utilizados dois tipos de gráfico: um histograma e um gráfico de pizza.

O histograma é um bom gráfico para comparar distribuições e contagens entre valores das variáveis. Em variáveis numéricas, ele ajuda a entender a distribuição da variável e comparar alguns valores. Como esta é uma variável categórica que recebe apenas dois valores, o gráfico foi uma solução simples para a comparação das contagens e percentual dos valores.

<img src= "https://user-images.githubusercontent.com/6025360/172622348-f1a1e259-1b17-4450-ad8a-5828cd3b9102.png" width = 500, height = 500/>

Com o histograma percebe-se que há mais clientes que não evadiram do que clientes evadidos. Para entender melhor a taxa de evasão, pode ser utilizado um gráfico de pizza.

<img src= "https://user-images.githubusercontent.com/6025360/172622431-43ab07fc-236b-48fc-81ae-f7d833657558.png" width = 500, height = 500/>

Com esses gráficos, é percebido que 26,5% dos clientes da empresa cancelaram os planos contratados. Apesar da empresa não apresentar metas para a taxa de evasão, a taxa se mostra alta em relação à outros casos.

Com a distribuição verificada, pode-se iniciar a próxima etapa e comparar como as outras variáveis do sistema se distribuiem em relação a variável target. 

## Analisando a correlação entre as variáveis

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

## Analisando a variável Churn em conjunto com outras variáveis

Para as análises extras da variável target, foram selecionadas algumas colunas que possuem relação forte com a mesma. Para isso, foram criadas visualizações de distribuição e frequência nas colunas `Valor_Mensal`, `Tempo_Contrato`, `Servico_Internet`, `Servico_Telefone`, `Tipo_Contrato`, `Conta_Digital` e `Metodo_Pagamento`.

Analisando a coluna `Valor_Mensal`, se percebe que a média de gastos mensais dos clientes que evadiram a empresa é maior do que aquelas que não cancelaram o contrato. Isso pode ser uma explicação do motivo do cancelamento, com os clientes ficando descontentes com o valor pago e migrando para outra empresa.

<img src = "https://user-images.githubusercontent.com/6025360/172623466-daf3c403-433f-42da-a68d-dab2e5701ca5.png" width = 500, height = 500/>

Para a coluna `Tempo_Contrato`, oberva-se que as pessoas que evadiram possuem uma média menor de tempo de contrato, se comparado as pessoas que ficaram na empresa. Com essa informação, foi verificado também a distribuição de evasão de acordo com o tempo de contrato. Ao fazer isso, foi possível entender que para clientes com tempo de contrato menor que 10 meses, há uma taxa de evasão maior do que o resto do dataset. Entre os clientes com 1 e 2 meses de contrato, há mais clientes que evadiram a empresa do que estão no momento nela.

<img src = "https://user-images.githubusercontent.com/6025360/172622824-d097eba4-2d5b-4f6a-82eb-51da3dccc56d.png" width = 500, height = 500/>

A variável `Servico_Internet` é outra coluna que apresentou distribuição atípica. Os clientes que assinaram o serviço de `Fibra Óptica` da empresa possuem uma taxa de evasão maior que a dos clientes totais da empresa. Para entender melhor esse comportamento foram feitas análises subsequentes.

<img src = "https://user-images.githubusercontent.com/6025360/172622943-9f54d9f6-6606-40d3-9005-ce001d45e445.png" width = 500, height = 500/>

Para a coluna `Servico_Telefone`, a análise não mostrou diferença de taxa de evasão em comparação com a taxa de todos os clientes. A maioria dos clientes possue serviço de telefonia contratado com a empresa.

<img src = "https://user-images.githubusercontent.com/6025360/172622995-c5fb1003-3051-4ac0-918b-2b40175d5efe.png" width = 500, height = 500/>

Durante a análise da coluna `Tipo_Contrato`, um comportamento parecido com a coluna `Servico_Internet` foi percebido. Clientes que optam por um plano mensal possuem taxa de evasão elevada. No entanto, foi percebido um comportamento diferente nas outras possibilidades também. Clientes com plano anual possuem uma taxa de evasão menor do que aqueles com plano mensal. Já clientes com plano bianual possuem a menor taxa de evasão, onde apenas 3% dos clientes com plano bianual evadiram da empresa.

<img src = "https://user-images.githubusercontent.com/6025360/172623052-a2fbf32e-63fc-4ac1-bb83-4a05abb875b1.png" width = 500, height = 500/>

Na coluna `Conta_Digital`, se percebe uma taxa maior de evasão para clientes que possuem conta digital, enquanto clientes que não possuem o serviço aparentam uma taxa de evasão menor. 

<img src = "https://user-images.githubusercontent.com/6025360/172623597-18c2f62c-0cb0-4e86-810e-e5b250d164c4.png" width = 500, height = 500/>

Por último, na coluna `Metodo_Pagamento`, aqueles que optam por pagamento via `Cheque eletrônico` possuem uma taxa de evasão elevada em comparação aos outros clientes. Os outros métodos de pagamento possuem taxa de evasão parecida e menor que a taxa de evasão global.

<img src = "https://user-images.githubusercontent.com/6025360/172623741-146621a1-ef4b-44f2-8c1b-cba2b9321628.png" width = 500, height = 500/>

## Análises extras

Além das análises com a variável target, também foi efetuada uma análise dos clientes que optaram por `Fibra óptica`. Por se tratar de uma tecnologia de mais qualidade do que o outro serviço de internet, se esperava uma taxa de evasão menor por parte desses clientes.

Para essa análise, foi criado um dataset temporário, apenas com os clientes que optaram pelo serviço. Depois, foi calculada novamente a correlação entre as variáveis.
A correlação não apresentou valores para duas variáveis: `Servico_Internet` e `Servico_Telefone`. Esse comportamento aconteceu pois apenas. A coluna `Servico_Internet` possuía apenas o valor do filtro, o que era esperado. Já a conta `Servico_Telefone` possuía apenas o valor `Sim`. Isso significa que todos os clientes que possuem serviço de fibra óptica também possuem serviço de telefone.

Com a correlação calculada, se selecionou as colunas que possuíam correlação forte com a variável target. Por isso as colunas `Tipo_Contrato`, `Tempo_Contrato` e `Valor_Total` foram escolhidas.

Para a coluna `Tipo_Contrato`, se percebe que a maioria dos clientes desse plano possue contrato mensal. Há também mais clientes evadidos nessa categoria do que clientes atualmente na empresa.

<img src = "https://user-images.githubusercontent.com/6025360/172623225-a44912f7-9424-47b4-bb34-dcf908a5ec9d.png" width = 500, height = 500/>

O mesmo comportamento visto na variável `Tempo_Contrato` foi percebido quando aplicado apenas para clientes de fibra óptica, porém mais acentuado. Clientes com menos de 10 meses de contrato tendem mais a cancelar o contrato do que se manter na empresa.

<img src = "https://user-images.githubusercontent.com/6025360/172623281-3b47a7a4-86a2-423d-ac4a-be5212559180.png" width = 500, height = 500/>

Por conta da característica `Tempo_Contrato`, a variável `Valor_Total` mostra que clientes que evadiram a empresa pagam um valor menor do que aqueles que se mantém na empresa.

<img src = "https://user-images.githubusercontent.com/6025360/172623332-641b50ae-ee66-4b5c-93fd-aa8ccf6ac95c.png" width = 500, height = 500/>

## Concluindo

Com as análises concluidas, se percebeu uma necessidade da empresa de focar em clientes que possuem menor tempo de contrato, tendo em vista a alta taxa de evasão dos clientes. Também é necessário que a empresa melhore seu serviço de fibra óptica, seja melhorando o sistema ou diminuindo o preço, para que menos clientes possam vir a cancelar o contrato.

Pode-se passar agora para a próxima etapa do projeto,em que ocorrerá a criação, calibração e validação de um modelo de machine learning que informe a empresa os clientes que possam vir a cancelar o contrato.
