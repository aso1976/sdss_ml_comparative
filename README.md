# Classificação Estelar com Machine Learning: SDSS DR17 🌌

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Scikit-Learn](https://img.shields.io/badge/Library-Scikit--Learn-orange)
![Status](https://img.shields.io/badge/Status-Concluído-success)

> **Uma Análise Comparativa entre Regressão Logística, Árvore de Decisão e Random Forest.**

Este projeto aplica técnicas de Ciência de Dados e Machine Learning para classificar objetos celestes (**Estrelas, Galáxias e Quasares**) utilizando dados espectrais do *Sloan Digital Sky Survey* (Data Release 17).

---

## 📋 Tabela de Conteúdos
1. [Sobre o Projeto](#-sobre-o-projeto)
2. [O Dataset](#-o-dataset)
3. [Metodologia e Engenharia de Features](#-metodologia-e-engenharia-de-features)
4. [Modelos Avaliados](#-modelos-avaliados)
5. [Resultados](#-resultados)
6. [Autores](#-autores)

---

## 🔭 Sobre o Projeto
A classificação automatizada de objetos astronômicos é crucial para lidar com o volume massivo de dados gerados por telescópios modernos. Este estudo visa comparar a eficácia de diferentes algoritmos supervisionados na distinção entre três classes fundamentais:

* **GALAXY:** Aglomerados massivos de estrelas e matéria escura.
* **STAR:** Fontes pontuais de luz locais (Via Láctea).
* **QSO (Quasar):** Núcleos galácticos ativos e distantes, altamente luminosos.

O projeto foi desenvolvido como parte do curso de **Ciência de Dados da FATEC Jundiaí (2025)**.

---

## 💾 O Dataset
Os dados são públicos e provenientes do SDSS DR17.
* **Fonte:** [Kaggle - Stellar Classification Dataset - SDSS17](https://www.kaggle.com/fedesoriano/stellar-classification-dataset-sdss17)
* **Dimensões:** 100.000 observações x 18 colunas.
* **Target:** Coluna `class` (Desbalanceada: ~60% Galáxias).

---

## ⚙️ Metodologia e Engenharia de Features

O pipeline de processamento seguiu etapas rigorosas para garantir a robustez dos modelos:

### 1. Limpeza de Dados (Data Cleaning)
* Remoção de colunas de identificação (IDs) que causariam *data leakage* (`obj_ID`, `spec_obj_ID`, `plate`, etc.).
* Filtragem de ruído: Remoção de linhas contendo valores de erro (`-9999`).

### 2. Engenharia de Features (A Física do Problema)
Identificamos alta multicolinearidade entre os filtros fotométricos originais (`u, g, r, i, z`). Para resolver isso e capturar melhor a física espectral, criamos **Índices de Cor**:

* **Cálculo de Proporção de Fluxo:** Criamos novas variáveis (`u_g`, `g_r`, `r_i`, `i_z`) subtraindo as magnitudes (logaritmo do fluxo). Isso representa a "cor" ou temperatura do objeto.
* **Seleção de Variáveis:** Mantivemos apenas a banda **`r`** (como referência de luminosidade total) e removemos as bandas redundantes, além das coordenadas espaciais (`alpha`, `delta`) que não influenciam a natureza do objeto.

### 3. Análise Exploratória (EDA)
* Análise de distribuição do **Redshift** (fator determinante para Quasares).
* Matrizes de correlação para validar a redução de multicolinearidade.

---

## 🤖 Modelos Avaliados

Utilizamos validação cruzada (`StratifiedKFold`) para garantir que os resultados não fossem fruto do acaso:

1.  **Regressão Logística (Multinomial):**
    * *Papel:* Baseline linear.
    * *Configuração:* Solver `lbfgs`, max_iter=1000.
2.  **Árvore de Decisão (Decision Tree):**
    * *Papel:* Modelo não-linear interpretável.
    * *Configuração:* Profundidade limitada para evitar overfitting.
3.  **Random Forest:**
    * *Papel:* Modelo de Ensemble robusto.
    * *Configuração:* 100 estimadores.

---

## 📊 Resultados

| Modelo | Acurácia Média (CV) | Observações |
| :--- | :---: | :--- |
| **Random Forest** | **~97.8%** | Melhor performance geral e robustez a outliers. |
| Árvore de Decisão | ~96.5% | Boa performance, mas tendência a overfitting leve. |
| Regressão Logística | ~95.0% | Surpreendentemente eficaz após a normalização dos dados. |

> **Insight Principal:** O `redshift` provou ser a feature mais importante. Objetos com alto redshift são quase invariavelmente Quasares, enquanto a distinção entre Estrelas e Galáxias depende mais dos índices de cor (`u_g`, `g_r`).

---

## 🛠️ Tecnologias Utilizadas

* **Linguagem:** Python 3.x
* **Manipulação de Dados:** Pandas, NumPy
* **Visualização:** Seaborn, Matplotlib
* **Machine Learning:** Scikit-Learn

---

## 👨‍💻 Autores

* **André Santos de Oliveira**
* Guilherme Esteves Marret
* Gustavo Henrique Bueno
* Sofia Costa Se

---

### Como reproduzir este projeto

1. Clone o repositório.
2. Instale as dependências: `pip install pandas numpy seaborn scikit-learn matplotlib`
3. Execute o notebook `SDSS_Comparativo_Modelos_ML.ipynb`.

---
*Dados extraídos do Sloan Digital Sky Survey (SDSS). Licença de uso público conforme DR17.*