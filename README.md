# PNL_Infnet

Projeto final da disciplina Processamento de Linguagem Natural - Infnet.

## Tema

Pipeline de PLN para analise de avaliacoes de clientes da Olist.

O dataset usado no projeto esta em:

```text
Data/olist_order_reviews_dataset.csv
```

## Ambiente virtual

Ative o ambiente virtual local:

```powershell
.\.venv\Scripts\Activate.ps1
```

Instale as dependencias:

```powershell
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m spacy download pt_core_news_sm
python -m ipykernel install --user --name pnl-infnet --display-name "Python (PNL Infnet)"
```

## Estrutura principal

```text
notebooks/
|-- 99_pipeline_end_to_end.ipynb

outputs/
|-- figures/
|-- graphs/
|-- models/
|-- tables/
```

## Execucao

Abra o notebook principal e execute as celulas em ordem:

```text
notebooks/99_pipeline_end_to_end.ipynb
```

O notebook carrega o dataset, prepara os textos, compara estrategias de pre-processamento, cria vetores TF-IDF, executa busca textual, treina classificadores, aplica modelagem de topicos, extrai termos/entidades e constroi um grafo de conhecimento simples.
