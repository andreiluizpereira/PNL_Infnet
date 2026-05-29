Olá, Andrei.

Este projeto consolida, na prática, os conhecimentos desenvolvidos na disciplina **Processamento de Linguagem Natural**.

Ao longo da disciplina, você aprendeu a construir pipelines de PLN com Python, NLTK e spaCy, aplicar técnicas de pré-processamento textual, representar textos numericamente com TF-IDF e word embeddings, treinar modelos de classificação, aplicar modelagem de tópicos, extrair entidades nomeadas, construir grafos de conhecimento e comunicar resultados de análise textual.

Agora, você deverá transformar esses conhecimentos em uma solução aplicada, construída a partir de um corpus textual público ou de uma base própria anonimizada.

### Contexto

Profissionais que trabalham com dados textuais enfrentam diariamente o desafio de transformar linguagem não estruturada em informação utilizável. Isso acontece em contextos como classificação de chamados de suporte, análise de avaliações de clientes, extração de entidades em documentos, identificação de temas em notícias, triagem de reclamações, análise de sentimento e organização de grandes coleções de textos.

Neste projeto, você deverá construir um pipeline completo de PLN, partindo de textos brutos até a geração de análises, modelos, visualizações e interpretações. O objetivo não é apenas aplicar técnicas isoladas, mas construir uma solução coerente, reprodutível e interpretável.

O projeto deve demonstrar que você consegue tomar decisões técnicas sobre o tratamento do texto, comparar representações, avaliar modelos, interpretar resultados e comunicar os achados de forma compreensível para uma audiência técnica e não técnica.

### Questão do projeto

**Como você pode construir um pipeline completo de Processamento de Linguagem Natural para transformar um corpus textual bruto em informações úteis, combinando pré-processamento, representação vetorial, modelagem, classificação, extração de entidades, grafo de conhecimento e visualização dos resultados?**

A partir dessa questão, escolha um corpus, defina o problema analítico, construa o pipeline e justify as decisões técnicas tomadas em cada etapa.

### Tarefa

Você deverá desenvolver um projeto completo de PLN em Python, utilizando um corpus textual público ou uma base própria anonimizada.

A solução deve partir do texto bruto, passar por etapas de pré-processamento, representação vetorial, análise semântica, classificação ou modelagem, extração de entidades e visualização dos resultados.

O projeto pode ser aplicado a diferentes contextos, como:

*   avaliações de produtos ou serviços;
    
*   reclamações de consumidores;
    
*   notícias ou artigos;
    
*   perguntas e respostas técnicas;
    
*   documentos jurídicos;
    
*   textos acadêmicos;
    
*   feedbacks de clientes, alunos ou usuários;
    
*   registros de atendimento;
    
*   textos corporativos ou administrativos;
    
*   corpus próprio anonimizado.
    

O corpus escolhido deve permitir avaliar as competências da disciplina. Por isso, ele precisa conter volume suficiente, textos com conteúdo semântico relevante, alguma forma de categoria ou rótulo e presença de entidades ou termos de domínio que possam ser explorados.

### Corpus de trabalho

O corpus é a base do projeto. A escolha deve ser feita com cuidado, pois ela define a qualidade das análises, dos modelos e das visualizações.

O corpus deve atender aos seguintes critérios mínimos:

*   conter pelo menos **1.000 documentos**;
    
*   ter textos com conteúdo suficiente para análise, preferencialmente com média próxima ou superior a **200 palavras por documento**;
    
*   possuir pelo menos **2 classes, categorias ou grupos comparáveis**, com quantidade suficiente de documentos em cada classe;
    
*   estar em português ou inglês;
    
*   conter entidades nomeadas ou termos de domínio, como pessoas, organizações, locais, datas, produtos, temas ou categorias;
    
*   ser público, reprodutível e citável, quando vier de fonte aberta;
    
*   ser compatível com execução em computador pessoal, sem exigência de GPU dedicada.
    

Você pode utilizar bases de fontes como Kaggle, Hugging Face, UCI ou repositórios públicos equivalentes. Também pode utilizar um corpus próprio, desde que ele possa ser disponibilizado em versão anonimizada, sintética ou substituta para avaliação pelo professor.

Exemplos de corpora possíveis incluem:

*   B2W Reviews 01;
    
