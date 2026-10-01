# CP02 — SERS | Machine Learning

Projeto de **Machine Learning** desenvolvido para o CP02 de SERS, com duas tarefas: **classificação de fontes de geração de energia** e **regressão da radiação solar**.

# Integrantes:

Gabriel Camarosani Gouvea Gonçalves da Silva — RM 569189

Gustavo Lima Andrade Santos — RM 571709

Gabriel Carvalho - RM 571381

Guilherme Cedro Teixeira - RM 571050

## Objetivo

O projeto aplica algoritmos de aprendizado de máquina a dois problemas relacionados ao setor de energia:

1. **Classificação (ANEEL):** classificar uma unidade de geração como **Solar, Eólica ou Hidráulica** a partir de sua potência instalada e localização (latitude e longitude).
2. **Regressão meteorológica:** estimar a **radiação solar (W/m²)** a partir de temperatura, umidade, cobertura de nuvens, velocidade do vento e hora do dia.

São comparados diferentes algoritmos e métricas de avaliação para identificar o comportamento dos modelos e discutir suas aplicações e limitações.

## Dados utilizados

### 1. Classificação — ANEEL

Arquivo: `aneel_classificacao_orange.csv`

* **Fonte indicada no projeto:** ANEEL (Agência Nacional de Energia Elétrica).
* **Quantidade:** 3.876 registros.
* **Variáveis de entrada:** `potencia_kw`, `latitude` e `longitude`.
* **Variável-alvo:** `fonte`.
* **Classes:** Solar, Eólica e Hidráulica.
* **Período:** o conjunto não possui uma coluna de data/hora; portanto, não há um período temporal a ser informado. Trata-se de um conjunto de registros de unidades de geração utilizado para a tarefa de classificação.
* **Distribuição das classes:** 1.476 registros de Hidráulica, 1.200 de Solar e 1.200 de Eólica.

### 2. Regressão — dados meteorológicos

Arquivo: `meteo_regressao_orange.csv`

* **Fonte:** conjunto de dados meteorológicos disponibilizado no próprio projeto para a atividade.
* **Quantidade:** 1.001 registros.
* **Período dos dados:** **01/04/2025 a 30/06/2025**.
* **Variáveis de entrada:** temperatura (°C), umidade (%), cobertura de nuvens (%), vento (km/h) e hora.
* **Variável-alvo:** `radiacao_w_m2`, em W/m².
* A coluna `data_hora` é utilizada para identificação temporal, mas não entra como variável de entrada no modelo.

## Metodologia

### Classificação

Os dados são separados em:

* **80% para treinamento:** 3.100 registros.
* **20% para teste:** 776 registros.
* `random_state = 17`.

Foram avaliados:

* Regressão Logística;
* Random Forest Classifier;
* K-Nearest Neighbors (KNN).

As principais métricas utilizadas foram **acurácia, precisão, recall e F1-score**, além da análise das matrizes de confusão.

### Regressão

Os dados são separados em:

* **80% para treinamento:** 800 registros.
* **20% para teste:** 201 registros.
* `random_state = 17`.

Foram avaliados:

* Regressão Linear;
* Random Forest Regressor;
* Decision Tree Regressor.

As métricas utilizadas foram **R², MAE e MSE**.

## Como executar o notebook

O projeto utiliza Python e pode ser executado preferencialmente no **Google Colab**.

### Opção 1 — Google Colab

1. Acesse o repositório no GitHub.
2. Abra o arquivo **`CP2_SERS.ipynb`**.
3. Abra o notebook no Google Colab.
4. Faça upload dos arquivos:

   * `aneel_classificacao_orange.csv`
   * `meteo_regressao_orange.csv`
5. Execute as células em ordem, de cima para baixo.

> **Importante:** o notebook foi configurado para ler os arquivos a partir de `/content/`, por exemplo `/content/aneel_classificacao_orange.csv`. Por isso, no Colab os CSVs devem estar disponíveis nesse diretório.

