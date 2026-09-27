# jev_laya_studies

Estudos práticos sobre os modelos Jev e Laya de IA, desenvolvidos pela Typesafe. Este repositório explora a classificação estruturada, explainabilidade e comparação entre abordagens zero-shot e supervisionadas.

## Objetivo do Repositório

Investigar capacidades e limitações de modelos de IA especializados em análise estruturada de texto:
- **Jev**: Modelo zero-shot da Typesafe para respostas estruturadas (Choice, Score, Noul)
- **Laya**: Modelo complementar para análise adicional
- **Comparação**: Jev vs. abordagens tradicionais (Logistic Regression supervisionado)
- **Explainabilidade**: Decomposição de decisões em fatores interpretáveis

## Instalação

### Requisitos
- Python 3.12+
- `uv` package manager (https://docs.astral.sh/uv/getting-started/)

### Setup

```bash
# 1. Clone o repositório
git clone <repo-url>
cd jev_laya_studies

# 2. Instale dependências
uv sync

# 3. Configure variáveis de ambiente
cp .env.example .env
# Adicione sua TYPESAFE_API_KEY ao .env
```

### Rodando o código

**Jupyter Notebooks** (recomendado para exploração):
```bash
uv run jupyter lab
```
Acesse em http://localhost:8888 e abra os notebooks em `notebooks/`

**Scripts Python**:
```bash
uv run python main.py
```

## Notebooks

### 01_jev_introduction.ipynb
**Objetivo**: Introdução ao modelo Jev e seus tipos de resposta

**O que esperar:**
- Carregamento da API Typesafe
- Três tipos de respostas estruturadas:
  - **Choice**: seleção entre opções com probabilidades
  - **Score**: avaliação em escala numérica
  - **Noul**: probabilidade sim/não (0-1)
- Exemplo prático: roteamento de ticket de suporte
- Interpretação de confiança e distribuições de probabilidade

**Duração**: ~5 min, ideal para iniciantes

---

### 02_jev_tweets_classification.ipynb
**Objetivo**: Classificação de tweets sobre desastres naturais

**O que esperar:**
- Dataset: ~7.6k tweets com label (disaster/não-disaster)
- Processamento em lotes (batch_size=32) para eficiência de API
- Comparação de modelos:
  - **Logistic Regression** (supervisionado): 80.1% accuracy, 0.838 ROC-AUC
  - **Jev zero-shot**: 71% accuracy, 0.747 ROC-AUC
- Análise de erros e matriz de confusão
- Otimização de thresholds de decisão
- Visualizações de performance

**Duração**: ~10-15 min, demonstra batch processing

---

### 03_jev_tweets_classification_explainability.ipynb
**Objetivo**: Explicabilidade estruturada para classificações

**O que esperar:**
- Decomposição de decisões em múltiplos fatores
- Consultas separadas ao Jev para diferentes aspectos
- Geração de explicações baseadas em confiança
- Trade-off: explainabilidade vs. métricas de acurácia
- Análise de concordância entre modelo e explicações

**Duração**: ~10 min, demonstra estrutura de análise

---

### 04_jev_titanic_classification.ipynb
**Objetivo**: Análise comparativa profunda - zero-shot vs. supervisionado

**O que esperar:**
- Dataset: Titanic (~800 passengers)
- **Paradigma supervisionado (Logistic Regression)**:
  - One-hot encoding para categorias
  - Standardização de features
  - Treinamento em 80% dos dados
  - Validação em 20%

- **Paradigma zero-shot (Jev)**:
  - Sem dados de treinamento
  - Representação semântica: features numéricas → texto natural
  - Exemplo: "Passageira de 25 anos, classe 1ª, família de 2, tarifa alta"

- **Explicabilidade estruturada** (5 fatores por passageiro):
  - Vantagem demográfica (idade, sexo)
  - Vantagem de classe
  - Vantagem familiar (tamanho da família)
  - Vantagem económica (tarifa paga)
  - Probabilidade geral de sobrevivência

- **Análise de correlação**: relação entre outputs do modelo e fatores explicativos
- Comparação lado-a-lado de performance e interpretabilidade

**Duração**: ~20-25 min, mais complexo, recomendado por último

---

## Conceitos-Chave

### Batch Processing
Múltiplas amostras por chamada de API (batch_size=32) reduz tokens consumidos e otimiza custos.

### Representação Semântica
Conversão de features numéricas para descrição textual natural, permitindo que Jev compreenda contexto sem treinamento prévio.

### Tipos de Resposta Jev

| Tipo | Uso | Exemplo |
|------|-----|---------|
| **Choice** | Categorização com múltiplas opções | Roteamento a departamento |
| **Score** | Avaliação em escala numérica | Nível de urgência (0-2) |
| **Noul** | Probabilidade sim/não | Risco de cancelamento (0-1) |

### Explicabilidade Estruturada
Em vez de uma confiança única, descompõe decisão em múltiplos fatores interpretáveis, cada um com sua própria probabilidade.

### Zero-shot vs. Supervisionado

| Aspecto | Zero-shot (Jev) | Supervisionado (LogReg) |
|---------|-----------------|------------------------|
| Dados de treino | ❌ Não requer | ✅ Requer ~80% dos dados |
| Explicabilidade | ✅ Estruturada nativa | ⚠️ Requer análise adicional |
| Acurácia | ⚠️ Geralmente menor | ✅ Geralmente maior |
| Flexibilidade | ✅ Adaptável a novos domínios | ❌ Específico ao treino |
| Custo inicial | ✅ Sem custo de treino | ❌ Custo computacional |

## Próximos Passos

- Explorar Laya model além de Jev
- Testar em domínios adicionais
- Otimizar prompts semânticos
- Investigar ensemble (combinar Jev + supervisionado)

## Estrutura do Repositório

```
jev_laya_studies/
├── notebooks/
│   ├── 01_jev_introduction.ipynb
│   ├── 02_jev_tweets_classification.ipynb
│   ├── 03_jev_tweets_classification_explainability.ipynb
│   ├── 04_jev_titanic_classification.ipynb
│   ├── disaster_tweets.csv
│   └── titanic_train.csv
├── main.py
├── CLAUDE.md                 # Guia para desenvolvimento
├── .env.example
├── pyproject.toml            # Dependências (uv)
└── README.md                 # Este arquivo
```

## Dependências Principais

- **typesafe-sdk**: SDK para API Jev
- **scikit-learn**: Logistic Regression e preprocessing
- **pandas**: Manipulação de dados
- **matplotlib, seaborn**: Visualizações
- **jupyter, jupyterlab**: Ambiente interativo
- **python-dotenv**: Carregamento de variáveis de ambiente

## Notas

- Cada notebook é **totalmente documentado** com comentários de linha e markdown explicativo
- O arquivo `CLAUDE.md` contém guia técnico para contribuidores
- Notebooks podem ser rodados de forma independente (cada um é autocontido)
- API Typesafe requer `TYPESAFE_API_KEY` válida no `.env`

## Autor

Leonardo Tavares

---

**Última atualização**: 27/09/2026
