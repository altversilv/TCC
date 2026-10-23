Nome do projeto
Aluno: Altamiro Verissimo da Silveira Junior
Orientadora: Manoela Kohler.
Co-orientador(/a/es/as): Nome Sobrenome. 


Trabalho apresentado ao curso VC MASTER como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

Link para o código. 

Link para a monografia. 

Trabalhos relacionados: 

Nome do Trabalho 1.
Nome do Trabalho 2.


Resumo
Este Trabalho considera a análise de modelos multimodais para classificação de imagens e sinais de áudio. Para isto, foram utilizados dois modelos de deep-learning, para posterior fusão de dados e fine-tunning. Foi utilizado um dataset com sinais de áudio e imagem  e analisado o desempenho de cada modelo individualmente e da combinação dos dois modelos para os respectivos sinais. Ao final, foi gerado o relatório das métricas para inferência da assertividade de cada modelo e para combinação dos dois e apresentado as conclusões finais. 
Abstract 
This work deals multimodal models analyze to image and audio signals classification. It was used two deep-learning models and after applied data fusion and fine-tunning. It was considered a dataset with image and  audio signals and evaluate the performance of each model and the both models. After that, it was generated a report including main performance parameters for each model and the both models and presented the main conclusions. 


1. Introdução
Neste estudo, foi utilizado o Marine animals multimodal dataset do huggingface                    (‘//huggingface.co/datasets/Hariprasath5128/marine-animals-multimodal-dataset’) contendo 24911 amostras, 32 espécies (classes), 1357 arquivos de audio e 581 imagens. São as seguintes classes: 
classes: ['Atlantic_Spotted_Dolphin' 'Bearded_Seal' 'Beluga,_White_Whale'
 'Bottlenose_Dolphin' 'Bowhead_Whale' 'Clymene_Dolphin' 'Common_Dolphin'
 'False_Killer_Whale' 'Fin,_Finback_Whale' 'Frasers_Dolphin'
 'Grampus,_Rissos_Dolphin' 'Harp_Seal' 'Humpback_Whale' 'Killer_Whale'
 'Leopard_Seal' 'Long-Finned_Pilot_Whale' 'Melon_Headed_Whale'
 'Minke_Whale' 'Narwhal' 'Northern_Right_Whale'
 'Pantropical_Spotted_Dolphin' 'Ross_Seal' 'Rough-Toothed_Dolphin'
 'Short-Finned_Pacific_Pilot_Whale' 'Southern_Right_Whale' 'Sperm_Whale'
 'Spinner_Dolphin' 'Striped_Dolphin' 'Walrus' 'Weddell_Seal'
 'White-beaked_Dolphin' 'White-sided_Dolphin']

A maioria das imagens estão no formato RGB, havendo entretanto imagens no formato grayscale e RGBA, sendo todas convertidas para o formato RGB.  Os pixels tem resolução de 8 bits. As imagens são redimensionadas para o padrão de (224,224). Os arquivos de áudio contém diferentes duração e frequências de amostragem. Segue abaixo a relação das frequências de amostragem:
array([   600,    640,   1280,   5000,   5120,   8000,  10000,  10200,
        10240,  14900,  16000,  18000,  20000,  20480,  21900,  30000,
        40000,  40960,  41600,  43900,  44100,  55500,  60600,  80000,
        81600,  81920, 148100, 160000, 166600])

O dataset contém as seguintes variáveis:

species (string): Species name
label (int32): Numeric label (0–31)
audio (Audio): Audio recording of the species
image (Image): Species image
image_index (int32): Image number for this species
total_images (int32): Total images for this species
source (string): "LLM-Vision-Marine-Animals" or "iNaturalist"

Eliminando as repetições dos sinais de áudio e imagem o dataset fica reduzido a 551 amostras. Foi avaliado neste estudo também outro dataset, (‘https://huggingface.co/datasets/blanchon/      ADVANCE’), contendo arquivos de imagem e áudio. As características deste dataset são:

Total Number of Images: 5075
Bands: 3 (RGB)
Image Resolution: 10mm
Image size: 512x512
Land Cover Classes: 13
Classes: airport, beach, bridge, farmland, forest, grassland, harbour, lake, orchard, residential, sparse shrub land, sports land, train station
Source: Sentinel-2
Dataset features: 5,075 pairs of geotagged audio recordings and images, three spectral bands - RGB (512x512 px), 10-second audio recordings
Dataset format:, images are three-channel jpgs, audio files are in wav format



Foram realizadas várias análises exploratórias no dataset, não identificando nenhuma repetição de imagem e áudio. Os arquivos de áudio tem uma duração de 10 s e frequência de amostragem de 22050 Hz.
2. Modelagem
Foi utilizado neste trabalho como modelo para obtenção dos vetores representativos (embedding) dos sinais de áudio e imagem o backbone da rede neural convolucional resnet50 pré-treinada com pesos do imagenet, resultando na saída do modelo um vetor de dimensão 1x2048 para cada imagem do dataset. No caso dos sinais de áudio, estes foram convertidos em um espectrograma em decibel utilizando a função melspectrogram e depois normalizados e escalonados no intervalo de 0-255,redimensionados para 224x224 e replicados para conversão em uma imagem de 3 canais para processamento do modelo. Da mesma forma, os arquivos de imagem foram redimensionados para o formato RGB (224x224) para compatibilizar com o formato do modelo. Para classificação dos embeddings de imagem e áudio é utilizada uma rede neural feedforward com uma camada escondida de 2750 neurônios e função de ativação relu além de uma camada de dropout com rate de 0.42. Na saída foi usado uma função de ativação softmax. O modelo foi testado para os arquivos de imagem, de áudio e finalmente para a fusão das duas informações (áudio + imagem). Foi definido um conjunto de teste de 20% do dataset reduzido de 551 amostras resultando em 111 amostras e 440 amostras de treinamento.
3. Resultados
Foi calculado para cada classe do respectivo modelo os parâmetros precision, recall e f1-score, bem como a respectiva matriz de confusão para os modelos de áudio, imagem e fusão de dados. Para o modelo de imagem: 

   
                                    precision    recall  f1-score   support

        Atlantic_Spotted_Dolphin       0.88      0.88      0.88         8
                    Bearded_Seal       0.50      0.50      0.50         2
             Beluga,_White_Whale       1.00      0.33      0.50         3
              Bottlenose_Dolphin       0.50      1.00      0.67         3
                   Bowhead_Whale       0.25      0.50      0.33         2
                 Clymene_Dolphin       0.00      0.00      0.00         1
                  Common_Dolphin       1.00      0.76      0.87        17
              False_Killer_Whale       1.00      0.67      0.80         3
              Fin,_Finback_Whale       0.00      0.00      0.00         2
                 Frasers_Dolphin       0.00      0.00      0.00         2
         Grampus,_Rissos_Dolphin       0.67      0.50      0.57         4
                       Harp_Seal       1.00      1.00      1.00         1
                  Humpback_Whale       0.75      0.75      0.75         4
                    Killer_Whale       0.40      1.00      0.57         2
                    Leopard_Seal       1.00      0.33      0.50         3
         Long-Finned_Pilot_Whale       1.00      0.40      0.57         5
              Melon_Headed_Whale       0.00      0.00      0.00         0
                     Minke_Whale       0.43      0.60      0.50         5
                         Narwhal       0.50      0.67      0.57         3
            Northern_Right_Whale       0.14      1.00      0.25         1
     Pantropical_Spotted_Dolphin       0.50      1.00      0.67         1
                       Ross_Seal       0.00      0.00      0.00         0
           Rough-Toothed_Dolphin       0.00      0.00      0.00         1
Short-Finned_Pacific_Pilot_Whale       1.00      0.25      0.40         4
            Southern_Right_Whale       0.00      0.00      0.00         4
                     Sperm_Whale       1.00      0.67      0.80         3
                 Spinner_Dolphin       0.00      0.00      0.00         3
                 Striped_Dolphin       0.71      0.71      0.71         7
                          Walrus       1.00      1.00      1.00         5
            White-beaked_Dolphin       0.75      0.75      0.75         4
             White-sided_Dolphin       1.00      0.12      0.22         8

                        accuracy                           0.57       111
                       macro avg       0.55      0.50      0.46       111
                    weighted avg       0.73      0.57      0.59       111

 












Para o modelo de aúdio:

  
                                 

                                  precision    recall  f1-score   support

        Atlantic_Spotted_Dolphin       0.86      0.75      0.80         8
                    Bearded_Seal       0.00      0.00      0.00         2
             Beluga,_White_Whale       1.00      0.33      0.50         3
              Bottlenose_Dolphin       1.00      0.67      0.80         3
                   Bowhead_Whale       1.00      0.50      0.67         2
                 Clymene_Dolphin       1.00      1.00      1.00         1
                  Common_Dolphin       1.00      0.71      0.83        17
              False_Killer_Whale       0.75      1.00      0.86         3
              Fin,_Finback_Whale       1.00      1.00      1.00         2
                 Frasers_Dolphin       0.67      1.00      0.80         2
         Grampus,_Rissos_Dolphin       0.60      0.75      0.67         4
                       Harp_Seal       0.50      1.00      0.67         1
                  Humpback_Whale       1.00      0.75      0.86         4
                    Killer_Whale       0.33      1.00      0.50         2
                    Leopard_Seal       0.50      0.33      0.40         3
         Long-Finned_Pilot_Whale       0.67      0.40      0.50         5
              Melon_Headed_Whale       0.00      0.00      0.00         0
                     Minke_Whale       1.00      1.00      1.00         5
                         Narwhal       0.67      0.67      0.67         3
            Northern_Right_Whale       0.20      1.00      0.33         1
     Pantropical_Spotted_Dolphin       0.50      1.00      0.67         1
           Rough-Toothed_Dolphin       0.33      1.00      0.50         1
Short-Finned_Pacific_Pilot_Whale       1.00      0.25      0.40         4
            Southern_Right_Whale       1.00      0.50      0.67         4
                     Sperm_Whale       0.33      0.33      0.33         3
                 Spinner_Dolphin       0.50      1.00      0.67         3
                 Striped_Dolphin       0.57      0.57      0.57         7
                          Walrus       0.83      1.00      0.91         5
            White-beaked_Dolphin       0.67      1.00      0.80         4
             White-sided_Dolphin       0.83      0.62      0.71         8

                        accuracy                           0.69       111
                       macro avg       0.68      0.70      0.64       111
                    weighted avg       0.78      0.69      0.70       111


 




           
Para o caso multi-modal ( imagem+áudio)

 

                                  precision    recall  f1-score   support

        Atlantic_Spotted_Dolphin       1.00      0.88      0.93         8
                    Bearded_Seal       1.00      0.50      0.67         2
             Beluga,_White_Whale       1.00      0.67      0.80         3
              Bottlenose_Dolphin       0.75      1.00      0.86         3
                   Bowhead_Whale       0.00      0.00      0.00         2
                 Clymene_Dolphin       0.50      1.00      0.67         1
                  Common_Dolphin       1.00      0.71      0.83        17
              False_Killer_Whale       1.00      1.00      1.00         3
              Fin,_Finback_Whale       0.33      0.50      0.40         2
                 Frasers_Dolphin       1.00      1.00      1.00         2
         Grampus,_Rissos_Dolphin       1.00      1.00      1.00         4
                       Harp_Seal       0.50      1.00      0.67         1
                  Humpback_Whale       0.75      0.75      0.75         4
                    Killer_Whale       0.50      1.00      0.67         2
                    Leopard_Seal       0.67      0.67      0.67         3
         Long-Finned_Pilot_Whale       1.00      0.60      0.75         5
              Melon_Headed_Whale       0.00      0.00      0.00         0
                     Minke_Whale       1.00      1.00      1.00         5
                         Narwhal       0.67      0.67      0.67         3
            Northern_Right_Whale       0.50      1.00      0.67         1
     Pantropical_Spotted_Dolphin       0.50      1.00      0.67         1
           Rough-Toothed_Dolphin       1.00      1.00      1.00         1
Short-Finned_Pacific_Pilot_Whale       1.00      0.75      0.86         4
            Southern_Right_Whale       1.00      1.00      1.00         4
                     Sperm_Whale       0.50      0.33      0.40         3
                 Spinner_Dolphin       0.60      1.00      0.75         3
                 Striped_Dolphin       0.67      0.86      0.75         7
                          Walrus       1.00      1.00      1.00         5
            White-beaked_Dolphin       0.80      1.00      0.89         4
             White-sided_Dolphin       1.00      0.75      0.86         8

                        accuracy                           0.80       111
                       macro avg       0.74      0.79      0.74       111
                    weighted avg       0.86      0.80      0.81       111


 



               






4. Conclusões
Em função da redução do número de amostras com repetição de imagens do dataset  a acurácia do modelo de imagem foi afetada drasticamente. A assertividade do modelo de aúdio foi menos comprometida. Entretanto, efetuando a fusão dos dois modelos, abordagem multi-modal, o resultado apresentou uma melhoria significativa.



Matrícula: 241.101.124

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação Visão Computacional Master
