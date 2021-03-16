# COVID-19: evitando um possível colapso nos sistemas de saúde
---
Este projeto foi desenvolvido durante o **Bootcamp de *Data Science* Aplicada (Alura)**, no último módulo. Neste, vimos alguns tópicos como modelos de ***Machine Learning***, ***Cross-Validation***, ***Efeitos da Aleatoriedade ao dividir o DataFrame em treino e teste***, ***Tuning de Hiperparâmetros*** ***etc***. As aulas foram feitas em cima de um Dataset do *Kaggle*, disponibilizado pelo **Hospital Sírio-Libanês**.

Como desafio, foi requisitado um **modelo que prevê a admissão de pacientes com COVID-19 para leitos de UTI**, ou seja, se a pessoa está em um risco maior por conta do novo coronavírus, além de realizar uma ***EDA*** dos dados.

- Análise completa em "**previsao_leitos_uti_brasil.ipynb**".

Observação: para ver alguns gráficos com o `plotly`, clique em "Open in Colab", no começo do *notebook*. O Github, infelizmente, não tem compatibilidade com a biblioteca.

# Contexto
---
A pandemia da COVID-19 atingiu o mundo inteiro, sobrecarregando assim os sistemas de saúde, os quais estavam despreparados para uma solicitação tão intensa e demorada de leitos de UTI, profissionais, equipamentos de proteção individual e recursos de saúde.

![colapse](/img/colapse.jpg)

Acima vemos a curva **cinza** sendo o ideal, pois está abaixo da capacidade que o sistema está dispondo. Obviamente que, medidas como:

- **Lavar sempre as mãos**;
- **Passar álcool em gel frequentemente e usar máscaras ao sair de casa**;
- **Se possível, ainda adotar o isolamento social**.

Ajudam muito a reduzir esses aumentos de casos diários, deixando a curva vermelha mais parecida com a situação que queremos. Entretanto, se essas não forem seguidas, teríamos que aumentar a capacidade dos sistemas de saúde, e assim, exigindo verba e tempo até adquirir mais recursos médicos.

Mas o que devemos fazer se a quantidade de leitos ficar estável e não existirem muitos equipamentos sobrando? É aí que nos deparamos com um outro problema, relacionado a **gestão de pessoas que vão para a UTI do próprio hospital:**

- Se encontrarmos uma situação em que todos os leitos estão ocupados, precisamos de **agilidade para evitar um possível superlotamento do espaço**, e além disso, teríamos que **transferir as pessoas para a UTI de outros hospitais**!
- Se determinado hospital estiver com leitos disponíveis, poderíamos contatar os outros locais de saúde para **transferir mais pessoas neste**.

Sendo assim, pensando na **necessidade de ser ágil** para a ida e vinda de pacientes, o Hospital Sírio-Libanês está tentando buscar uma solução para o caso, isto é, **prever se a pessoa precisará de leito antecipadamente**. E é disso que se trata este projeto.

**Base de dados:** [Dataset do Hospital Sírio-Libanês (*Kaggle*)](https://www.kaggle.com/S%C3%ADrio-Libanes/covid19), com todas as features já *normalizadas*.

# Ferramentas utilizadas
---
- Linguagem de programação Python;
- Pacotes: `pandas`, `numpy`, `matplotlib`, `seaborn`, `plotly`, `IPython`, `sklearn` e `picke`;
- Notebook: Google Colaboratory.

# Descrição breve das etapas
---
## 1. Pré-processamento dos dados

### 1.1. Primeiro momento
- Preenchimento de valores faltantes com métodos "*backfill*" e "*fowardfill*" dado um mesmo paciente;
- Remoção de pacientes que já são declarados como `ICU` = 1 na primeira janela (`0-2`);
- Remoção dos pacientes que não foram coletadas nenhuma informação de certa variável em nenhuma das janelas;
- Seleção de uma única janela de tempo (`0-2`);
- Criação do *DataFrame* utilizado pelo Sírio-Libanês.

### 1.2. Segundo momento
- Remoção das variáveis que possuem `MIN`, `MAX` e `MEDIAN` no final.

### 1.3. Terceiro momento
- Remoção das variáveis altamente correlacionadas com alguma outra;
- Junção do meu *DataFrame* com o do hospital.

## 2. Análise Exploratória dos Dados
Utilizei gráficos de barras, *violinplots*, *boxplots*, matriz de correlação (mapa de calor) e gráficos de regressão. Vão alguns exemplos de visualizações abaixo:

![age_percentil](/img/age_percentil.png)

![icu_age_above](/img/icu_age_above.png)

![gender](/img/gender.png)

![blood_vital_signs](/img/blood_vital_signs.png)

![correlation_map](/img/correlation_map.png)

![regression_plots_high_corr](/img/regression_plots_high_corr.png)

## 3. Machine Learning: modelos testados
Foram testados:
- Naive-Bayes;
- Regressão Logística;
- Árvore de Decisão;
- Random Forest.

## 4. Procedimento para saber qual deles é o melhor
1. Divisão das variáveis em dependente (y) e independentes (X);
2. Cross-Validation com todo o *DataFrame*, calculando também o intervalo de confiança para os resultados (métricas) dos modelos (o intervalo foi calculado pois existiam poucas linhas, podendo haver um viés aleatório na divisão de treino e teste);
3. Avaliação de todas as métricas e seleção do melhor modelo. Abaixo vão os resultados:

![accuracy](/img/accuracy.png)

![precision](/img/precision.png)

![recall](/img/recall.png)

![f1-score](/img/f1-score.png)

![roc-auc](/img/roc-auc.png)

Após algumas análises, a ***Random Forest Classifier*** foi a escolhida, por ela ser a primeira em quase todas as métricas! Na verdade, o que realmente pesou foi o melhor *Recall*, pois quanto menos falsos negativos tiver, ou seja, a pessoa não ser admitida para UTI mas que na verdade era para ser, é melhor. Um *Recall* ruim pode literalmente matar pessoas... Então optei também pelo modelo com a melhor sensibilidade.

## 5. *Tuning* de Hiperparâmetros
Acabei testando no `GridSearchCV()` (função do `sklearn`) valores diferentes para `max_features` e `n_estimators`. O resultado foi:

- `max_features`: 7;
- `n_estimators`: 2000.

Observação: se rodarmos o notebook denovo, **poderemos encontrar outros valores para esses hiperparâmetros, por conta da baixa quantidade de pacientes e o fator aleatório**!

## 6. Visualizações extras

- *Coutor plots* (2D e 3D):

![countor-plot-2d](/img/countor-plot-2d.png)

![countor-plot-3d](/img/countor-plot-3d.png)

- *Feature Importances*:

![feature-importances](/img/feature-importances.png)
