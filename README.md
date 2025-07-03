# Classificação de Pokémons da Primeira Geração Utilizando Descritores de Imagens

[cite_start]Este repositório contém o código e os resultados de um projeto que propõe o desenvolvimento de um sistema inteligente para a classificação automática de imagens dos Pokémons da primeira geração: Bulbasaur, Charmander e Squirtle[cite: 6].

## 1. Introdução

[cite_start]O sistema foi construído utilizando extração de características, combinando um descritor de textura (LBP) e uma rede neural convolucional (VGG16)[cite: 7]. [cite_start]Foram empregados e otimizados três classificadores distintos: k-Nearest Neighbors (k-NN), Support Vector Machine (SVM) e Árvores de Decisão, utilizando a biblioteca scikit-learn[cite: 8]. [cite_start]O objetivo principal foi avaliar e comparar o desempenho destas abordagens na tarefa de classificação proposta[cite: 9].

## 2. Base de Dados

[cite_start]A base de dados foi composta por imagens obtidas de uma coleção disponível na plataforma Kaggle[cite: 11]. [cite_start]O conjunto de dados contém um total de 337 imagens, divididas em 216 para treino e 121 para teste[cite: 12]. [cite_start]As imagens foram organizadas em três classes correspondentes aos Pokémons alvo[cite: 13].

## 3. Extração de Características

[cite_start]Foi adotada uma abordagem de combinação de características, onde o vetor final de cada imagem é a concatenação de características extraídas por diferentes métodos, visando capturar tanto informações de texturas quanto semânticas[cite: 16].

### 3.1. Descritor Local Binary Pattern (LBP)

[cite_start]O LBP é um descritor de textura que analisa a vizinhança de cada pixel[cite: 18]. [cite_start]As imagens foram convertidas para escala de cinza e o LBP foi aplicado com os seguintes parâmetros[cite: 19]:
* [cite_start]Número de vizinhos: $P=8$ [cite: 20]
* [cite_start]Raio: $R=2$ [cite: 21]
* [cite_start]Método: `nri_uniform` (uniforme e invariante à rotação) [cite: 22]

[cite_start]O resultado para cada imagem foi um histograma de 59 posições, representando a distribuição dos padrões de textura[cite: 23].

### 3.2. Deep Features com VGG16

[cite_start]Utilizou-se a rede neural convolucional VGG16, pré-treinada na base ImageNet, para extrair características profundas[cite: 25]. [cite_start]As imagens (em formato RGB e redimensionadas para $224 \times 224$) foram processadas pela rede, e as ativações da última camada de pooling convolucional foram extraídas[cite: 26]. [cite_start]Para reduzir a dimensionalidade deste vetor, foi aplicado o método Principal Component Analysis (PCA), gerando um vetor de 50 componentes principais por imagem[cite: 27].

### 3.3. Pipeline de Pré-processamento e Otimização de Hiperparâmetros

[cite_start]O vetor de características combinado (LBP + VGG16), com 109 dimensões, passou por um pipeline de pré-processamento que consistiu em[cite: 29]:
* [cite_start]Normalização dos dados com `StandardScaler`[cite: 30].
* [cite_start]Balanceamento com a técnica SMOTE para corrigir desequilíbrio entre as classes majoritárias[cite: 31].

[cite_start]A otimização dos hiperparâmetros foi uma etapa crucial para maximizar o desempenho de cada classificador[cite: 32]. [cite_start]Para isso, foi utilizada a validação cruzada com 5 vias ($k=5$) com o objetivo de encontrar os parâmetros que maximizassem a métrica f1 score média[cite: 33].

* [cite_start]**k-NN e Árvore de Decisão**: Foi empregado o método `GridSearchCV`, que realiza uma busca exaustiva por todas as combinações de hiperparâmetros definidas em um espaço de busca pré-determinado[cite: 35].
* [cite_start]**SVM**: Para este modelo, foi utilizado o `RandomizedSearchCV`, que testa um número fixo de combinações amostradas aleatoriamente a partir de distribuições de parâmetros[cite: 36].

## 4. Classificadores Utilizados

[cite_start]Os parâmetros descritos abaixo são os melhores encontrados após o processo de otimização[cite: 39].

### 4.1. k-Nearest Neighbors (k-NN)

[cite_start]Configurado com os seguintes hiperparâmetros[cite: 41]:
* [cite_start]Número de vizinhos ($k$): 7 [cite: 42]
* [cite_start]Peso dos vizinhos: `distance` (vizinhos mais próximos têm maior influência) [cite: 43]
* [cite_start]Métrica de distância: `cosine` [cite: 44]
* [cite_start]PCA adicional para manter 85% da variância[cite: 45].

### 4.2. Support Vector Machine (SVM)

