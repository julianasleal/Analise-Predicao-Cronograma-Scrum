# Modelagem Preditiva de Portfólio em Scrum Público

Análise e modelagem preditiva de projetos de engenharia no setor público aplicando metodologia Scrum.

## 📋 Descrição

Este projeto implementa um **framework de modelagem preditiva** desenvolvido para a Comissão Regional de Obras 5 (CRO 5), adaptável a qualquer organização pública que mantenha backlog estruturado. O sistema utiliza **7 algoritmos de machine learning** para prever duração de projetos e distribuição de esforço por fase, validados com técnicas robustas para amostras pequenas.

### Principais Resultados
- ✅ Modelo de **analogia histórica (k-NN, k=3)**: melhor desempenho em previsão de duração (MAE=5,02 sprints; Q²=+0,245)
- ✅ Modelo de **distribuição de esforço por fase (Lasso Multi-output)**: redução de 28% do erro na etapa mais complexa
- ✅ Validação por **leave-one-out cross-validation** — protocolo adequado para datasets pequenos

---

## 🚀 Início Rápido

### Requisitos
- Python 3.8+
- pandas, numpy, scikit-learn, openpyxl
- Google Colab (opcional) ou ambiente local

### Instalação

```bash
# Clone ou baixe o repositório
cd seu-repositorio

# Instale as dependências
pip install pandas numpy scikit-learn openpyxl

# Execute o script
python analise_de_planejamento_cro_5_v1.py
```

### Como Usar

#### Modo Local
1. Coloque o arquivo Excel (`datasetscrum.xlsx`) na mesma pasta do script
2. Execute: `python analise_de_planejamento_cro_5_v1.py`
3. O script detectará e carregará automaticamente

#### Modo Google Colab
1. Acesse o notebook no Colab
2. A interface solicitará upload do arquivo Excel
3. Resultados renderizados automaticamente em gráficos e tabelas

---

## 📁 Estrutura do Projeto

```
.
├── analise_de_planejamento_cro_5_v1.py    # Script principal de análise
├── dashboard_projetos.html                 # Dashboard interativo de saída
├── modelotabela.xlsx                       # Template para novos projetos
├── Artigo-Scrum-v1.docx                    # Artigo acadêmico
├── README.md                               # Este arquivo
│
├── Referências Teóricas/
│   ├── Citadas no artigo
│
└── Dados de Projetos/
    ├── Suprimido
```

---

## 🔧 Funcionalidades Principais

### 1. **Carregamento de Dados**
- Lê arquivos Excel com estrutura padronizada
- Validação automática de colunas (24 colunas esperadas)
- Conversão de tipos: datas, numéricos, categóricos
- Tratamento de valores ausentes com filtragem inteligente

### 2. **Engenharia de Features**
Cria automaticamente 18+ novas variáveis:
- `duracao_dias`: duração do projeto
- `var_pts_pct`: variação percentual de story points
- `idx_eficiencia`: índice de eficiência
- `vel_prevista` e `vel_realizada`: velocidades do time
- Variações percentuais e absolutas por fase (arquitetura, engenharia, orçamento, etc.)

### 3. **Modelagem Preditiva**
Compara 7 algoritmos de machine learning:
- **Ridge Regression**
- **Lasso / ElasticNet**
- **k-Nearest Neighbors (k=3)**
- **Random Forest**
- **Gradient Boosting Regressor**
- **Support Vector Regression (SVR)**
- **Combinações (voting/stacking)**

### 4. **Validação Robusta**
- **Leave-One-Out Cross-Validation (LOO-CV)**: adequado para amostras pequenas
- Métricas: MAE, RMSE, R², Q²
- Análise de importância de features
- Comparação de modelos com visualizações

### 5. **Dashboard Interativo**
Arquivo HTML com gráficos e resumos:
- Distribuição de projetos por tipo e fase
- Desvios de estimativa
- Importância de features por modelo
- Matriz de correlação

---

## 📊 Estrutura de Dados Esperada

### Colunas Obrigatórias (24)
```
id, om, projeto, benfeitoria, tipo_om, tipo_obra, 
area, pts_prev, pts_real, sprint_prev, sprint_exec,
ep_prev, ep_exec, ap_prev, ap_exec, 
pe_arq_prev, pe_arq_exec, pe_eng_prev, pe_eng_exec,
orc_prev, orc_exec, dt_inicio, dt_termino,
arquitetura_reaproveitada
```

---

## 📈 Exemplo de Uso Completo

```python
import pandas as pd
from analise_de_planejamento_cro_5_v1 import (
    carregar_dados,
    engenharia_features,
    preparar_features,
    executar_modelos,
    salvar_resultados
)

# 1. Carregar dados
df = carregar_dados()  # Lê .xlsx automaticamente

# 2. Engenharia de features
df = engenharia_features(df)

# 3. Preparar features para modelos
X, y_targets, encoders = preparar_features(df)

# 4. Executar todos os modelos
resultados = executar_modelos(X, y_targets)

# 5. Salvar e visualizar
salvar_resultados(resultados, df)
```

