# Titanic 🚢
<p>
  O naufrágio do RMS Titanic é um dos eventos mais trágicos e marcantes da história marítima. Em 15 de abril de 1912, o Titanic colidiu com um iceberg e afundou, resultando na perda de mais de 1.500 vidas. Esse desastre marcou um ponto de virada na história da navegação e desencadeou mudanças significativas em normas de segurança marítima. Além do aspecto histórico, o Titanic também desempenha um papel importante no mundo da análise de dados.
</p>
<p>
  A disponibilidade de dados detalhados sobre os passageiros do Titanic, juntamente com as informações sobre sobrevivência e outros atributos, criou uma oportunidade única para a análise e o aprendizado de máquina. O dataset "Titanic: Machine Learning from Disaster" se tornou um dos conjuntos de dados mais populares e usados na comunidade de ciência de dados e aprendizado de máquina.
</p>
<p>
  A análise desse dataset permite a exploração de inúmeras questões interessantes relacionadas aos passageiros a bordo do Titanic. É possível investigar o perfil demográfico dos passageiros, como idade, sexo e classe socioeconômica, e como esses fatores podem estar relacionados à taxa de sobrevivência. Além disso, é possível analisar a distribuição de sobreviventes em diferentes categorias, como classe de passagem, local de embarque e acompanhamento familiar.
</p>
<p>
  Os dados do Titanic também proporcionam uma oportunidade para realizar análises de engenharia de recursos, tratando valores ausentes, criando novas variáveis e identificando padrões ocultos. A análise de recursos importantes pode ser empregada para identificar quais atributos têm maior influência na previsão de sobrevivência.
</p>
<p>
  Além disso, é possível utilizar técnicas de aprendizado de máquina para construir modelos preditivos que tentem prever a sobrevivência dos passageiros com base em suas características. A construção de modelos, ajuste de hiperparâmetros e validação cruzada são algumas das abordagens para obter previsões precisas.
</p>

