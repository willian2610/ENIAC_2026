# ENIAC 2026 - 

Este repositório contém os experimentos desenvolvidos para o trabalho submetido ao ENIAC 2026.

O objetivo é avaliar o comportamento de um sistema Retrieval-Augmented Generation (RAG) aplicado à literatura médica, comparando diferentes estratégias de prompt e analisando o alinhamento entre as respostas geradas e os documentos recuperados.

O pipeline utiliza recuperação vetorial baseada em embeddings, armazenamento em ChromaDB e geração de respostas por meio do modelo Phi-3 executado localmente via Ollama.

---

## Pipeline Experimental

```text
PDFs
  ↓
Extração de Texto (PyMuPDF)
  ↓
Limpeza de Conteúdo
  ↓
Chunking por Parágrafos
  ↓
Embeddings (all-mpnet-base-v2)
  ↓
ChromaDB
  ↓
Similarity Retrieval
  ↓
Phi-3 (Ollama)
  ↓
Avaliação das Respostas
  ↓
Análise de Similaridade do Cosseno
```

---

## Preparação do Ambiente


### Instalação do Ollama

Instale o Ollama:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Baixe o modelo utilizado nos experimentos:

```bash
ollama pull phi3
```

---

## Base Documental

Os documentos devem ser armazenados no diretório:

```text
sources/
```

Durante a execução:

1. Os PDFs são carregados.
2. O texto é extraído página por página.
3. Seções de referências são removidas.
4. O conteúdo é dividido em chunks.
5. Os embeddings são gerados.
6. Os vetores são persistidos em ChromaDB.

---

## Estratégias de Prompt Avaliadas

### Prompt Restritivo

Características:

* Utiliza apenas o contexto recuperado.
* Não permite conhecimento externo.
* Não permite inferências.
* Responde apenas quando há evidência documental.

Objetivo:

Reduzir alucinações e aumentar a rastreabilidade das respostas.

---

### Prompt Não Restritivo

Características:

* Prioriza o contexto recuperado.
* Permite uso de conhecimento prévio do modelo.

Objetivo:

Avaliar o impacto do conhecimento paramétrico do LLM na qualidade das respostas.

---

## Conjunto de Perguntas

As perguntas foram divididas em quatro categorias:

| Categoria         | Descrição                                     |
| ----------------- | --------------------------------------------- |
| Present           | Informação totalmente presente nos documentos |
| Partially Present | Informação parcialmente presente              |
| Absent            | Informação ausente nos documentos             |
| Out of Context    | Perguntas sem relação com o domínio médico    |

---

## Avaliação

Para cada pergunta são registrados:

* Resposta gerada
* Chunks recuperados
* Fontes utilizadas
* Tempo de execução
* Avaliação manual
* Métricas de similaridade do cosseno

---

## Similaridade do Cosseno

A avaliação quantitativa utiliza embeddings para calcular:

* Similaridade máxima
* Similaridade média
* Similaridade mínima
* Distribuição dos scores entre resposta e chunks recuperados

A métrica permite analisar o grau de alinhamento entre a resposta produzida e o conteúdo efetivamente recuperado pelo sistema.

---

## Resultados Gerados

Os experimentos geram automaticamente:

```text
results/
└── run_YYYYMMDD_HHMMSS/
    ├── prompt_restritivo/
    │   ├── resultados_prompt_restritivo.csv
    │   ├── avaliacao_similaridade_cosseno_prompt_restritivo.csv
    │   ├── heatmap_prompt_restritivo.png
    │   └── cosine_distribution_prompt_restritivo.png
    │
    └── prompt_nao_restritivo/
        ├── resultados_prompt_nao_restritivo.csv
        ├── avaliacao_similaridade_cosseno_prompt_nao_restritivo.csv
        ├── heatmap_prompt_nao_restritivo.png
        └── cosine_distribution_prompt_nao_restritivo.png
```

---

## Visualizações

O projeto gera automaticamente:

### Heatmaps

Mostram quais chunks apresentaram maior similaridade com cada resposta.

### Distribuições de Similaridade

Utilizam violin plots para representar a distribuição dos scores de similaridade do cosseno.

---

## Reprodutibilidade

Os experimentos foram desenvolvidos para permitir reprodução completa dos resultados mediante:

* Mesma coleção de documentos PDF.
* Mesmo modelo de embeddings.
* Mesmo modelo LLM (Phi-3).
* Mesmos prompts.
* Mesmo conjunto de perguntas.

---

## Autor do Código

Willian de Vargas (Mestrando do Programa de Pós-Graduação em Tecnologias da Informação e Gestão em Saúde da Universidade Federal de Ciências da Saúde de Porto Alegre)

