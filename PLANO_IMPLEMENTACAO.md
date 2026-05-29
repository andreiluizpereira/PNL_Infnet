# Plano de Implementacao do Projeto Final de PLN

## 1. Visao geral do projeto

**Disciplina:** Processamento de Linguagem Natural  
**Tema proposto:** Analise de avaliacoes de clientes da Olist usando tecnicas de PLN  
**Dataset:** `Data/olist_order_reviews_dataset.csv`  
**Formato do desenvolvimento:** notebooks Python, com foco em simplicidade, reproducibilidade e interpretacao academica.

O projeto vai construir um pipeline completo de PLN a partir das avaliacoes textuais da Olist. O objetivo sera transformar os comentarios brutos dos clientes em informacoes uteis sobre satisfacao, temas recorrentes, termos relevantes, similaridade textual, entidades/termos de dominio e relacoes simples representadas em grafo.

Como a base possui notas de avaliacao (`review_score`), o trabalho pode tratar a nota como rotulo supervisionado. Para simplificar a modelagem, a classificacao principal sera formulada como:

- **negativo:** notas 1 e 2;
- **neutro:** nota 3;
- **positivo:** notas 4 e 5.

Tambem sera possivel testar uma versao binaria mais simples:

- **negativo:** notas 1 e 2;
- **positivo:** notas 4 e 5;
- nota 3 removida da classificacao binaria.

## 2. Estrutura sugerida de pastas

```text
PNL_Infnet/
|-- Data/
|   |-- olist_order_reviews_dataset.csv
|-- notebooks/
|   |-- 00_setup_e_inspecao.ipynb
|   |-- 01_preprocessamento.ipynb
|   |-- 02_representacao_e_busca.ipynb
|   |-- 03_modelagem_classificacao_topicos.ipynb
|   |-- 04_ner_grafo_conhecimento.ipynb
|   |-- 99_pipeline_end_to_end.ipynb
|-- outputs/
|   |-- figures/
|   |-- models/
|   |-- tables/
|   |-- graphs/
|-- requirements.txt
|-- README.md
|-- PLANO_IMPLEMENTACAO.md
```

Para a entrega final, o arquivo mais importante sera o `99_pipeline_end_to_end.ipynb`, contendo o fluxo completo. Os demais notebooks podem ser usados durante o desenvolvimento para organizar melhor as etapas e reduzir a complexidade.

## 3. Bibliotecas principais

Usar bibliotecas conhecidas e simples:

- `pandas` e `numpy` para manipulacao dos dados;
- `matplotlib` e `seaborn` para graficos;
- `nltk` para tokenizacao, stopwords e stemming;
- `spacy` para lematizacao, POS tagging e NER;
- `scikit-learn` para vetorizacao, classificacao, metricas, LSA/NMF e busca por similaridade;
- `gensim` para Word2Vec, se for incluido;
- `wordcloud` para nuvem de palavras;
- `networkx` para grafo de conhecimento;
- `pyvis`, opcionalmente, para visualizacao interativa do grafo.

Modelo spaCy sugerido:

```bash
python -m spacy download pt_core_news_sm
```

## 4. Plano de desenvolvimento por etapas

### Etapa 1: Preparacao do ambiente

Objetivo: deixar o projeto reprodutivel.

Passos:

1. Criar a pasta `notebooks/`.
2. Criar a pasta `outputs/` com subpastas para figuras, tabelas, modelos e grafos.
3. Criar `requirements.txt` com as dependencias usadas.
4. Registrar no `README.md` as instrucoes basicas de execucao.
5. Confirmar que o dataset esta em `Data/olist_order_reviews_dataset.csv`.

Resultado esperado:

- ambiente organizado;
- instrucoes simples para reproduzir o trabalho;
- dependencias declaradas.

### Etapa 2: Carregamento e inspecao inicial do corpus

Notebook sugerido: `00_setup_e_inspecao.ipynb`

Passos:

1. Carregar o CSV com `pandas`.
2. Verificar quantidade de linhas e colunas.
3. Identificar campos disponiveis:
   - `review_id`;
   - `order_id`;
   - `review_score`;
   - `review_comment_title`;
   - `review_comment_message`;
   - `review_creation_date`;
   - `review_answer_timestamp`.
