# Título do Projeto: Processamento de imagens com HOG e KNN

Link do respositório: https://github.com/ItaloF532/processamento_de_imagens

## Equipe

RA: 2538911 - Italo Francisco Magdalena

## Resumo

O projeto consiste em um script Python (`image_classifier.py`) que treina um modelo de Machine Learning para classificar imagens em 10 categorias e, em seguida, utiliza o modelo treinado para classificar uma imagem a partir de uma URL fornecida.

## Funcionamento Técnico

O script implementa um pipeline completo de classificação de imagens, desde o download de dados e treinamento até a inferência em uma nova imagem. Abaixo estão os detalhes técnicos de cada etapa.

### 1. Dataset - CIFAR-10

Foi utilizado o dataset [CIFAR-10 do OpenMl](https://www.openml.org/search?type=data&status=active&id=40927), para isso utilize uma lib para efetuar o download do modelo `sklearn.datasets.fetch_openml.`

O CIFAR_10 tem 60 mil imagens coloridas 32x32 divididas em 10 classes: avião, automóvel, pássaro, gato, veado, cachorro, sapo, cavalo, navio, caminhão.

Para mais detalhes pode consultar o link fornecido acima ou [aqui](https://www.openml.org/search?type=data&status=active&id=40927).

### 2. Extração de Características com HOG (Histogram of Oriented Gradients)

HOG ou Histogram of Oriented Gradients (em português Histogram of Oriented Gradients) é um descritor de recursos para detectar objetos ou reconhecer padrões. Ele funciona capturando a estrutura e a textura de uma imagem, focando especificamente nos gradientes, que representam mudanças na intensidade.
No script desse projeto utilizamos o HOG para fazer justmante o descrito acima, ou seja, extraímos as caracteristicas da imagem gerando um vetor de características que descreve a estrutura de formas e contornos do objeto na imagem. Os parâmetros `pixels_per_cell=(8, 8)` e `cells_per_block=(2, 2)` definem a granularidade da extração.:

### 3. Modelo de Classificação - KNN (K-Nearest Neighbors)

KNN é um classificador de aprendizado supervisionado, que usa a proximidade para fazer classificações ou previsões sobre o agrupamento de um determinado ponto de dados. No script utilizamos o CIFAR_10 como conjunto de dados para treino (80%) e teste (20%). ELe basicamente armazena em memória todos os vetores de características do conjunto de treino.

### 4. Persistência do Modelo

Para evitar a necessidade de retreinar o modelo a cada execução, o objeto do classificador treinado é serializado e salvo em disco no arquivo `image_classifier_model.pkl` utilizando `joblib.dump`.
Ao iniciar, o script verifica a existência desse arquivo. Se presente, o modelo é carregado diretamente na memória com `joblib.load`, otimizando o tempo de execução. Caso contrário, o pipeline de treinamento é acionado.

### 5. Classificação (Inferência)

Dado uma imagem em `image_url` a imagem é baixada, seus bytes são armazenados em memória e passa pelo mesmo pipeline de pré-processamento e extração de características HOG aplicado aos dados de treino.
Após isso com o KNN treinado, utilizamos o método `predict()` identificar os 5 vizinhos mais próximos e retorna a classe majoritária entre eles.

## Como Usar

Alterar a variável `image_url` e executar o notebook.
