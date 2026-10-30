# Tecnologias Inteligentes Aplicadas à Saúde: Resumo das Aulas

## Como a disciplina funciona

A nota é dividida em participação nas aulas (20%), anotações em sala (20%) e desafios e trabalhos (60%).

## Predição e previsão

Os dois termos parecem iguais, mas têm sentidos diferentes. Predição vem de "predizer", ou seja, dizer antes. É dar uma resposta estimada de forma geral, sem falar de tempo futuro: o computador olha os dados e classifica, categoriza, rotula ou etiqueta alguma coisa. Previsão vem de "prever", ou seja, ver antes. É mais específica e sempre envolve tempo: usa dados do passado, organizados em uma série temporal, para estimar como uma variável vai se comportar no futuro, como o próximo valor da série.

Nos dois casos é preciso ter uma base de dados robusta, com um volume grande e consistente de informações. Sem isso, o resultado não é confiável.

## Sistema de recomendação e sistema de apoio à decisão

O sistema de recomendação olha o histórico de uma pessoa e recomenda algo com base no padrão pessoal dela. Já o sistema de apoio à decisão (SAD) olha o histórico geral, de todo mundo, e sugere algo com base nos padrões coletivos. Em resumo: um é feito para o indivíduo, o outro para o grupo. Por baixo, a recomendação usa predição e previsão para funcionar.

## Dados, KDD e a diferença para a estatística

KDD significa descoberta de conhecimento em grandes bases de dados. Dentro dele está a mineração de dados, que usa o reconhecimento de padrões. Esse reconhecimento é feito com redes neurais artificiais e matemática, e é nele que ficam os algoritmos de predição e de previsão.

A estatística e a mineração de dados também se diferenciam pelo ponto de partida. Na estatística, a gente pega uma amostra pequena para entender o todo. Na mineração, a gente pega o todo para entender um indivíduo.

## Como resolver um problema, passo a passo

Para atacar um problema, a ordem é esta: primeiro decidir se ele é de predição ou de previsão, depois escolher a base de dados, aplicar o ETL e, por fim, definir o modelo e compará-lo com outros (o chamado benchmark). Antes de tudo isso, é preciso entender a regra do negócio, que define quais atributos serão avaliados.

O ETL significa extração, transformação e carga. É a etapa de organizar os dados antes do treinamento. Os dados podem vir de um banco de dados, usando plugins de conexão do pandas, ou de um arquivo csv. Também é preciso padronizar a estrutura dos dados que serão minerados, usando listas, dicionários ou pares de chave e dado.

## Modelos de predição

Existem vários modelos de classificação, e cada um tem pontos fortes e fracos.

A **Árvore de Decisão** (Decision Tree) funciona com regras do tipo "se isso, então aquilo". É fácil de interpretar, mas pode sofrer de overfitting, que é quando o modelo decora os exemplos de treino e erra quando aparecem dados novos.

A **Floresta Aleatória** (Random Forest) é um conjunto de várias árvores de decisão. Ela tira a média das respostas das árvores, por isso é mais robusta que uma árvore sozinha e tem boa performance geral.

O **KNN** (K-Nearest Neighbors) classifica um item olhando para os vizinhos mais próximos dele. É simples e funciona bem com poucos dados, mas fica lento quando o volume é grande.

O **SVM** (Support Vector Machine) tenta encontrar o melhor limite que separa as classes. Vai bem quando há muitas variáveis, mas pode ser lento e difícil de ajustar.

O **Naive Bayes** se baseia em probabilidade, usando o Teorema de Bayes. É rápido e eficiente, mas parte da ideia de que as variáveis são independentes entre si, o que nem sempre é verdade.

O **Gradient Boosting** aprende com os próprios erros: cada novo modelo tenta corrigir o anterior. Tem alta acurácia e é muito eficaz, só que demora mais para treinar.

Por fim, existem os modelos **XGBoost, LightGBM e CatBoost**, que não vêm com o scikit-learn e precisam ser instalados à parte. São muito usados em competições de machine learning, como as do Kaggle, e têm altíssima performance e muito controle, mas são mais complexos.

Trocar de modelo é simples: basta mudar a linha que cria o modelo e manter o resto do código igual, ou seja, treinar com `fit` e testar com `predict`. Para comparar os modelos, usam-se ferramentas como `accuracy_score`, `classification_report`, `confusion_matrix` e `cross_val_score`.

## Acurácia e F1-Score

A acurácia mede quantas previsões o modelo acertou em relação ao total. A conta é: verdadeiros positivos mais verdadeiros negativos, divididos pelo total de previsões. Ela funciona bem quando as classes estão equilibradas, com quantidades parecidas de cada tipo. O problema é que ela pode enganar quando as classes são desbalanceadas. Se 95% dos dados são de uma única classe, um modelo que sempre responde essa classe já tem 95% de acurácia, mas não aprendeu nada sobre a classe menor.

O F1-Score resolve isso combinando duas medidas. A precisão responde: entre os casos que o modelo disse que eram positivos, quantos realmente eram? A conta é verdadeiros positivos divididos por verdadeiros positivos mais falsos positivos. O recall, também chamado de sensibilidade, responde: entre os casos que realmente eram positivos, quantos o modelo encontrou? A conta é verdadeiros positivos divididos por verdadeiros positivos mais falsos negativos. O F1 junta as duas por uma média especial (a média harmônica) e é a melhor escolha quando as classes estão desbalanceadas ou quando é importante equilibrar falsos positivos e falsos negativos.

Resumindo: a acurácia mostra o desempenho geral do modelo, e o F1-Score dá uma avaliação mais equilibrada entre evitar alarmes falsos e deixar casos passarem sem ser notados.

## PyCaret

O PyCaret é uma biblioteca que automatiza boa parte do trabalho de treinar, comparar e avaliar modelos. A atividade proposta foi refazer com ele os dois problemas de comparação de modelos feitos antes de forma manual. A função `compare_models` treina vários modelos de classificação de uma vez e mostra qual deles se saiu melhor.

## IA generativa: LLM, RAG, agentes e MCP

Esta parte da disciplina apresentou os LLMs (modelos de linguagem grandes, como o próprio Claude), o RAG, os agentes de IA e o MCP. O RAG é uma forma de fazer o modelo responder usando documentos e dados específicos, em vez de depender só do que ele já aprendeu. O agente de IA é um sistema que executa tarefas usando ferramentas. O MCP é um padrão para conectar a IA a essas ferramentas e fontes de dados.

Uma ideia mais completa combina LLM, transformers, embeddings e RAG. Os transformers são o tipo de arquitetura por trás dos LLMs, e os embeddings transformam textos em números para que o computador consiga comparar significados. Com a engenharia de prompt, dá para pedir que a resposta venha em formato JSON, que é fácil de ser usado por outros sistemas.

## Pipelines e exemplo prático

Um pipeline é uma sequência de etapas que juntas resolvem um problema. Foi visto que ele pode incluir os modelos preditivos e de previsão, treinados de forma manual ou com PyCaret, e também a IA generativa. O exemplo trabalhado foi o Somdiabetes, em que um LLM com RAG pode recomendar insulina e gerar relatórios.

## Forma de trabalhar com IA

Foi apresentada uma dinâmica de trabalho para organizar o contexto. Ela usa o SDD (Spec Driven Development), que significa desenvolver a partir de uma especificação escrita antes, com as rotas por URL testadas no Postman. O exemplo de contexto foi o GestãoDS. Também vale manter as referências do escopo do projeto, como acontece com chats dentro de um projeto específico. Outra ideia são as skills, que servem para personalizar a IA, como a skill Caveman.
