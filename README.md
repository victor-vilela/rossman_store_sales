# Rossmann Store Sales Prediction

## 1. The Company

A Rossmann é uma grande rede de farmácia alemã presente em mais 7 países da europa: Albânia, Kosovo, Polônia, Espanha, República Tcheca, Turquia e Hungria.<br>
Fundado por Dirk Roßmann em 1972 na cidade de Hannover, hoje a Rossmann é uma das maiores redes de farmácias tendo mais de 4000 filiais espalhados pela Europa.

Todo contexto de negócio envolvendo este projeto é fictício.<br>

### 1.1. Business Problem

Durante uma reunião mensal de resultados da companhia, o CFO da Rossmann solicitou uma Previsão de Vendas das próximas 6 semanas de cada loja para definição de budget para a reforma das lojas.

Hoje o processo de predição de vendas é baseado em experiências passadas, além de serem feitas manualmente nas 1115 lojas da Rossmann.
Dessa forma, a apresentação contém muita divergência e essa vizualização é limitada ao computador.

### 1.2. Proposed Solution

O CFO terá a visualização através do Smartphone do faturamento nas próximas 6 semanas de cada loja.
Nesse caso, será utilizado um algoritmo de machine learning para realizar essa Previsão de Vendas.

## 2. Database

As variáveis do dataset original são:

| Feature | Description |
| ------------ | ------------- |
| Id | Id que representa uma duplicata (Store, Date) dentro do conjunto de teste |
| Store | ID exclusivo para cada loja |
| Sales | o volume de negócios para um determinado dia |
| Customers | o número de clientes em um determinado dia |
| Open | indicador para saber se a loja estava aberta: 0 = fechado, 1 = aberto |
| StateHoliday | indica um feriado estadual. a = public holiday, b = easter holiday, c = christmas, 0 = none |
| SchoolHoliday | indica se a Loja foi afetada pelo fechamento das escolas públicas |
| StoreType | diferencia 4 modelos de loja diferentes: a, b, c, d |
| Assortment | descreve um nível de sortimento: a = basic, b = extra, c = extended |
| CompetitionDistance | distância em metros até a loja concorrente mais próxima |
| CompetitionOpenSince[Month/Year] | fornece o ano e o mês aproximados da hora em que o concorrente mais próximo foi aberto |
| Promo | Indica se uma loja está realizando uma promoção naquele dia |
| Promo2 | Promo2 é uma promoção contínua e consecutiva para algumas lojas: 0 = loja não está participando, 1 = loja está participando |
| Promo2Since[Year/Week] | descreve o ano e a semana do calendário em que a loja começou a participar do Promo2 |
| PromoInterval | Descreve os intervalos consecutivos em que o Promo2 é iniciado, nomeando os meses em que a promoção é reiniciada. Por exemplo. "Fevereiro, maio, agosto, novembro" significa que cada rodada começa em fevereiro, maio, agosto, novembro de qualquer ano para essa loja |

## 3. Solution Strategy

### 3.1 Final Product 

O que será entregue?

- Um bot via Telegram que a partir do código da loja informado, será apresentado o valor do faturamento nas próximas 6 semanas.

### 3.2 Tools

- Python 3.8.3
- API Telegram
- Heroku
- LunarVim
- Jupyter Notebook ( Análise e prototipagens )
- Git e Github;
- Flask

### 3.3 Process

A resolução desse desafio é baseada na metodologia CRISP-DS <!-- (medium) -->

1. Realizar o entendimento do negócio junto ao time financeiro e de vendas, com o objetivo de compreender melhor o problema identificado.
2. Carrego o dataset para exploração de suas dimensões, tipos de dados, se há dados nulos, outliers e finalizo com a análise da estatística descritiva.
3. Criação de um mapa mental de hipóteses a fim de gerar insights para a EDA.
4. Desenvolvo novas features e filtro os dados com o objetivo de chegar na resposta das hipóteses de forma assertiva.
5. Inicio a Análise Exploratória dos Dados (EDA) passando por cada hipótese, chegando a conclusão se a hipótese é verdadeira ou falsa.
6. Começo a preparação dos dados para o treinamento do modelo. Nessa etapa são realizadas as transformações das features e o reescalonamento.
7. O dataset é divido em treino e teste para a utilização do Boruta, que é um método de seleção de variáveis, para definir quais features serão relevantes para o modelo.
8. No treinamento do modelo, avalio a performance de 5 modelos diferentes (lineares e não lineares):
  * Average Model
  * Linear Regression Model
  * Linear Regression Regularizes Model
  * Random Forest Regressor
  * XGBoost Regressor
