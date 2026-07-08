# Desenvolvimento de IA para Análise Preditiva [T1]

**Situação de Aprendizagem (Projeto Avaliativo) — Módulo 1 — Semana 14**

## Sumário

1. [Contextualização](#1-contextualização)
2. [Desafio](#2-desafio)
3. [Resultados Esperados (Entrega)](#3-resultados-esperados-entrega)
4. [Requisitos da Aplicação](#4-requisitos-da-aplicação)
5. [Roteiro da Aplicação](#5-roteiro-da-aplicação)
   - [5.1 Formato do Sistema](#51-formato-do-sistema)
   - [5.2 Documentação no README.md](#52-documentação-no-readmemd)
   - [5.3 Uso do GitHub](#53-uso-do-github)
   - [5.4 Gravação de Vídeo](#54-gravação-de-vídeo)
6. [Critérios de Avaliação](#6-critérios-de-avaliação)
7. [Plano de Projeto](#7-plano-de-projeto)
8. [Sobre o Uso de Inteligência Artificial](#8-sobre-o-uso-de-inteligência-artificial)

---

## 1 Contextualização

O desenvolvimento de modelos de Inteligência Artificial exige a execução das etapas de preparação de dados (*Data Prep*), engenharia de recursos (*Feature Engineering*) e análise exploratória. O treinamento de algoritmos com dados inconsistentes ou desbalanceados causa o fenômeno **Garbage In, Garbage Out** (Lixo Entra, Lixo Sai), gerando modelos com sobreajuste (*overfitting*) que apresentam alto desempenho na fase de treino, mas reduzem a acurácia quando expostos a novos dados.

Neste projeto de encerramento de módulo, você aplicará o fluxo de trabalho de um Cientista de Dados: executará o tratamento estatístico de uma base histórica, construirá uma nova variável numérica, controlará o overfitting por meio do ajuste de hiperparâmetros e selecionará o modelo classificador final com base na métrica de acurácia.

## 2 Desafio

Você atuará como **Cientista de Dados** para estruturar um **Pipeline Preditivo** completo aplicado à Indústria 4.0.

**O Problema:** Um parque fabril monitorado por sensores necessita prever quebras mecânicas nos equipamentos para evitar paradas na linha de produção. A variável alvo do projeto é binária:

- `Falha = 1` → quando há uma avaria detectada
- `Falha = 0` → funcionamento normal

**Link da Base de Dados (.csv):** [Google Drive](https://drive.google.com/drive/folders/1_QcYvhSoJO6SxOJz8Om6gaVuWyPZKdMs?usp=sharing)

Além da base de dados, o engenheiro responsável disponibilizou anotações do Departamento de Engenharia com detalhes importantes a serem considerados.

## 3 Resultados Esperados (Entrega)

- O código deve ser inserido e versionado no **GitHub em modo público**.
- O vídeo deve ser inserido no **Google Drive em modo leitor** para qualquer pessoa com o link.
- Ambos os links devem ser disponibilizados na tarefa **Módulo 1 - Projeto Avaliativo** (semana 13 do AVA) até **17/07/2026 às 22h**.

> ⚠️ **Atenção:**
> - Não serão aceitos projetos submetidos após a data limite, nem alterados depois de entregues.
> - Será considerada como data final de entrega a última atualização no repositório GitHub. Não modifique o código até receber a nota.
> - Links não submetidos no AVA terão penalidade na nota.

## 4 Requisitos da Aplicação

O código Python no Notebook deve ser segmentado e documentado nas seguintes etapas estruturadas (a mesma estrutura pode ser usada em um projeto `.py` com separação em módulos):

### Fase 1: Análise Exploratória (EDA)
- Apresentar dimensões do dataset, tipos de dados e resumo estatístico (`.describe()`).
- Plotar no mínimo **3 gráficos analíticos** (ex: histograma de distribuição, gráfico de barras do desbalanceamento da variável alvo, mapa de calor de correlação de Pearson).
- Incluir célula de texto analisando os padrões identificados e como direcionam a estratégia de modelagem.

### Fase 2: Limpeza e Tratamento de Dados (Data Prep)
- Identificar e remover linhas duplicadas.
- Identificar dados ausentes e aplicar imputação por **Média ou Mediana**, justificando a escolha.
- Gerar **boxplots** para identificar outliers.

### Fase 3: Feature Engineering
- Criar uma nova coluna numérica por operação matemática entre colunas existentes (tratando nulos previamente).
- Sugestão: `potencia = velocidade_rotacao_rpm * torque_nm` (outra combinação é permitida, desde que explicada no vídeo e documentada).

### Fase 4: Divisão e Balanceamento dos Dados
- Separar variáveis preditoras (X) da variável alvo (y).
- Dividir em treino (80%) e teste (20%) com `stratify=y`.
- Aplicar reamostragem (**SMOTE** ou **Random Under Sampling**) exclusivamente no treino, evitando *Data Leakage*.

### Fase 5: Escalonamento de Variáveis (StandardScaler)
- Aplicar `StandardScaler` apenas nas variáveis contínuas do **KNN** (`fit_transform` no treino, `transform` no teste).
- Manter a **Árvore de Decisão** sem escalonamento, justificando o motivo no código.

### Fase 6: Ajuste de Parâmetros e Combate ao Overfitting
- **KNN:** variar `n_neighbors` (K) em ao menos 3 valores ímpares (ex: 3, 5, 7), registrando acurácia de treino e teste.
- **Árvore:** variar `max_depth` em ao menos 3 limites (ex: 3, 5, None), registrando acurácia de treino e teste.
- Texto identificando onde ocorreu overfitting e qual configuração garantiu estabilidade no teste.

### Fase 7: Avaliação da Acurácia e Veredito Final
- Calcular acurácia final do melhor KNN e da melhor árvore no conjunto de teste.
- Comparar os resultados e concluir qual modelo deve ser adotado pela empresa.

## 5 Roteiro da Aplicação

### 5.1 Formato do Sistema

- **Notebook Principal (`.ipynb`):** código completo em um único arquivo, dividido em blocos (cabeçalhos Markdown) que separem as 7 fases.
- **Código limpo e explicativo:** células de texto justificando cada decisão técnica.
- **Reprodutibilidade (`requirements.txt`):** listar todas as dependências com versões.
- **Caminhos relativos de dados:** dados locais devem ficar em uma pasta (ex: `/data`). É proibido usar caminhos absolutos locais (ex: `C:/Usuarios/SeuNome/...`).

### 5.2 Documentação no README.md

O `README.md` do repositório deve conter, entre outros pontos:

* Nome do software;
* Descrição do problema que resolve;
* Técnicas e tecnologias utilizadas (imagens/diagramas são bem-vindos);
* Instruções de execução;
* Melhorias futuras possíveis.

### 5.3 Uso do GitHub

- Commits com descrição sucinta e direta, poucos arquivos alterados por commit.
- Preferir mensagens no imperativo: *"implementa X"* ou *"corrige Y"* (em vez de *"X foi implementado"*).
- Criar uma branch `develop` para concentrar os merges durante o desenvolvimento.
- Utilizar *feature branches* a partir da `develop` para cada tarefa.
- Não excluir branches após o merge do pull request.
- Código final mergeado na branch `main`.

### 5.4 Gravação de Vídeo

Vídeo de **até 7 minutos** abordando:
1. Objetivo do sistema e demonstração de funcionamento.
2. O que é necessário para executar o sistema.
3. Como as tarefas foram organizadas antes do desenvolvimento.
4. Quais branches foram criadas e seus objetivos.
5. O que poderia ter sido melhorado no código.
6. Argumentação clara das escolhas técnicas realizadas.

**Formato:** vertical ou horizontal, com rosto visível e boa iluminação. Publicar no Google Drive em modo leitor e compartilhar o link no AVA (sugestão: incorporar o vídeo no `README.md`).

> 🚫 Não é permitido usar ferramentas de criação de vídeo com IA nesta etapa.

## 6 Critérios de Avaliação

Nota de **0 a 10**, com peso de **60%** sobre a avaliação do módulo. Projetos com plágio (de soluções na internet ou de colegas) recebem nota **0**.

### Apresentação do Projeto

| Nº | Critério | 0 | 2,0 |
|---|---|---|---|
| 1 | Gravação de vídeo | Não foi realizada | Gravou e abordou todos os tópicos do item 5.4 |

### Uso adequado do GitHub e Readme.md

| Nº | Critério | 0 | 0,25 | 0,5 |
|---|---|---|---|---|
| 5 | Branch por etapa + commits claros | Commits diretos na main, sem mensagens claras | Branches por funcionalidade, mas commits misturados ou mensagens inadequadas | Branches por funcionalidade e commits concisos com mensagens adequadas |
| 6 | Documentação README.md | Não criada | Criada de forma muito simplificada | Completa, com todos os tópicos sugeridos |

### Desenvolvimento adequado da Aplicação

| Nº | Critério | 0 | 0,5 | 1,0 |
|---|---|---|---|---|
| 7 | Análise Exploratória (EDA) | Não realizado | Dados/gráficos sem célula interpretativa | Tamanho, describe, 3 gráficos e análise em texto |
| 8 | Limpeza e Tratamento de Dados | Não realizado | Tratamento sem justificativa média/mediana | Duplicados removidos, nulos tratados com justificativa, outliers via boxplot |
| 9 | Feature Engineering | Não realizado | Coluna criada com erros/NaN | Nova coluna validada, sem erros |
| 10 | Divisão e Balanceamento | Não realizado | Split 80/20 sem `stratify` | Split 80/20 com `stratify`, balanceamento só no treino |
| 11 | Escalonamento (StandardScaler) | Não realizado ou idêntico para ambos | `fit_transform` aplicado indevidamente no teste | Aplicado só no KNN corretamente, com justificativa para a Árvore |
| 12 | Ajuste de Parâmetros / Overfitting | Parâmetros não alterados | Alterados, mas avaliados só no treino | 3+ variações por modelo, overfitting apontado no texto |
| 13 | Avaliação da Acurácia / Veredito Final | Acurácia não extraída ou sem conclusão | Acurácias calculadas, mas sem comparação/indicação do modelo | Acurácias comparadas e modelo final indicado |

## 7 Plano de Projeto

Ao construir a aplicação, o estudante desenvolve as seguintes competências técnicas:

- Gerenciamento de repositórios com Git/GitHub (branches e histórico de modificações).
- Execução de pipelines de dados em Python, mitigando *Data Leakage* e assimetria estatística.
- Análise de desempenho de modelos de IA por comparação de métricas de acurácia em dados independentes.

## 8 Sobre o Uso de Inteligência Artificial

> É **vedada** a utilização de ferramentas de IA (ChatGPT, Claude, GitHub Copilot, entre outras) para a **geração e escrita integral do código** deste projeto. O pipeline deve ser construído de forma autônoma pelo estudante.
>
> É **permitido** o uso de IA exclusivamente para consulta teórica, esclarecimento de dúvidas conceituais ou geração de exemplos genéricos de sintaxe. É **proibida** a delegação do desenvolvimento das funções ou blocos do projeto.