---

## 🎯 Casos de Uso

### Para Gestores de Portfólio
- **Prever duração realista** de novos projetos baseado em histórico
- **Estimar distribuição de esforço** por fase (arquitetura, engenharia, orçamento)
- **Identificar gargalos** nas etapas mais complexas
- **Otimizar alocação** de recursos

### Para Pesquisadores
- Replicar framework em outras organizações públicas
- Validar metodologia com dados escassos (LOO-CV)
- Comparar performance de modelos em contexto de amostra pequena
- Publicar resultados em periódicos de gestão pública

### Para Analistas de Dados
- Pipeline reproduzível de pré-processamento
- Técnicas de feature engineering aplicadas
- Comparação sistemática de modelos
- Análise de importância de features (permutation importance)

---

## 📚 Referências Académicas

Este trabalho baseia-se em:
- **Mergel et al. (2018)**: Agile government e modernização administrativa
- **Tai & Awasthi (2025)**: Revisão sistemática de governo ágil
- **Shepperd & Schofield (1997)**: Estimativa por analogia
- **Kruskal & Wallis (1952)**: Testes não-paramétricos para amostras pequenas
- **Stone (1974)**: Validação cruzada leave-one-out
- dentre outros.

Veja artigo para referências completas.

---

## 🔬 Metodologia de Validação

### Por que Leave-One-Out (LOO-CV)?
Em contextos de **amostra pequena** (n=14), métodos tradicionais como k-fold podem ser instáveis. LOO-CV:
- Treina n-1 observações, testa em 1
- Repete n vezes
- Minimiza viés de estimativa em amostras reduzidas
- Frequente em pesquisa epidemiológica e científica

### Métricas Reportadas
- **MAE (Mean Absolute Error)**: erro médio em unidades originais
- **RMSE**: penaliza erros maiores
- **R² (coeficiente de determinação)**: proporção de variância explicada
- **Q² (validação cruzada)**: similar a R², mas em dados não-vistos

---

## ⚙️ Configurações Importantes

### Limiares
- **THRESHOLD_ARQ_REAPROV**: mínimo de projetos com arquitetura reaproveitada para ativar LOO (padrão: 3)
- **RANDOM_STATE**: seed para reprodutibilidade (padrão: 42)

### Fases de Projeto (ETAPAS)
```python
{
    "Estudo de Problema": ("ep_prev", "ep_exec"),
    "Arquitetura": ("pe_arq_prev", "pe_arq_exec"),
    "Engenharia": ("pe_eng_prev", "pe_eng_exec"),
    "Orçamento": ("orc_prev", "orc_exec")
}
```

---

## 🐛 Troubleshooting

| Erro | Solução |
|------|---------|
| `FileNotFoundError: No .xlsx found` | Coloque arquivo Excel na pasta do script (modo local) |
| `ValueError: Expected N columns, found M` | Verifique estrutura: deve ter 24+ colunas |
| `NaN in features` | Script filtra automaticamente linhas com pts_prev/pts_real ausentes |
| Colab não encontra arquivo | Use widget de upload (`colab_files.upload()`) |

---

## 💾 Saídas Geradas

O script produz automaticamente:
- ✅ **Tabela de Resultados**: MAE, RMSE, R², Q² para cada modelo
- ✅ **Gráficos**: importância de features, dispersão predito vs real
- ✅ **Arquivo JSON**: resumo de resultados e metadados
- ✅ **Dataset Enriquecido**: dados originais + todas as features criadas

---

## 📝 Licença

Este projeto é resultado de pesquisa realizada em contexto de setor público.

---

## 👥 Autores e Contribuidores

Desenvolvido como framework de modelagem preditiva adaptável a organizações públicas em estágio inicial de digitalização.

---

## 📞 Contato e Suporte

Para dúvidas ou adaptações do framework:
- Consulte o arquivo `Artigo-Scrum-v1.docx` para contexto acadêmico
- Revise `Como_Elaborar_Projetos_de_Pesquisa_-_Antonio_Carlos_Gil_-_6ed.pdf` para metodologia

---

## 🔗 Links Úteis

- **Google Colab Notebook**: [Link ao notebook original](https://colab.research.google.com/drive/1OZNbRmzUL3Fnw82SO1MclF_SW8dJGBOD)
- **Scikit-learn**: https://scikit-learn.org/
- **Pandas Documentation**: https://pandas.pydata.org/
- **PMBOK Guide (8ª ed.)**: Project Management Institute, 2025

---

**Última atualização**: Junho 2026  
**Versão**: 1.0 (stable)
