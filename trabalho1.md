# Relatório de Análise e Comparação de Modelos de Classificação

---

## 1. Conceitos Fundamentais

* **Acurácia (Accuracy):** Proporção geral de predições corretas em relação ao total de casos avaliados. É útil como visão panorâmica, mas pode ser enganosa em bases com classes desbalanceadas.
* **F1-Score (Macro):** Média harmônica entre **Precisão** (capacidade de evitar alarmes falsos) e **Recall/Revocação** (capacidade de capturar todos os casos positivos reais). Na modalidade **Macro**, calcula-se o F1 individual de cada classe e extrai-se a média simples, atribuindo peso igual a ambas as classes.
* **Overfitting (Sobreajuste):** Ocorre quando o algoritmo decora ruídos, exceções e particularidades do conjunto de treino em vez de identificar o padrão geral subjacente. Manifesta-se por métricas excelentes no treino e queda acentuada nos dados de teste.
* **Matriz de Confusão ($2 \times 2$):**
  * **Verdadeiro Negativo (VN - topo/esquerda):** Modelo previu 0 e a realidade era 0 (Acerto).
  * **Falso Positivo (FP - topo/direita):** Modelo previu 1 e a realidade era 0 (Alarme falso).
  * **Falso Negativo (FN - base/esquerda):** Modelo previu 0 e a realidade era 1 (Omissão/Perda do alvo).
  * **Verdadeiro Positivo (VP - base/direita):** Modelo previu 1 e a realidade era 1 (Acerto).

---

## 2. Comparativo entre os Problemas

| Aspecto | Problema 1: Predição de Compra | Problema 2: Risco de Internação |
| :--- | :--- | :--- |
| **Domínio das Variáveis** | Comportamento comercial (Idade, Renda, Score, Engajamento). | Parâmetros clínicos (Idade, Pressão Arterial, Colesterol, Frequência Cardíaca). |
| **Qualidade do Sinal** | Ruído estatístico proposital e fronteira de decisão difusa. | Correlação fisiológica forte e direta com o desfecho. |
| **Balanceamento (Teste)** | Desbalanceado: 45 casos classe 0 (60%) vs. 30 casos classe 1 (40%). | Balanceado: 38 casos classe 0 (50,7%) vs. 37 casos classe 1 (49,3%). |
| **Desempenho Geral** | Modesto (Acurácias entre 60,00% e 69,33%). | Elevado (Acurácias entre 69,33% e 81,33%). |
| **Padrão de Erro Crítico** | Alta taxa de Falsos Negativos (ignora 47% a 73% dos compradores). | Erros mais equilibrados entre falsos alarmes e omissões. |
| **Melhor Modelo (F1 Macro)** | **Random Forest** (Acurácia: 0.6933 \| F1: 0.6514). | **Naive Bayes** (Acurácia: 0.8133 \| F1: 0.8125). |

---

## 3. Análise Detalhada dos Resultados

### Problema 1 (Predição de Compra)
* **Sinais de Overfitting:** Modelos não lineares como *Decision Tree* e *Random Forest* atingem taxas próximas de 100% no treino sem restrições, mas caíram para ~65% e ~69% no teste. O *Gradient Boosting* sofreu a pior degradação (acurácia de 60% e F1 de 0.5297), evidenciando que memorizou o ruído sintético.
* **Viés Conservador:** Algoritmos lineares/probabilísticos (*Logistic Regression*, *Naive Bayes*, *SVM*) tenderam fortemente para a classe majoritária (não comprou), sacrificando a identificação dos compradores reais (recalls entre 37% e 40%).
* **Métrica Recomendada:** **F1-Score (Macro)**, pois penaliza a incapacidade dos modelos em prever a classe minoritária, revelando falhas que a acurácia isolada mascara.

### Problema 2 (Risco de Internação)
* **Generalização Consistente:** Devido à relevância física dos atributos clínicos, os modelos apresentaram generalização sólida para dados novos.
* **Destaques:** O **Naive Bayes** obteve o melhor equilíbrio (81,33% de acerto e apenas 5 alarmes falsos). A **Decision Tree**, embora propensa a overfitting por gerar 11 falsos alarmes, foi a que menos liberou pacientes graves por engano (apenas 7 falsos negativos).
* **Métrica Recomendada:** **F1-Score (Macro)** (ou acompanhamento direto do **Recall da classe 1**), dado que em saúde o custo de um falso negativo (não internar quem precisa) é clinicamente crítico.

---

## 4. Por que os Resultados Diferiram?

1. **Relação Causa-Efeito vs. Ruído:** Indicadores vitais possuem limites biológicos bem definidos (hipertensão, arritmia), facilitando o traçado das fronteiras de decisão pelos algoritmos. Já os dados de compra continham ruído gerado artificialmente que embaçou as relações lineares e não lineares.
2. **Distribuição das Classes:** O balanceamento no Problema 2 forçou os algoritmos a aprenderem ambos os cenários com pesos equivalentes. No Problema 1, a classe majoritária induziu os modelos a favorecerem o chute conservador na classe 0.
3. **Comportamento dos Ensembles:** No Problema 1, os ensembles ajustaram-se excessivamente às flutuações randômicas. No Problema 2, o sinal claro permitiu que convergissem para fronteiras mais estáveis.

---

## 5. Veredito sobre Prontidão para Produção

### Problema 1: **Inviável para Produção**
* **Justificativa:** Taxas de falsos negativos entre 47% e 73% causariam perda massiva de oportunidades de negócio e desperdício de recursos de marketing.
* **Passos necessários:**
  * Aplicar balanceamento de classes (`class_weight='balanced'`).
  * Podar hiperparâmetros (`max_depth`, `min_samples_split`) para conter a memorização do ruído.
  * Ajustar o limiar de corte de probabilidade (ex.: aceitar previsões de compra a partir de 35% ou 40%).

### Problema 2: **Viável como Ferramenta de Apoio à Decisão (Não Autônomo)**
* **Justificativa:** Modelos como *Naive Bayes* e *Regressão Logística* exibiram consistência satisfatória (>76% a 81% de acerto), sendo adequados para triagem preliminar ou ordenação de filas de atendimento, sob supervisão médica humana.
* **Passos necessários:**
  * Ajustar o limiar de decisão para priorizar o recall da classe 1, minimizando ao máximo os falsos negativos restantes.
  * Realizar validação cruzada estratificada (*Stratified K-Fold*) e validar com uma base de dados externa de maior escala.