### Opção 2 — Execução local

Instale Python 3 e as bibliotecas utilizadas:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

Depois, clone o repositório:

```bash
git clone https://github.com/gabrielcggs/CP02-SERS-MACHINE-LEARNING.git
cd CP02-SERS-MACHINE-LEARNING
jupyter notebook CP2_SERS.ipynb
```

Na execução local, ajuste os caminhos dos CSVs no notebook, substituindo `/content/` pelo diretório em que os arquivos estão armazenados.

## Resultados

### Classificação

| Modelo                       |   Acurácia | Precisão (weighted) | Recall (weighted) | F1-score (weighted) |
| ---------------------------- | ---------: | ------------------: | ----------------: | ------------------: |
| Regressão Logística          |     71,13% |                0,71 |              0,71 |                0,71 |
| **Random Forest Classifier** | **97,81%** |            **0,98** |          **0,98** |            **0,98** |
| KNN                          |     85,57% |                0,86 |              0,86 |                0,86 |

O **Random Forest Classifier** apresentou acurácia de **97,81%** no conjunto de teste (776 registros).

No relatório por classe, os F1-scores foram:

* **Eólica:** 0,97
* **Hidráulica:** 0,98
* **Solar:** 0,98

A matriz de confusão mostrou poucos erros de classificação, indicando que, neste conjunto de dados, potência e localização foram suficientes para produzir uma separação muito boa entre as três classes.

O notebook também destaca que, em uma aplicação real, seria recomendável incorporar informações adicionais, como histórico de produção, condições climáticas, tecnologia da usina e fatores operacionais.

### Regressão

| Modelo                      |         R² | MAE (W/m²) | MSE ((W/m²)²) |
| --------------------------- | ---------: | ---------: | ------------: |
| Regressão Linear            |     0,6429 |     118,17 |     22.490,22 |
| Decision Tree Regressor     |     0,8739 |      61,15 |      7.945,73 |
| **Random Forest Regressor** | **0,9393** |  **44,46** |  **3.824,89** |

O **Random Forest Regressor** apresentou R² de aproximadamente **0,94**, explicando cerca de 94% da variabilidade observada no conjunto de teste, com MAE de aproximadamente **44,46 W/m²**.

A Regressão Linear apresentou R² de 0,6429, enquanto a Decision Tree atingiu 0,8739.

Os resultados indicam que os modelos baseados em árvores capturam melhor as relações não lineares entre as variáveis meteorológicas e a radiação solar.

O notebook também analisa a importância da variável **hora**, relacionada ao ciclo diário da radiação solar.

## Conclusão

O projeto demonstra a aplicação de Machine Learning em dois problemas distintos do setor de energia.

Na **classificação**, o Random Forest apresentou desempenho elevado na identificação das fontes Solar, Eólica e Hidráulica, alcançando **97,81% de acurácia** no conjunto de teste.

Na **regressão**, o Random Forest Regressor apresentou o maior R² e os menores erros entre os modelos avaliados, alcançando **R² de 0,9393**, **MAE de 44,46 W/m²** e **MSE de 3.824,89**.

Apesar dos bons resultados experimentais, os modelos devem ser interpretados dentro das características dos conjuntos utilizados.

Em especial, a previsão de radiação solar não equivale diretamente à previsão de geração fotovoltaica, pois a produção elétrica também depende de fatores como eficiência dos painéis, temperatura das células, orientação e inclinação, sombreamento, sujeira e perdas do sistema.

## Estrutura do repositório

```text
CP02-SERS-MACHINE-LEARNING/
├── CP2_SERS.ipynb
├── aneel_classificacao_orange.csv
├── meteo_regressao_orange.csv
└── README.md
```

## Notebook

[CP2_SERS.ipynb](./CP2_SERS.ipynb)

---

**Projeto acadêmico — CP02 SERS | Machine Learning**