9. Executo o Cross Validation para avaliar melhor o desempenho de cada modelo.
10. Nesse primeiro ciclo foi decidido seguir com o XGBoost para a exploração deste modelo.
11. Ajusto os parâmetros do modelo para aumentar sua performance utilizando técnicas de Fine Tuning 
12. Traduzo o resultado do modelo em resultado financeiro para a empresa.
13. Publico em produção o modelo via bot do Telegram para que os stakeholders tenham acesso aos faturamentos de cada loja.
14. Levanto os pontos de melhoria para serem colocados no roadmap da solução no próximo ciclo do CRISP-DS.

### 4. Top 3 Data Insights

**Hipótese 01:**

Lojas deveriam vender mais depois do dia 10 de cada mês.<br>
**True** - Lojas vendem mais depois do dia 10 de cada mês<br>

**Hipótese 02:**

Lojas com competidores mais próximos deveriam vender menos<br>
**False** - Lojas com competidores mais próximos vende mais e não menos<br>

**Hipótese 03:**

Lojas com maior sortimento deveriam vender mais<br>
**False** - Lojas com maior sortimento vendem MENOS<br>

### 5. Business Results

O modelo teve a seguinte performance:
- MAE (Erro Médio Absoluto): 768,00
- MAPE (Erro Percentual Médio Absoluto): 0,11

O faturamento de cada loja pode haver um erro médio de $768,00, tanto para cima quanto para baixo.
Esse erro que representa um percentual de erro de 11%.<br>
A partir de uma amostra dos resultados, podemos avaliar qual seria o melhor e o pior cenário de avaliação do modelo:

| store	| prediction |	worst_scenario	|	best_scenario	|	MAE	|	MAPE	|
------------ | ------------- | ------------- | ------------- | ------------- | ------------- |
| 531	|	532	|	395439.031250	|	394346.954405	|	396531.108095	|	1092.076845	|	0.100532	|
| 1047	|	1048	|	260673.718750	|	259903.527687	|	261443.909813	|	770.191063	|	0.094101	|
| 425	|	426	|	187825.265625	|	187428.752349	|	188221.778901	|	396.513276	|	0.085576	|
| 356	|	357	|	274354.781250	|	273600.591058	|	275108.971442	|	754.190192	|	0.094413	|
| 1024	|	1025	|	268734.968750	|	268135.283190	|	269334.654310	|	599.685560	|	0.082744	|

**Impacto para o negócio**

Anteriormente o CFO e seus gerentes se baseavam apenas em experiências passadas, tendo que realizar manualmente suas análises.<br>
Após a publicação do bot no Telegram, o CFO pode consultar o faturamento da loja nas próximas 6 semanas e alocar recursos para a reforma das lojas.

### 6. Conclusions

O objetivo do desafio foi alcançado com sucesso. <br>
Agora o time de negócio consegue tomar decisões de forma orientada aos dados para alavancarem seus lucros.

A visualização do funcionamento do bot no Telegram pode ser encontrado aqui: https://youtu.be/CGhKVooWthI

### 7. Lessons Learned

- Cramers V -> Medida de Associação entre variáveis categóricas.
- Boruta -> Método de seleção de variáveis.
- Fine Tuning -> Ajuste de parâmetros do modelo.

### 8. Next Steps to Improve

- Melhorar a performance do modelo.
- Ajustar a mensagem no bot para apresentar o pior e melhor cenário.
- Implementar a previsão de clientes nas próximas 6 semanas para que o faturamento esteja mais ajustado para a realidade de cada loja.

### 9. References

- A base de dados foi extratida do [Kaggle](https://www.kaggle.com/competitions/rossmann-store-sales) 
- Este projeto é parte do curso "DS em Produção" da [Comunidade DS](https://www.comunidadedatascience.com/).
