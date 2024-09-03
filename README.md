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