4. Criar uma coluna textual combinando titulo e comentario:

```python
df["texto"] = (
    df["review_comment_title"].fillna("") + " " +
    df["review_comment_message"].fillna("")
).str.strip()
```

5. Remover registros sem texto.
6. Calcular estatisticas iniciais:
   - total de avaliacoes;
   - total de avaliacoes com texto;
   - distribuicao das notas;
   - tamanho medio dos comentarios em caracteres, palavras e sentencas;
   - quantidade de comentarios por ano/mes.
7. Criar o rotulo de sentimento/satisfacao com base na nota.

Resultado esperado:

- dataframe limpo para as proximas etapas;
- primeira caracterizacao do corpus;
- graficos iniciais sobre notas e tamanho dos textos.

Visualizacoes:

- grafico de barras da distribuicao das notas;
- histograma do tamanho dos comentarios;
- grafico de barras da quantidade de documentos por classe.

### Etapa 3: Pre-processamento textual com NLTK e spaCy

Notebook sugerido: `01_preprocessamento.ipynb`

Objetivo: construir e comparar estrategias simples de tratamento textual.

Passos:

1. Normalizar o texto:
   - converter para minusculas;
   - remover excesso de espacos;
   - remover URLs, emails e caracteres nao informativos;
   - preservar palavras importantes do dominio.
2. Tokenizar palavras e sentencas com NLTK.
3. Remover stopwords em portugues.
4. Criar uma lista pequena de stopwords customizadas, por exemplo:
   - `produto`;
   - `compra`;
   - `pedido`;
   - `loja`;
   - apenas se aparecerem como termos excessivamente genericos.
5. Aplicar stemming com `RSLPStemmer` do NLTK.
6. Aplicar lematizacao com spaCy.
7. Aplicar POS tagging com spaCy.
8. Comparar os vocabulos gerados por:
   - texto original normalizado;
   - texto com stemming;
   - texto com lematizacao.
9. Escolher a representacao principal para as etapas seguintes.

Decisao tecnica sugerida:

- usar lematizacao como representacao principal por ser mais interpretavel;
- manter stemming apenas como comparacao obrigatoria;
- usar uma versao simples normalizada para modelos supervisionados se ela tiver desempenho melhor.

Resultado esperado:

- colunas com texto limpo, tokens, stems e lemas;
- comparacao do tamanho do vocabulario;
- justificativa da estrategia escolhida.

Visualizacoes:

- termos mais frequentes antes e depois do pre-processamento;
- nuvem de palavras;
- distribuicao de POS tags;
- comparacao do tamanho do vocabulario entre estrategias.

### Etapa 4: Representacao vetorial e busca textual

Notebook sugerido: `02_representacao_e_busca.ipynb`

Objetivo: transformar textos em vetores e criar um buscador simples por similaridade.

Passos:

1. Criar uma representacao Bag-of-Words com `CountVectorizer`.
2. Criar uma representacao TF-IDF com `TfidfVectorizer`.
3. Testar unigramas e bigramas com `ngram_range=(1, 2)`.
4. Calcular similaridade por cosseno usando os vetores TF-IDF.
5. Implementar uma funcao simples de busca:

```python
def buscar_documentos(consulta, vectorizer, matriz_tfidf, df, top_n=5):
    consulta_vec = vectorizer.transform([consulta])
    scores = cosine_similarity(consulta_vec, matriz_tfidf).ravel()
    indices = scores.argsort()[::-1][:top_n]
    return df.iloc[indices][["review_score", "sentimento", "texto"]].assign(score=scores[indices])
```

6. Testar pelo menos 3 consultas:
   - `"entrega atrasada"`;
   - `"produto chegou quebrado"`;
   - `"recebi antes do prazo"`.
7. Comentar se os documentos retornados fazem sentido.
8. Opcionalmente, treinar Word2Vec com Gensim em uma amostra ou no corpus completo.
9. Opcionalmente, visualizar embeddings com t-SNE usando uma amostra pequena de termos.

