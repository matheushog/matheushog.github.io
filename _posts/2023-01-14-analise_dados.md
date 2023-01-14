---
title: Análise de dados com python
date: 2023-01-14 12:00 -03:00
categories: [data]
tags: [analysis,python,learning]
---
# Análise de dados com python

A carreira de analista de dados ou engenheiro de dados, possui grande demanda no mercado nacional e internacional. Uma pesquisa de 2021 do site [Canaltech](https://canaltech.com.br/startup/demanda-por-profissionais-de-dados-vai-a-quase-500-salarios-chegam-a-r-22-mil-192808/), mostrou uma demanda de mais de **420 mil** profissionais na área entre os anos de 2019 e 2024, sendo que no Brasil são formados **42 mil** profissionais ano.

Estes fatos me motivaram a buscar informações e assistar a algumas aulas disponíveis em plataformas como o youtube. Há muita informação disponível, de forma gratuita, a respeito do tema em canais brasileiros e internacionais. Em todos os videos que assisti, todos sempre orientaram a buscar algum tema interessante, buscar um banco de dados representativo do tema escolhido e "por a mão na massa".

Escolhi um tema que no momento de minha escolha, não sabiamos nada a respeito e que, a meu ver, seria essencial juntar o máximo de informações possíveis. O tema que escolhi foi a **pandemia de Covid-19**.

Em pleno lockdown de 2020, comecei a buscar por fontes de informações que fossem confiáveis. Acabei me focando em fontes de origens internacionais como a [Organização Mundial de Saúde](https://www.who.int/) e a da [Universidade Johns Hopkins](https://www.jhu.edu/). Atualmente temos projetos envolvendo diversos pesquisadores brasileiros de diferentes áreas e instituições, como o [Observatório Covid-19](https://covid19br.github.io/), que realizam o acompanhamento da doença.

---
## Bibliotecas utilizadas

```python
import pandas as pd
import seaborn as sns
import plotly
import plotly.graph_objs as go
import plotly.express as px
import numpy as np
import matplotlib.pyplot as plt
pd.options.plotting.backend = 'plotly'
```

Para melhor interação com o código escrito, utilizei a extensão [Jupyter Notebook](https://jupyter.org/) do IDE [Visual Studio Code](https://code.visualstudio.com/).

A primeira biblioteca importada, `pandas`, nos auxilia no manuseio dos bancos de dados que serão analisados (link para a [documentação](https://pandas.pydata.org/docs/)). 

As bibliotecas `seaborn`, `matplotlib` e `plotly` são voltadas à visualização da análise de dados realizada. 

Caso durante a execução do código seja mostrado algum *warning* acusando a falta de alguma biblioteca necessária à execução do código, podemos utilizar um `!pip install` para resolver esta demanda.

---
## Importando o banco de dados

```python
df = pd.read_csv("https://covid19.who.int/WHO-COVID-19-global-data.csv")
```

Aqui importou-se o banco de dados, no formato `.csv` e com auxilio da bilbioteca `pandas`, da Organização Mundial de Saúde.

---
## Conferindo o banco de dados

```python
country = "Brazil"

br = df[df['Country'] == country]
br.reset_index(inplace=True)
br.tail()   # Mostra as 5 últimas linhas
```

Após análise da estrutura geral do banco de dados, referenciado como `df` ou **d**ata**f**rame, filtrou-se apenas os valores correspondentes ao país escolhido, **Brasil**. Uma imagem da forma como o *dataframe* é mostrado no `jupyter notebook`, é apresentado abaixo:

![dataframe WHO](/assets/images/df.png)

---
## Data da primeira aplicação da vacina

```python
dfv = pd.read_csv("https://covid19.who.int/who-data/vaccination-data.csv")  # df com as datas
y = dfv[dfv['ISO3'] == "BRA"]["FIRST_VACCINE_DATE"]
y = y[29]
x = pd.to_datetime(y)
```

Este código nos retorna `x` com a data da aplicação da primeira vacina no Brasil, <mark>17/01/2021</mark>, de acordo com o banco de dados da Organização Mundial de Saúde.

---
## Plotando a análise de dados

Utilizando as informações presentes no *dataframe*, e nos baseando na primeira aplicação da vacina, escreveu-se o seguinte código utilizando a biblioteca `plotly`:

```python
br['new_deaths_smoothed'] = br["New_deaths"].rolling(7).mean() # média móvel semanal

fig = go.Figure([
      go.Bar(name='Mortes', x=br['Date_reported'], y=br['New_deaths']),
      go.Scatter(name='Média Móvel', x=br['Date_reported'], y=br['new_deaths_smoothed'], mode='lines', showlegend=True),
])

fig.add_shape(name='Inicio da vacinação', type="line", # adiciona a data da 1ª vacina no Brasil
    x0=x, y0=0, x1=x, y1=br['New_deaths'].max(),
    line=dict(color="Green", width=1, dash="dashdot")
)

fig.update_layout(
    yaxis_title='Número de novos obitos',
    xaxis_tickformat = '%B %Y',
    hovermode="x"
)

fig.show()
```

Gerando o seguinte gráfico:

![dataframe plot](/assets/images/df_plot.png)

---
## Análise de dados: por ano

```python
br['Date_reported'] = pd.to_datetime(br['Date_reported'])

fig = px.scatter(br, x="New_cases", y="New_deaths", 
                color=br['Date_reported'].dt.month,
                labels={"color": "Mês"},
                trendline="ols",
                facet_col=br['Date_reported'].dt.year,)
fig.show()
```

O gráfico a seguir nos auxilia a visualizar a evolução dos **casos x óbitos** da doença. As cores presentes nos gráficos, representam os meses em que os casos foram reportados:

![dataframe plot ano](/assets/images/df_plot_ano.png)

---
## Análise de dados: variação de óbitos

```python
fig = px.box(br, x=br['Date_reported'].dt.year, y="New_deaths")
fig.show()
```

O gráfico a seguir, representa a a variação do número de óbitos reportados por ano:

![dataframe plot ano](/assets/images/df_plot_obitos.png)

---
## Análise de dados: variação de casos

```python
fig = px.box(br, x=br['Date_reported'].dt.year, y="New_cases")
fig.show()
```

O gráfico a seguir, representa a a variação do número de casos reportados por ano:

![dataframe plot ano](/assets/images/df_plot_casos.png)

---

## Próximos passos

Como relatado no começo deste *post*, existem análises de dados realizadas por um grande número de pesquisadores, nacionais e internacionais. Esta análise realizada e apresentada neste site, foi uma primeira tentativa de aprender, entre o básico e intermediário, dos principais tópicos de análise de dados.

Existem alguns pontos que ainda preciso aprimorar como: 
- melhorar os gráficos apresentados (adicionando legendas e labels dos eixos x e y);
- aprender outros tipos de gráficos;
- aprender a manipular banco de dados com a biblioteca `pandas`;
- aprender a criar um dashboard com as principais informações, em *python*.

Agradeço aos diversos *posts* no fórum [stack**overflow**](https://stackoverflow.com/).

Até a próxima,

Matheus.