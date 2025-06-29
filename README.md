## Documentação dos Parâmetros de Extração de Características

- **LBP (Local Binary Pattern):**
  - `P=8`: número de vizinhos.
  - `R=1`: raio.
  - `method='nri_uniform'`: método de codificação.

- **Deep Features (VGG16):**
  - `target_size=(224, 224)`: tamanho da imagem.
  - `pca_components=50`: componentes principais para redução de dimensionalidade (PCA).

- **Pipeline de Extração:**
  - Combinação dos vetores LBP e VGG16 para cada imagem.
  - Normalização (StandardScaler), balanceamento (SMOTE) e redução de dimensionalidade (PCA) aplicados antes do treinamento dos classificadores.

---

## Análise de Desempenho dos Classificadores

### k-NN
- **Melhores parâmetros:** `n_neighbors=5`, `weights='distance'`, `metric='cosine'`, `pca__n_components=0.9`
- **F1-score médio (validação cruzada):** 0.6926 (±0.0404)
- **Acurácia média (validação cruzada):** 0.7031 (±0.0420)
- **Acurácia no teste:** 0.6860

**Relatório de Classificação (Teste):**
| Classe      | Precision | Recall | F1-score | Suporte |
|-------------|-----------|--------|----------|---------|
| Bulbasaur   | 0.94      | 0.76   | 0.84     | 41      |
| Charmander  | 0.55      | 0.74   | 0.63     | 35      |
| Squirtle    | 0.63      | 0.58   | 0.60     | 45      |
| **Média**   | 0.71      | 0.69   | 0.69     | 121     |

---

### SVM
- **Melhores parâmetros:** `C=14.17`, `gamma=0.022`, `kernel='rbf'`, `degree=2`, `class_weight='balanced'`, `pca__n_components=100`
- **F1-score médio (validação cruzada):** 0.9130 (±0.0445)
- **Acurácia no teste:** 0.82

**Relatório de Classificação (Teste):**
| Classe      | Precision | Recall | F1-score | Suporte |
|-------------|-----------|--------|----------|---------|
| Bulbasaur   | 0.80      | 0.90   | 0.85     | 41      |
| Charmander  | 0.81      | 0.74   | 0.78     | 35      |
| Squirtle    | 0.84      | 0.80   | 0.82     | 45      |
| **Média**   | 0.82      | 0.82   | 0.81     | 121     |

---

### Árvore de Decisão (com Bagging)
- **Melhores parâmetros:** `max_depth=10`, `criterion='gini'`, `min_samples_leaf=2`, `max_features='sqrt'`
- **F1-score médio (validação cruzada):** 0.8573 (±0.0411)
- **Acurácia no teste:** 0.76

**Relatório de Classificação (Teste):**
| Classe      | Precision | Recall | F1-score | Suporte |
|-------------|-----------|--------|----------|---------|
| Bulbasaur   | 0.84      | 0.90   | 0.87     | 41      |
| Charmander  | 0.63      | 0.69   | 0.66     | 35      |
| Squirtle    | 0.79      | 0.69   | 0.74     | 45      |
| **Média**   | 0.76      | 0.76   | 0.76     | 121     |

---

## Pontos Fortes e Fracos do Sistema

**Pontos Fortes:**
- O uso combinado de descritores LBP e VGG16 permite capturar tanto texturas quanto padrões de alto nível.
- O pipeline de pré-processamento (normalização, SMOTE, PCA) melhora a robustez e a generalização.
- O SVM apresentou o melhor desempenho geral, com alta acurácia e F1-score, mostrando boa capacidade de generalização.

**Pontos Fracos:**
- O k-NN teve desempenho inferior, especialmente para a classe Charmander, indicando sensibilidade a desbalanceamento e sobreposição de classes.
- A árvore de decisão, mesmo com bagging, apresentou recall menor para Squirtle e Charmander, sugerindo dificuldade em separar essas classes.
- O tempo de extração de características profundas (VGG16) é elevado, tornando o processamento mais demorado.
- O sistema depende da qualidade das imagens e pode ser sensível a variações de iluminação e ruído.

---

**Resumo:**  
O sistema é eficiente para classificação de personagens, principalmente com SVM, mas pode ser aprimorado para lidar melhor com classes menos representadas e cenários de imagens mais desafiadores.