Resultado esperado:

- vetores BoW e TF-IDF;
- mecanismo de busca textual funcionando;
- exemplos de consultas com resultados interpretados.

Visualizacoes:

- top termos por TF-IDF;
- heatmap pequeno de similaridade entre documentos;
- t-SNE opcional de termos ou documentos.

### Etapa 5: Modelagem, classificacao e topicos

Notebook sugerido: `03_modelagem_classificacao_topicos.ipynb`

Objetivo: extrair significado estruturado usando classificacao supervisionada e modelagem de topicos.

Frente 1: classificacao supervisionada

Passos:

1. Definir `X` como texto pre-processado.
2. Definir `y` como sentimento derivado da nota.
3. Separar treino e teste com `train_test_split`, usando estratificacao.
4. Criar pipeline com `TfidfVectorizer` e classificador.
5. Treinar modelos simples:
   - Naive Bayes;
   - Regressao Logistica;
   - SVM Linear, se o tempo permitir.
6. Tratar desbalanceamento com `class_weight="balanced"` nos modelos que suportam essa opcao.
7. Avaliar com:
   - accuracy;
   - precision;
   - recall;
   - F1-score;
   - matriz de confusao.
8. Comparar os resultados em uma tabela.

Decisao tecnica sugerida:

- manter Regressao Logistica como modelo principal se tiver bom equilibrio entre simplicidade, desempenho e interpretabilidade;
- usar Naive Bayes como baseline.

Frente 2: modelagem de topicos

Passos:

1. Usar TF-IDF ou CountVectorizer.
2. Aplicar NMF como tecnica principal de topicos, por ser simples e funcionar bem com TF-IDF.
3. Testar entre 5 e 8 topicos.
4. Listar os principais termos de cada topico.
5. Atribuir um nome interpretativo para cada topico.
6. Comparar topicos por classe de sentimento, quando fizer sentido.

Tecnica opcional:

- LSA com `TruncatedSVD` para comparacao simples.

Resultado esperado:

- pelo menos dois modelos supervisionados comparados;
- pelo menos uma tecnica de topicos aplicada;
- metricas e interpretacao dos resultados.

Visualizacoes:

- matriz de confusao;
- tabela de metricas;
- barras com termos mais importantes por topico;
- distribuicao de topicos por sentimento.

### Etapa 6: NER, extracao de informacao e grafo de conhecimento

Notebook sugerido: `04_ner_grafo_conhecimento.ipynb`

Objetivo: extrair entidades, termos ou padroes e representar relacoes em um grafo simples.

Passos:

1. Aplicar NER com spaCy em uma amostra de comentarios.
2. Extrair entidades reconhecidas, como:
   - organizacoes;
   - locais;
   - datas;
   - pessoas, se existirem.
3. Como reviews curtos podem ter poucas entidades nomeadas, complementar com termos de dominio extraidos por regras simples:
   - `entrega`;
   - `prazo`;
   - `produto`;
   - `atraso`;
   - `embalagem`;
   - `qualidade`;
   - `defeito`;
   - `troca`;
   - `reembolso`;
   - `atendimento`.
4. Usar regex para extrair padroes simples:
   - datas;
   - numeros;
   - mencoes a prazo;
   - termos relacionados a atraso ou recebimento.
5. Opcionalmente, usar distancia de Levenshtein para aproximar variacoes como:
   - `entrega`, `entregue`, `entregaram`;
   - `atraso`, `atrasado`, `atrasada`.
6. Construir um grafo com NetworkX:
   - nos: classes de sentimento, termos de dominio e entidades;
   - arestas: coocorrencia entre sentimento e termo, ou entre termos no mesmo comentario.
7. Garantir pelo menos 20 nos.
8. Calcular centralidade:
   - grau;
   - betweenness, se o grafo nao ficar grande demais.
9. Visualizar o grafo com NetworkX ou PyVis.
10. Responder uma pergunta analitica usando o grafo, por exemplo:
    - "Quais termos aparecem mais conectados a avaliacoes negativas?"
    - "Quais problemas concentram mais relacoes nos comentarios?"

