# COVID-19: evitando um possível colapso nos sistemas de saúde
---
Este projeto foi desenvolvido durante o **Bootcamp de *Data Science* Aplicada (Alura)**, no último módulo. Neste, vimos alguns tópicos como modelos de ***Machine Learning***, ***Cross-Validation***, ***Efeitos da Aleatoriedade ao dividir o DataFrame em treino e teste***, ***Tuning de Hiperparâmetros*** ***etc***. As aulas foram feitas em cima de um Dataset do *Kaggle*, disponibilizado pelo **Hospital Sírio-Libanês**.

Como desafio, foi requisitado um **modelo que prevê a admissão de pacientes com COVID-19 para leitos de UTI**, ou seja, se a pessoa está em um risco maior por conta do novo coronavírus, além de realizar uma ***EDA*** dos dados.

- Análise completa em "**previsao_leitos_uti_brasil.ipynb**".

Observação: para ver alguns gráficos com o `plotly`, clique em "Open in Colab", no começo do *notebook*. O Github, infelizmente, não tem uma compatibilidade com a biblioteca.

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

**Base de dados:** [Dataset do Hospital Sírio-Libanês (*Kaggle*)](https://www.kaggle.com/S%C3%ADrio-Libanes/covid19).

# Ferramentas utilizadas
---
- Linguagem de programação Python;
- Pacotes: `pandas`, `numpy`, `matplotlib`, `seaborn`, `plotly`, `IPython`, `sklearn` e `picke`;
- Notebook: Google Colaboratory.

# Descrição breve das etapas
---
## 1. Pré-processamento

### 1.1. Primeiro momento
- Preenchimento de valores faltantes com métodos "*backfill*" e "*fowardfill*" dado um mesmo paciente;
- Remoção de pacientes que já são declarados como `ICU` = 1 na primeira janela (`0-2`);
- Remoção dos pacientes que não foram coletadas nenhuma informação de certa variável em nenhuma das janelas;
- Seleção de uma única janela de tempo (`0-2`).

### 1.2. Segundo momento
- Remoção das variáveis que possuem `MIN`, `MAX` e `MEDIAN` no final.

### 1.3. Terceiro momento
- Remoção das variáveis altamente correlacionadas com alguma outra.

## 2. Análise Exploratória dos Dados
Utilizei gráficos de barras, *violinplots*, *boxplots*, matriz de correlação (mapa de calor) e gráficos de regressão. Vão alguns exemplos de visualizações abaixo:
![](https://github.com/Emersonmiady/rg-bank/blob/main/img/montant.png?raw=true)
![](https://github.com/Emersonmiady/rg-bank/blob/main/img/fraud_hour.png?raw=true)
![](https://github.com/Emersonmiady/rg-bank/blob/main/img/fraud_day.png?raw=true)



## 3. Preparando para o ML
Transformei as variáveis categóricas relevantes em *dummies*, depois dividi em 0.2 de teste e apliquei o `StandardScaler()` para a padronização dos dados, já que ultilizaríamos um modelo de Regressão Logística para a comparação de modelos.

## 4. Machine Learning: modelos testados
Foram testados:
- Naive-Bayes;
- Regressão Logística;
- Árvore de Decisão;
- Random Forest.

## 4. Resultados das métricas

![](https://github.com/Emersonmiady/rg-bank/blob/main/img/models_description.png)

O melhor modelo foi o que teve melhor Recall, pelo menos quando utilizamos as regras de negócio. Ou seja, foi a Árvore de Decisão!

## 5. Resultados de Negócio
1. "O faturamento esperado pela empresa é igual a: 3.004.400.679,40, se considerarmos 1/4 das transações em 1 mês. Entretanto, se fossemos deduzir o quanto se aumenta, para 100% das transações mensais, temos a possibilidade de encontrar 3.76x esse valor!"

2. "O prejuízo esperado pela empresa é igual a: 45.659.154,44, se considerarmos 1/4 das transações em 1 mês. Entretanto, se fossemos deduzir o quanto se aumenta, para 100% das transações mensais, temos a possibilidade de encontrar 21.19x esse valor!"

3. "O lucro esperado pela empresa é igual a: 2.958.741.524,96, se considerarmos 1/4 das transações em 1 mês. Entretanto, se fossemos deduzir o quanto se aumenta, para 100% das transações mensais, temos a possibilidade de encontrar 3.5x esse valor!"

**Observação:** não sabímos a unidade monetária!
