# PreditivIA — Pipeline de Manutenção Preditiva Industrial

Projeto avaliativo do Módulo 1 (Semana 14) — Desenvolvimento de IA para Análise Preditiva.

## 📌 Descrição do Problema

Um parque fabril monitorado por sensores precisa **prever quebras mecânicas em equipamentos** antes que elas ocorram, evitando paradas não planejadas na linha de produção.

O projeto constrói um pipeline completo de Ciência de Dados que classifica cada leitura de sensores em:

- `falha_maquina = 0` → funcionamento normal
- `falha_maquina = 1` → falha detectada

Dois algoritmos de classificação são treinados, ajustados e comparados — **KNN** e **Árvore de Decisão** — para indicar qual deve ser adotado pela empresa.

**Base de dados:** `manutencao_preditiva.csv` (10.000 linhas, 14 colunas), incluindo variáveis como `tipo` (categoria da máquina), `temperatura_ar_k`, `temperatura_processo_k`, `velocidade_rotacao_rpm`, `torque_nm` e `desgaste_ferramenta_min`.

## 🛠️ Técnicas e Tecnologias Utilizadas

**Linguagem e bibliotecas:**
- Python 3
- `pandas` e `numpy` — manipulação de dados
- `matplotlib` e `seaborn` — visualização
- `scikit-learn` — modelagem (KNN, Árvore de Decisão, StandardScaler, métricas)
- `imbalanced-learn` (SMOTE) — balanceamento de classes

# Nome do software: projeto_maquinas

**Etapas do pipeline:**

| Fase | Técnica aplicada |
|---|---|
| 1. Análise Exploratória (EDA) | `.info()`, `.describe()`, gráfico de barras por tipo de máquina, histogramas das variáveis contínuas, gráfico de desbalanceamento da variável alvo, mapa de calor de correlação de Pearson |
| 2. Limpeza e Tratamento | Verificação de duplicados (nenhum encontrado), imputação de nulos por **mediana** (mais robusta a outliers), boxplots para detecção de outliers |
| 3. Feature Engineering | Criação da variável `potencia = velocidade_rotacao_rpm * torque_nm` |
| 4. Divisão e Balanceamento | `train_test_split` 80/20 com `stratify=y` e `random_state=42`; balanceamento via **SMOTE** aplicado **somente no treino** |
| 5. Escalonamento | `StandardScaler` (`fit_transform` no treino / `transform` no teste) aplicado apenas nas variáveis contínuas usadas pelo **KNN**; Árvore de Decisão mantida sem escalonamento |
| 6. Ajuste de Hiperparâmetros | KNN: `n_neighbors` = 3, 5, 7 — Árvore: `max_depth` = 3, 5, None |
| 7. Avaliação Final | Acurácia + **matriz de confusão** para decidir o modelo final |
| 8. Investigação da MULTICOLINEARIDADE

## 📊 Principais Descobertas da Análise Exploratória

- O dataset é fortemente **desbalanceado**: cerca de 96,6% dos registros são "sem falha" e apenas 3,4% "com falha" — por isso a acurácia isolada não é suficiente para avaliar os modelos.
- A variável `velocidade_rotacao_rpm` apresenta assimetria positiva e concentra a maior parte dos outliers identificados nos boxplots.
- Há forte correlação negativa (-0,88) entre `velocidade_rotacao_rpm` e `torque_nm`, e forte correlação positiva (0,88) entre `temperatura_ar_k` e `temperatura_processo_k` — possíveis sinais de multicolinearidade.
- Os outliers de `torque_nm` e `velocidade_rotacao_rpm` foram **mantidos**, pois concentram uma taxa de falha proporcionalmente maior — remover essas linhas descartaria sinal relevante para o modelo.
- Isoladamente, nenhuma variável tem correlação forte com `falha_maquina` (a maior é `torque_nm`, com 0,19), indicando que a falha resulta da **combinação** de fatores — cenário em que modelos como KNN e Árvore de Decisão, que capturam interações não lineares, são adequados.

## 🏆 Resultado e Modelo Final

| Modelo | Configuração | Acurácia (teste) | Falsos Negativos |
|---|---|---|---|
| KNN | `n_neighbors=3` | 90,50% | 24 |
| Árvore de Decisão | `max_depth=5` | — | **12** |

Apesar de o KNN apresentar acurácia bruta ligeiramente superior, a **matriz de confusão** mostra que ele comete o dobro de Falsos Negativos em relação à Árvore de Decisão (24 contra 12). Como o objetivo do sistema é detectar falhas reais — e deixar uma falha passar despercebida (falso negativo) é muito mais custoso do que um alarme falso — **a Árvore de Decisão (`max_depth=5`) foi escolhida como modelo final**, por errar menos no ponto mais crítico do problema.

## ▶️ Instruções de Execução

1. Clone o repositório:
   ```bash
   git clone <url-do-repositorio>
   cd <nome-do-repositorio>
   ```
2. Crie um ambiente virtual (opcional, mas recomendado):
   ```bash
   python -m venv venv
   source venv/bin/activate  # Windows: venv\Scripts\activate
   ```
3. Instale as dependências:
   ```bash
   pip install -r requirements.txt
   ```
4. Certifique-se de que o arquivo `manutencao_preditiva.csv` esteja no caminho esperado pelo notebook (recomenda-se a pasta `/data`).
5. Abra e execute o notebook `projeto_maquinas.ipynb` célula a célula, em ordem, no Jupyter Notebook ou VS Code.

## 🔭 Melhorias Futuras

- Testar outros algoritmos de classificação (Random Forest, XGBoost, LightGBM) para comparação de desempenho.
- Investigar a multicolinearidade entre `velocidade_rotacao_rpm`/`torque_nm` e `temperatura_ar_k`/`temperatura_processo_k`, avaliando remoção ou combinação dessas variáveis.
- Testar outras técnicas de balanceamento (ex: SMOTE-Tomek, ADASYN) e comparar o impacto no número de falsos negativos.
- Persistir o modelo final (ex: `joblib`/`pickle`) e disponibilizar uma API simples para predições em tempo real.

## 📁 Estrutura do Projeto

```
├── data/
│   └── manutencao_preditiva.csv
├── projeto_maquinas.ipynb
├── requirements.txt
└── README.md
```