Resultado esperado:

- entidades e termos extraidos;
- grafo com pelo menos 20 nos;
- ranking de nos mais centrais;
- interpretacao das relacoes mais importantes.

Visualizacoes:

- tabela de entidades/termos mais frequentes;
- grafo de conhecimento;
- barras com centralidade dos principais nos.

### Etapa 7: Notebook principal end-to-end

Notebook final: `99_pipeline_end_to_end.ipynb`

Objetivo: consolidar o trabalho em um unico notebook executavel do inicio ao fim.

Ordem sugerida do notebook:

1. Titulo, objetivo e descricao curta do corpus.
2. Importacao das bibliotecas.
3. Carregamento dos dados.
4. Inspecao inicial.
5. Preparacao da coluna textual.
6. Criacao dos rotulos.
7. Pre-processamento.
8. Comparacao stemming versus lematizacao.
9. Visualizacoes exploratorias.
10. Vetorizacao BoW e TF-IDF.
11. Busca textual por similaridade com 3 consultas.
12. Classificacao supervisionada.
13. Modelagem de topicos.
14. NER e extracao de termos.
15. Grafo de conhecimento.
16. Sintese dos resultados.
17. Limitacoes.
18. Possiveis melhorias.
19. Instrucoes de reproducao.

Para manter simplicidade, o notebook final deve evitar funcoes muito complexas. O ideal e usar poucas funcoes utilitarias apenas quando elas reduzem repeticao, por exemplo:

- funcao de limpeza textual;
- funcao de busca por similaridade;
- funcao para exibir termos principais dos topicos;
- funcao para extrair termos de dominio.

### Etapa 8: Relatorio tecnico em PDF

Objetivo: transformar os resultados do notebook em uma explicacao clara.

Estrutura sugerida:

1. Capa:
   - nome do aluno;
   - disciplina;
   - titulo do projeto.
2. Introducao:
   - problema;
   - objetivo;
   - justificativa do corpus.
3. Descricao do dataset:
   - origem;
   - campos usados;
   - quantidade de documentos;
   - distribuicao das notas;
   - limitacoes dos comentarios curtos.
4. Metodologia:
   - pre-processamento;
   - stemming versus lematizacao;
   - representacoes vetoriais;
   - busca textual;
   - classificacao;
   - topicos;
   - NER;
   - grafo.
5. Resultados:
   - principais graficos;
   - metricas dos modelos;
   - exemplos de busca;
   - topicos encontrados;
   - entidades e termos relevantes;
   - interpretacao do grafo.
6. Sintese nao tecnica:
   - principais achados em linguagem acessivel;
   - implicacoes praticas.
7. Limitacoes:
   - comentarios curtos;
   - poucas entidades nomeadas formais;
   - rotulo de sentimento inferido pela nota;
   - ausencia de outras tabelas da Olist neste dataset local.
8. Melhorias futuras:
   - usar outras tabelas da Olist, como pedidos, produtos e vendedores;
   - testar modelos de linguagem pre-treinados;
   - criar anotacao manual para NER customizado;
   - fazer ajuste de hiperparametros.
9. Reproducibilidade:
   - versao do Python;
   - dependencias;
   - caminho do dataset;
   - ordem de execucao dos notebooks.

Nome do PDF:

```text
andrei_pereira_sistemas-cognitivos-linguagem-natural_pln.pdf
```

## 5. Cronograma sugerido

| Fase | Atividade | Entrega parcial |
|---|---|---|
| 1 | Organizacao do ambiente e leitura do dataset | pastas, requirements e notebook 00 |
| 2 | EDA e pre-processamento | notebook 01 com graficos iniciais |
| 3 | Vetorizacao e busca textual | notebook 02 com consultas testadas |
| 4 | Classificacao e topicos | notebook 03 com metricas e topicos |
| 5 | NER, extracao e grafo | notebook 04 com grafo e centralidade |
| 6 | Consolidacao | notebook 99 end-to-end |
| 7 | Escrita final | relatorio tecnico em PDF |

## 6. Checklist dos requisitos do enunciado

### Requisito 1: Pre-processamento textual