[cite_start]A SVM foi otimizada com busca aleatória, resultando nos seguintes hiperparâmetros[cite: 47]:
* [cite_start]Kernel: `linear` [cite: 48]
* [cite_start]Parâmetro de regularização ($C$): $\approx0.0072$ [cite: 49]
* [cite_start]Gamma ($\gamma$): $\approx 0.0022$ [cite: 50]
* [cite_start]Peso de classes: `None` [cite: 51]

### 4.3. Árvore de Decisão

[cite_start]Utilizou-se um ensemble de 10 árvores de decisão através do Bagging Classifier, com os seguintes parâmetros para as árvores[cite: 53]:
* [cite_start]Critério de divisão: `entropy` [cite: 55]
* [cite_start]Profundidade máxima: 10 [cite: 56]
* [cite_start]Mínimo de amostras para divisão: 10 [cite: 57]
* [cite_start]Mínimo de amostras por folha: 2 [cite: 58]
* [cite_start]Máximo de features para divisão: `sqrt` [cite: 59]

## 5. Resultados e Análise

[cite_start]Os modelos foram avaliados no conjunto de teste[cite: 61]. As métricas de desempenho (Acurácia, Precisão, Recall e F1-Score) e as matrizes de confusão são apresentadas no documento original.

[cite_start]Na validação cruzada, a Árvore de Decisão obteve a melhor Acurácia (0.8678), seguida pelo SVM (0.8099) e pelo k-NN (0.7355)[cite: 63].

### Desempenho dos Classificadores no Conjunto de Teste

[cite_start]**k-NN (Acurácia: 73.55%)** [cite: 65]

| Classe      | Precisão | Recall | F1-score | Suporte |
|-------------|----------|--------|----------|---------|
| Bulbasaur   | 1.00     | 0.73   | 0.85     | 41      |
| Charmander  | 0.59     | 0.86   | 0.70     | 35      |
| Squirtle    | 0.72     | 0.64   | 0.68     | 45      |
| Média Ponderada | 0.78     | 0.74   | 0.74     | 121     |

[cite_start]**SVM (Acurácia: 80.99%)** [cite: 90]

| Classe      | Precisão | Recall | F1-score | Suporte |
|-------------|----------|--------|----------|---------|
| Bulbasaur   | 0.89     | 0.83   | 0.86     | 41      |
| Charmander  | 0.74     | 0.83   | 0.78     | 35      |
| Squirtle    | 0.80     | 0.78   | 0.79     | 45      |
| Média Ponderada | 0.81     | 0.81   | 0.81     | 121     |

[cite_start]**Árvore de Decisão (Acurácia: 86.78%)** [cite: 117]

| Classe      | Precisão | Recall | F1-score | Suporte |
|-------------|----------|--------|----------|---------|
| Bulbasaur   | 0.86     | 0.93   | 0.89     | 41      |
| Charmander  | 0.93     | 0.77   | 0.84     | 35      |
| Squirtle    | 0.83     | 0.89   | 0.86     | 45      |
| Média Ponderada | 0.87     | 0.87   | 0.87     | 121     |

### Considerações

**Pontos Fortes**:
* [cite_start]O classificador de Árvore de Decisão apresentou o melhor desempenho geral, com acurácia de 86.78% e F1-score ponderado de 0.87[cite: 145]. [cite_start]O uso de um comitê de árvores com profundidade controlada (`max_depth=10`) se mostrou a abordagem mais eficaz[cite: 146].
* [cite_start]Este modelo demonstrou alta precisão para a classe Charmander (0.93) e alto recall para a classe Bulbasaur (0.93), indicando uma boa capacidade de generalização para classes distintas[cite: 147].
* [cite_start]A fusão de características LBP e VGG16 foi bem-sucedida, fornecendo um vetor de características rico o suficiente para permitir altas taxas de acerto[cite: 148].

**Pontos Fracos**:
* [cite_start]O classificador k-NN obteve o desempenho mais baixo (acurácia de 73.55%), com notável dificuldade na precisão para a classe Charmander (0.59), classificando incorretamente muitos exemplos como sendo desta classe[cite: 150].
* [cite_start]Mesmo no melhor modelo (Árvore de Decisão), a classe Charmander apresentou o menor recall (0.77), sugerindo que ainda é a classe mais difícil de ser completamente identificada[cite: 151].
* [cite_start]A extração das características com a VGG16 é um processo computacionalmente intensivo, representando um gargalo em termos de tempo de execução[cite: 152].

## 6. Conclusão

[cite_start]O sistema desenvolvido foi capaz de classificar imagens de Pokémons da primeira geração com resultados excelentes, destacando-se o modelo de Árvore de Decisão, que atingiu 86.78% de acurácia[cite: 154]. [cite_start]A combinação de descritores de textura e profundos provou ser eficaz, e a otimização de hiperparâmetros foi fundamental para o bom desempenho do ensemble de árvores[cite: 155]. [cite_start]O desempenho inferior do k-NN reforça que, para este espaço de características, as fronteiras de decisão complexas criadas pelas árvores foram mais adequadas que a simples proximidade de vizinhos[cite: 156].