## Obtenção de dados
Todos os dados necessários para a análise e construção do modelo foram retirados da [página do desafio do Titanic](https://www.kaggle.com/c/titanic/data) no site Kaggle. Os dados foram divididos em dois conjuntos:

- **Conjunto de treino (*train.csv*)**: composto por diversas informações sobre os passageiros, como gênero, classe de embarque, idade, etc. Esse conjunto também deve ser usado para construir e treinar o modelo de *Machine Learning*, nele é informado se o passageiro sobreviveu ou não.
- **Conjunto de teste (*test.csv*)**: este *dataset* não informa se o passageiro sobreviveu ou não, e deve ser usado como dados nunca vistos pelo modelo, de forma a verificar o desempenho do mesmo.

A plataforma do Kaggle também disponibiliza um gabarito (*gender_submission.csv*) informando os sobreviventes do conjunto de testes.

## Atributos do dataset
- **PassengerID**: Número de identificação do passageiro;
- **Survived**: Informa se o passageiro sobreviveu ao naufrágio (**0** = não e **1** = sim).
- **PCclass**: Classe do bilhete (**1** = 1ª classe; **2** = 2ª classe e **3** = 3ª classe);
- **Name**: Nome do passageiro;
- **Sex**: Sexo do passageiro;
- **Age**: Idade do passageiro;
- **SibSp**: Quantidade de cônjuges e/ou irmãos a bordo;
- **Parch**: Quantidade de pais e filhos a bordo;
- **Ticket**: Número da passagem;
- **Fare**: Preço da passagem;
- **Cabin**: Número da cabine do passageiro;
- **Embarked**: Porto de embarque: (C = Cherbourg; Q = Queenstown; S = Southampton);

O dataset é formado por 12 variáveis e 891 entradas, sendo elas 7 variáveis numéricas e 5 do tipo object, estas últimas são strings.

## Bibliotecas utilizadas

```Python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import math
import seaborn as sns
import plotly.express as px
```
- **Pandas**: Uma das bibliotecas mais utilizadas para análise de dados em Python. Apresenta estruturas de dados em forma de DataFrames e Series, as quais são extremamente eficazes para manipulação e análise de dados tabulares. Pandas proporciona operações prontamente realizáveis, como leitura de arquivos CSV, filtragem, agregação e outras. 

- **Numpy**: Uma das bibliotecas mais essenciais para computação numérica em Python. NumPy apresenta suporte a arrays multidimensionais (ndarrays) e ao mesmo tempo fornece um conjunto bastante extenso de funções matemáticas que facilitam as operações sobre arrays, permitindo grandes eficiências, servindo de base para muitas outras bibliotecas de ciência de dados. 

- **Matplotlib**: Uma das bibliotecas mais conhecidas para visualização de dados em Python. Matplotlib possibilita a geração de gráficos estáticos, animados e interativos em diversas formas (linhas, barras, histogramas, entre outros). A interface pyplot é particularmente utilizada para gerar gráficos de forma simples e intuitiva. 

- **Math**: Um dos módulos padrões do Python que fornece as funções matemáticas básicas (trigonométricas, logaritmos, fatoriais e outras operações matemáticas frequentes). 

- **Seaborn**: Uma biblioteca baseada em Matplotlib que simplifica a criação de gráficos estatísticos com uma estética mais bonita e integrada. Seaborn fornece facilidade em criação de gráficos mais sofisticados (tais como correlações, distribuições e gráficos de regressão), fornece temas e paletas de cores. 

- **Plotly express**: Uma biblioteca de visualização interativa que fornece uma sintaxe simples para a criação de gráficos complexos e um alto nível de interatividade. Plotly Express é conhecida por sua facilidade de uso e integração com ferramentas web, permitindo a criação de dashboards e gráficos interativos com poucos comandos. 

## Limpeza dos dados
O próximo passo, antes de iniciar nossa análise, é a limpeza dos dados. Ou seja, vamos percorrer a base procurando por dados faltantes e ou outliers

Alguns passos importantes:

1. Remover dados duplicados
2. Remover outliers (são dados discrepantes em relação às outras informações da base)
3. Lidar com dados ausentes

Para tratar os valores ausentes, aplicamos as seguintes estratégias:

- Para a coluna 'Age', preenchi os valores ausentes com a mediana da idade usando a função fillna().
- Para a coluna 'Embarked', preenchi os valores ausentes com a moda (valor mais frequente) usando a função fillna().
- Devido a muitos valores ausentes na coluna 'Cabin', decidi removê-la usando a função drop() com axis=1 para indicar que queremos remover uma coluna.

O método drop_duplicates é uma função do Pandas usada para remover linhas duplicadas em um DataFrame. Ele identifica e remove duplicatas, mantendo apenas a primeira ocorrência de cada linha
```Python
df = df.drop_duplicates()
```
A função remove_outliers_iqr, remove outliers de um conjunto de dados com base no método do Intervalo Interquartil (IQR). Recebe como entrada o dataframe, o nome da coluna e um valor multipliativo que determina a sensibilidade ao identificar outliers.
```Python
def remove_outliers_iqr(data, column, threshold=1.5):
    q1 = data[column].quantile(0.25)
    q3 = data[column].quantile(0.75)
    iqr = q3 - q1
    lower_bound = q1 - threshold * iqr
    upper_bound = q3 + threshold * iqr
    return data[(data[column] >= lower_bound) & (data[column] <= upper_bound)]
```
```Python
df = remove_outliers_iqr(df, 'Pclass')
df = remove_outliers_iqr(df, 'Survived')
```
Tratando os valores ausentes e removendo a coluna 'Cabin'.
```Python
# Preenchendo valores ausentes na coluna 'Age' com a mediana
median_age = df['Age'].median()
df['Age'].fillna(median_age, inplace=True)

# Preenchendo valores ausentes na coluna 'Embarked' com a moda
mode_embarked = df['Embarked'].mode()[0]
df['Embarked'].fillna(mode_embarked, inplace=True)

# Removendo coluna 'Cabin' por conter muitos valores ausentes
df.drop('Cabin', axis=1, inplace=True)
```
## Análise de dados
### Contagem de sobreviventes
![contagem de sobreviventes](https://i.imgur.com/7PM8Ywa.png)
### Relação de classe e sobrevivência
![classe x sobrevivência](https://i.imgur.com/gMSCEjm.png)
### Distribuição de gênero
![distribuição de gênero](https://i.imgur.com/8UoyfC9.png)
### Idade dos passageiros
![Idades](https://i.imgur.com/LGQ5HQC.png)
### Distribuição de sobreviventes por classe e gênero
![distribuição de gênero x classe x sobrevivência](https://i.imgur.com/mdwFXRc.png)
### Matriz de correlação
![Matriz de correlação](https://i.imgur.com/QkQ4ZDp.png)

## Conclusões da análise
Os gráficos demonstram claramente uma relação entre a sobrevivência dos passageiros, seu gênero, classe social e idade. De acordo com o filme que retrata a tragédia, os passageiros das classes mais altas tiveram prioridade para embarcar nos botes de resgate, com alguns desses botes sequer atingindo a capacidade máxima. Além disso, os dados confirmam que houve prioridade para mulheres e crianças no processo de evacuação.
Com esses dados, é possível criar um programa que diga se o usuário sobreviveria ou não, caso estivesse presente no navio.

## Titanic Predict
Através da biblioteca sklearn, foi possível criar um modelo de Decision Tree. A "Árvore de decisão" é um método gráfico comum usado para modelagem preditiva. Uma árvore de decisão assemelha-se a um fluxograma. Um nó é uma “pergunta” sobre um atributo. Representa uma condição e se divide em dois ramos. O nó de divisão com base na condição de um atributo é indicado por um arco e os dois ramos resultantes são os ramos subsequentes. O entroncamento dos dois ramos na árvore representa uma combinação de todos os atributos, o nó final é o resultado sobre um atributo. As árvores de decisão são frequentemente usadas para problemas onde a explicabilidade do modelo é fundamental e são fáceis de explicar, o que nos ajudou a analisar melhor o desempenho do modelo. Portanto, as árvores de decisão podem ser úteis para processar dados de várias maneiras.