- [ ] carregamento e inspecao inicial do corpus;
- [ ] tokenizacao de palavras;
- [ ] tokenizacao de sentencas;
- [ ] normalizacao textual;
- [ ] remocao de stopwords;
- [ ] stopwords customizadas;
- [ ] comparacao entre stemming e lematizacao;
- [ ] POS tagging com spaCy;
- [ ] analise do impacto no vocabulario;
- [ ] visualizacoes de apoio.

### Requisito 2: Representacao vetorial e busca textual

- [ ] CountVectorizer;
- [ ] TF-IDF;
- [ ] n-gramas;
- [ ] similaridade por cosseno;
- [ ] mecanismo de busca textual;
- [ ] 3 consultas testadas e comentadas.

### Requisito 3: Modelagem, classificacao ou sentimento

- [ ] classificacao com Naive Bayes;
- [ ] classificacao com Regressao Logistica ou SVM;
- [ ] tratamento de desbalanceamento;
- [ ] precision, recall, F1 e matriz de confusao;
- [ ] modelagem de topicos com NMF ou LSA;
- [ ] analise escrita dos resultados.

### Requisito 4: NER, extracao e grafo

- [ ] NER com spaCy;
- [ ] extracao com regex ou regras de dominio;
- [ ] termos de dominio extraidos;
- [ ] grafo com NetworkX;
- [ ] pelo menos 20 nos;
- [ ] calculo de centralidade;
- [ ] visualizacao do grafo;
- [ ] resposta a pergunta analitica usando o grafo.

### Requisito 5: Visualizacao, comunicacao e reproducibilidade

- [ ] notebook principal end-to-end;
- [ ] graficos salvos em `outputs/figures/`;
- [ ] tabelas ou metricas salvas em `outputs/tables/`;
- [ ] grafo salvo em `outputs/graphs/`;
- [ ] sintese nao tecnica;
- [ ] limitacoes;
- [ ] instrucoes de reproducao.

## 7. Decisoes para manter o projeto simples

- Usar o dataset local ja disponivel na pasta `Data`.
- Tratar `review_score` como rotulo principal.
- Priorizar TF-IDF para busca e classificacao.
- Usar Naive Bayes como baseline e Regressao Logistica como modelo principal.
- Usar NMF para topicos, pois e simples e interpretavel com TF-IDF.
- Usar regras de dominio junto com NER, porque avaliacoes curtas podem nao conter muitas entidades nomeadas tradicionais.
- Construir um grafo por coocorrencia de termos e sentimentos, evitando extracao linguistica complexa de relacoes.
- Consolidar tudo em um notebook final end-to-end para facilitar a avaliacao.

## 8. Riscos e mitigacoes

| Risco | Impacto | Mitigacao |
|---|---|---|
| Muitos comentarios vazios | reduz o corpus textual util | filtrar apenas registros com comentario ou titulo |
| Comentarios curtos | limita analise semantica profunda | usar volume alto de documentos e interpretar essa limitacao |
| Classes desbalanceadas | prejudica classificacao | usar estratificacao, `class_weight` e metricas por classe |
| Poucas entidades nomeadas | enfraquece NER tradicional | complementar com termos de dominio e regex |
| Notebook muito pesado | dificulta execucao | usar amostras controladas em spaCy, NER, t-SNE e grafo |
| Dependencias externas | dificulta reproducao | registrar tudo em `requirements.txt` e documentar modelo spaCy |

## 9. Resultado final esperado

Ao final, o projeto devera entregar:

1. Um notebook principal end-to-end com o pipeline completo.
2. Notebooks auxiliares com o desenvolvimento por etapa, se forem mantidos.
3. Figuras, tabelas e grafo exportados em `outputs/`.
4. Um relatorio tecnico em PDF.
5. Instrucoes claras de reproducao no `README.md`.

O resultado deve demonstrar que o corpus bruto foi transformado em informacoes interpretaveis sobre a experiencia dos clientes, incluindo problemas recorrentes, termos associados a avaliacoes negativas e positivas, desempenho de modelos de classificacao e uma visao relacional simples por meio do grafo de conhecimento.
