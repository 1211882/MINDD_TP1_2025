# MINDD_TP1_2025
MINDD first project


1. Business & Data Understanding

O que fizemos no código:

Leitura do CSV e normalização dos nomes das colunas.

Verificação de duplicados (drop_duplicates).

Análise inicial de estatísticas descritivas (df.describe()) e tipos (df.dtypes).

Distribuição da variável alvo y (percentagem de sim/não).

Como interpretar:

df.describe() dá média, desvio padrão, min, max, percentis para cada coluna numérica.

Se vês valores extremos (outliers) em colunas como age ou campaign, podes considerar tratamento posterior.

Distribuição de y mostra desbalanceamento; se no for 88% e yes 12%, isso indica necessidade de técnicas como SMOTE.

2. Exploratory Data Analysis (EDA)

Código que faz isto:

Histogramas (sns.histplot) das colunas numéricas.

Boxplots (sns.boxplot) para detetar outliers.

Percentagem de unknown em colunas categóricas.

Como interpretar:

Histogramas: mostram a frequência de cada valor. Se a distribuição é enviesada (skewed), podes pensar em transformar ou normalizar.

Boxplots: pontos fora das “caixas” são outliers. Podem ser removidos ou tratados, dependendo do impacto.

Percentagem de unknown: valores desconhecidos são substituídos pelo modo. Ex.: se job tem 80% unknown, isso indica que a coluna pode não ser muito informativa.

3. Data Cleaning & Transformation

Passos do código:

Substituir unknown pelo modo (df[col].mode()[0]).

Converter meses para valores ordinais (month_ordinal).

One-hot encoding para meses e dias da semana.

Remover colunas irrelevantes (duration, pdays).

Como interpretar:

Substituir unknown pelo modo evita perder linhas e permite usar essas colunas nos modelos.

One-hot encoding transforma variáveis categóricas em colunas binárias, essencial para modelos que não aceitam texto.

Remover duration e pdays: sabemos que não são úteis antes da chamada ou têm muitos valores default (999).

4. Feature Scaling & Selection

Escalonamento:

StandardScaler aplica z-score: média 0, desvio 1.

Importante para modelos que dependem de distância (como Logistic Regression).

Remoção de correlações altas:

upper = corr_matrix.where(...) cria a metade superior da matriz de correlação.

Qualquer coluna com correlação >0.9 é removida (to_drop).

Isto evita multicolinearidade, que distorce coeficientes em regressão.

Random Forest / RFE para seleção:

Random Forest: calcula a importância de cada feature.

RFE (Logistic Regression): seleciona features mais relevantes para prever y.

Como interpretar:

Se importances.head(15) mostra age, campaign, poutcome_success como top 3, estas são as variáveis que mais influenciam a decisão do cliente.

Features muito correlacionadas ou com importância muito baixa podem ser removidas para simplificar o modelo.

5. Train/Test Split + SMOTE

Passos:

Separar treino e teste (train_test_split) com stratify = y.

Aplicar SMOTE apenas no treino.

Como interpretar:

Treino + SMOTE: balanceia classes para que o modelo não fique enviesado para no.

Teste permanece inalterado: simula dados reais desbalanceados.

6. Model Training & Evaluation

Modelos:

Random Forest e Logistic Regression.

Métricas e gráficos:

Classification report:

Precision: proporção de positivos previstos que são realmente positivos.

Recall: proporção de positivos reais que o modelo conseguiu capturar.

F1-score: média harmónica de precision e recall.

Support: número de instâncias da classe no teste.

Confusion Matrix:

2x2 tabela mostrando TP, TN, FP, FN.

Ajuda a ver se o modelo está a errar mais nos yes ou nos no.

AUC & ROC Curve:

ROC: curva False Positive Rate vs True Positive Rate.

AUC: área sob a curva, valor entre 0.5 (aleatório) e 1 (perfeito).

Quanto mais próximo de 1, melhor.

Feature Importance (RF):

Gráfico de barras horizontal mostra quais features influenciam mais o modelo.

Como interpretar:

Um modelo com F1-score alto na classe minoritária (yes) é melhor, especialmente num dataset desbalanceado.

Se AUC >0.8, modelo é considerado bom.

Features importantes indicam onde o banco deve focar políticas de marketing.

7. Visualizações Finais

Matriz de correlação das features selecionadas:

Ajuda a confirmar que não há multicolinearidade.

Valores perto de 1 ou -1 indicam correlação forte.

Histogramas e boxplots:

Permitem comunicar distribuições e outliers no relatório.

Importância das features (RF):

Mostra de forma intuitiva quais variáveis afetam mais a probabilidade do cliente subscrever o produto.

Resumo: Como tirar conclusões do código

EDA → Entender o dataset e identificar problemas (outliers, desbalanceamento, valores desconhecidos).

Data Cleaning & Encoding → Garantir que o dataset está pronto para modelos.

Feature selection → Identificar features relevantes, remover redundantes.

SMOTE → Tratar desbalanceamento de classes.

Treino + Avaliação → Observar métricas, AUC, matriz de confusão, e importância das features.

Conclusões → Quais features são determinantes, limitações, recomendações de ação.