*   Olist Brazilian E-Commerce Reviews;
    
*   Amazon Product Reviews;
    
*   Yelp Reviews;
    
*   BBC News Classification;
    
*   AG News;
    
*   News Category Dataset;
    
*   Consumer Complaints CFPB;
    
*   Stack Overflow Questions;
    
*   Brazilian Legal Decisions;
    
*   PubMed Abstracts.
    

Caso utilize dados próprios, descreva o contexto, a origem, as etapas de anonimização e as limitações de compartilhamento.

### Requisitos mínimos

O projeto deve atender a **5 requisitos mínimos**. Dentro de cada requisito, você poderá fazer escolhas técnicas compatíveis com o corpus e com o problema escolhido.

1. **Pré-processamento textual com NLTK e spaCy**

O projeto deve conter um pipeline de pré-processamento aplicado ao corpus real.

Esse pipeline deve incluir, no mínimo:

*   carregamento e inspeção inicial do corpus;
    
*   tokenização de palavras e sentenças;
    
*   normalização textual;
    
*   remoção de stopwords, incluindo stopwords customizadas quando fizer sentido;
    
*   comparação entre stemming e lemmatização;
    
*   aplicação de POS tagging com spaCy;
    
*   análise do impacto do pré-processamento no vocabulário.
    

Você deve justificar as escolhas realizadas. Por exemplo, explique se optou por stemming, lemmatização ou outra estratégia, e mostre como essa escolha afeta o vocabulário e as análises seguintes.

Como evidência, inclua visualizações como nuvem de palavras, histograma de comprimento dos documentos, distribuição de termos frequentes ou distribuição de POS tags.

2. **Representação vetorial e busca textual**

O projeto deve transformar os textos em representações numéricas e usar essas representações para análise.

Você deve aplicar pelo menos duas abordagens entre:

*   Bag-of-Words com CountVectorizer;
    
*   TF-IDF com TfidfVectorizer;
    
*   n-gramas, como bigramas ou trigramas;
    
*   Word2Vec com Gensim;
    
*   análise de similaridade por cosseno;
    
*   visualização de embeddings com t-SNE.
    

É obrigatório construir um mecanismo simples de busca textual por similaridade. A partir de uma consulta textual, o sistema deve retornar os documentos mais similares do corpus, com seus respectivos scores.

Inclua testes com pelo menos 3 consultas diferentes e comente se os resultados retornados fazem sentido para o domínio escolhido.

3. **Modelagem, classificação ou análise de sentimento**

O projeto deve extrair significado estruturado do corpus por meio de técnicas de modelagem, classificação ou análise de sentimento.

Você deve escolher e aplicar técnicas compatíveis com seu corpus, contemplando pelo menos duas das seguintes frentes:

*   modelagem de tópicos com LSA;
    
*   modelagem de tópicos com LDA;
    
*   modelagem de tópicos com NMF;
    
*   análise de sentimento lexical com VADER ou SentiWordNet;
    
*   classificação supervisionada com Naive Bayes;
    
*   classificação supervisionada com SVM;
    
*   classificação supervisionada com Regressão Logística;
    
*   tratamento de desbalanceamento com class\_weight, oversampling ou undersampling;
    
*   comparação de modelos com precision, recall, F1 e matriz de confusão.
    

Se o corpus tiver rótulos, treine e compare classificadores. Se o corpus não tiver rótulos adequados, dê mais ênfase à modelagem de tópicos e justifique essa escolha.

O projeto deve incluir métricas, visualizações e uma análise escrita sobre quais técnicas funcionaram melhor para o problema escolhido.

4. **NER, extração de informação e grafo de conhecimento**

O projeto deve aplicar técnicas de extração de informação para transformar partes do texto em estruturas analisáveis.

Você deve incluir pelo menos três das seguintes opções:

*   extração de entidades nomeadas com spaCy;
    
*   extração de padrões com regex, como datas, emails, URLs, valores, códigos ou termos específicos;
    
*   uso de distância de Levenshtein para aproximar variações ortográficas de entidades;
    
*   anotação de exemplos e criação de um NER customizado simples com spaCy;
    
*   extração de relações ou triplas sujeito-verbo-objeto;
    
*   construção de um grafo de conhecimento com NetworkX;
    
*   cálculo de centralidade no grafo;
    
