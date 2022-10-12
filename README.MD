# Self-driving Vessels: YOLOv5 Approach for Water Surface Object Detection in Amazon Rivers

The use of Computer Vision techniques for Water Surface Object Detection tasks has been rising as a strong trend in autonomous vessels context. In this work, we propose um modelo inédito, baseado no algoritmo YOLOv5, para detecção de objetos localizados, especificamente, em superfícies de rios da Amazônia. Para esse modelo, são utilizadas técnicas de Transfer Learning a fim de aproveitar a base de imagens WSODD.  Além disso, o deploy do modelo é realizado em uma plataforma embarcada (plataforma Jetson Nano) a fim de simular um cenário real de utilização. A seguir, são apresentados os detalhes deste repositório

## 1. Dataset WSOD-ARD (Water Surface Object Detection - Amazon Rivers Dataset)

O dataset está disponível publicamente por meio deste link: [WSOD-ARD Download](https://ueaedubr-my.sharepoint.com/:u:/g/personal/trs_eng17_uea_edu_br/EZymPOCqBTRIvAG5Hfl2MW4BejByGrN72PmM1VPhJHPT9A?e=IC1Wd0)

> *password*: **wsodard2022**

![alt text](https://github.com/rTiagoS/water-surface-object-detection/blob/main/images/mosaic_img.png)


### 1.1. Método para coleta das imagens

As imagens foram coletadas a partir do cruzamento de combinações de strings em repositórios online de imagens e vídeos, conforme ilustra a figura abaixo.

![alt text](https://github.com/rTiagoS/water-surface-object-detection/blob/main/images/coleta-de-imagens.png)

As imagens foram todas anotadas por meio da ferramenta roboflow.

![alt text](https://github.com/rTiagoS/water-surface-object-detection/blob/main/images/roboflow_annotations.png)

## 2. Transfer Learning com YOLOv5

### 2.1. Configuração do ambiente

O treinamento foi realizado dentro de um docker container. Para provisioná-lo, utilize o seguinte comando como referência:

> docker run --ipc=host -p 8888:8888 --gpus all -it -v "/path/to/dataset":"/usr/src"  ultralytics/yolov5:latest

Para mais detalhes sobre pré-requisitos e configurações adicionais, consultar: 

[YOlov5 Docker](https://docs.ultralytics.com/environments/Docker-Quickstart/)

### 2.2. Treinamento

Para o treinamento do modelo com transfer learning, foram utilizados os pesos obtidos a partir do treinamento com a base de imagens WSODD. Esses pesos estão salvos em `/weights/wsodd.pt`.

Para realização da etapa de treinamento, utilize o seguinte comando como referência:

> !python train.py --img 416 --batch 16 --epochs 300 --data /path/to/dataset/data.yaml --weights /path/to/weights/wsodd.pt --cache

Para mais detalhes sobre o treinamento dentro de uma instância do jupyter-notebook, consultar o notebook em `/notebooks/wsodd-ard-training.ipynb`