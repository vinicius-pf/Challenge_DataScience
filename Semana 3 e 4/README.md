# [Semana 1](https://bit.ly/Semana1_Alura) - Alura Challenge BI

## Dashboard Operacional

A Alura Log, uma empresa de logística, está enfrentando alguns problemas por conta do aumento de demanda durante a pandemia da COVID-19. Para manter a qualidade do serviço, percebeu que precisava acompanhar algumas métricas para conseguir efetuar algumas decisões operacionais. O dashboard aqui produzido trás as métricas pedidas, assim como algumas extras, desenvolvidas com o intuito de melhorar a qualidade do serviço da empresa. 

## Base de Dados

A empresa disponibilizou 4 tabelas no formato CSV:
  
  1. Tabela pedidos, que contém o registro de todos os pedidos feitos pelos clientes.
  2. Tabela produtos, que contém os produtos cadastrados e seus valores.
  3. Tabela veículos, que contém veículos registrados que fazem o transporte dos produtos
  4. Tabela estoque, que contém o registro de estoque dos produtos por mês

### Relacionamentos

Há 3 relacionamentos entre as tabelas:
  
1. A 'Tabela pedidos' se relaciona com a 'Tabela produtos' pela coluna 'ID Produtos', com uma cardinalidade de muitos para 1 (n:1)
2. A 'Tabela estoque' se relaciona com a 'Tabela produtos' pela coluna 'ID Produtos', com uma cardinalidade de muitos para 1 (n:1)
3. A 'Tabela pedidos' se relaciona com a 'Tabela veículos' pela coluna 'ID Veículo', com uma cardinalidade de muitos para 1 (n:1)


## Métricas

A empresa requisitou que as seguintes métricas estivessem no relatório:
  
  - S2D(Ship to door): Tempo, em dias, que a entrega demorou a ser efetuada
  - Número de entregas por estado
  - Número de veículos disponíveis
  - Nível de estoque por ano
  - Número de entregas atrasadas e no prazo

Além destas, também foram desenvolvidas algumas métricas extras:
  - Faturamento total, por ano e por estado
  - S2D médio por estado

  
## Ferramentas utilizadas
  Para a criação do dashboard foi utilizado o software Power BI. As imagens de fundo das páginas foram criadas no Microsoft Power Point.