*   visualização do grafo com matplotlib, PyVis ou ferramenta equivalente;
    
*   resposta a uma pergunta analítica ou de negócio usando o grafo.
    

O grafo deve ter pelo menos **20 nós** e deve ser construído a partir de entidades, termos ou relações extraídas do corpus. Ele deve ser acompanhado de interpretação: explique o que os nós mais centrais ou as relações mais importantes revelam sobre o domínio analisado.

5. **Visualização, comunicação e reprodutibilidade**

O projeto deve ser apresentado como um pipeline reprodutível e interpretável.

Você deve entregar um notebook principal que execute o processo de ponta a ponta, do carregamento do corpus às visualizações finais. O código deve estar organizado, comentado quando necessário e sem dependências ocultas.

O projeto deve incluir visualizações que ajudem a comunicar os resultados, como:

*   nuvens de palavras;
    
*   gráficos de barras de termos ou entidades;
    
*   histogramas de comprimento de documentos;
    
*   heatmaps de TF-IDF, tópicos ou matriz de confusão;
    
*   visualização t-SNE;
    
*   visualizações de NER ou dependências com displaCy;
    
*   grafo de conhecimento;
    
*   curvas de precision-recall;
    
*   tabelas comparativas de métricas.
    

Ao final, inclua uma síntese interpretativa em linguagem acessível, dirigida a um stakeholder não técnico. Essa síntese deve explicar os principais achados do pipeline, e não apenas descrever as técnicas utilizadas.

### Desenvolvimento da solução

O projeto deve ser desenvolvido em Python, preferencialmente em notebook, utilizando bibliotecas como NLTK, spaCy, scikit-learn, Gensim, pandas, matplotlib, NetworkX, WordCloud, PyVis ou equivalentes.

O pipeline deve ser construído de forma progressiva. Primeiro, você caracteriza e prepara o corpus. Depois, transforma os textos em representações numéricas. Em seguida, aplica modelos de análise, classificação ou tópicos. Por fim, extrai entidades, constrói estruturas relacionais e comunica os resultados.

O professor deve conseguir reproduzir o projeto a partir dos arquivos entregues. Caso o corpus não possa ser incluído diretamente, forneça instruções claras de download, preparação e execução. Caso utilize dados próprios ou sensíveis, forneça uma versão pública, anonimizada, sintética ou substituta que permita avaliar o pipeline.

Código funcional, isoladamente, não é suficiente. A avaliação também considera a qualidade da análise, a coerência das decisões técnicas, a interpretação dos resultados e a capacidade de comunicar os achados.

### Entrega

Você deverá entregar três artefatos:

1.  **Notebook principal end-to-end**, contendo o pipeline completo, do carregamento do corpus às visualizações finais. (Entregar arquivo .ipynb ou link compartilhado no Google Colab)
    
2.  **Arquivos auxiliares necessários**, como dados, amostras, imagens exportadas, grafo exportado, modelos salvos ou instruções de download do corpus.
    
3.  **Relatório técnico em PDF**, explicando o problema, o corpus, as decisões técnicas, os resultados e as limitações.
    

O relatório deve conter:

*   nome do aluno;
    
*   nome da disciplina;
    
*   título do projeto;
    
*   descrição do corpus escolhido;
    
*   link ou instruções de obtenção do corpus;
    
*   justificativa para escolha do corpus;
    
*   caracterização inicial dos dados;
    
*   descrição do pipeline de pré-processamento;
    
*   comparação entre stemming e lemmatização;
    
*   descrição das representações vetoriais utilizadas;
    
*   evidências do motor de busca textual;
    
*   descrição das técnicas de tópicos, classificação ou sentimento aplicadas;
    
*   métricas e visualizações dos modelos;
    
*   descrição da extração de entidades e informações;
    
*   explicação do grafo de conhecimento;
    
*   interpretação dos resultados;
    
*   síntese em linguagem não técnica;
    
*   limitações do pipeline;
    
*   possíveis melhorias;
    
*   instruções para reprodução do projeto.
    

O arquivo PDF deve seguir o padrão:

**nome\_sobrenome\_sistemas-cognitivos-linguagem-natural\_pln.pdf**

Exemplo:

**ana\_silva\_sistemas-cognitivos-linguagem-natural\_pln.pdf**