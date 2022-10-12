# Self-driving Vessels: YOLOv5 Approach for Water Surface Object Detection in Amazon Rivers

The use of Computer Vision techniques for Water Surface Object Detection tasks has been rising as a strong trend in autonomous vessels context. In this work, we propose um modelo inédito, baseado no algoritmo YOLOv5, para detecção de objetos localizados, especificamente, em superfícies de rios da Amazônia. Para esse modelo, são utilizadas técnicas de Transfer Learning a fim de aproveitar a base de imagens [WSOD](https://www.frontiersin.org/articles/10.3389/fnbot.2021.723336/full).  Além disso, o deploy do modelo é realizado em uma plataforma embarcada (plataforma Jetson Nano) a fim de simular um cenário real de utilização. A seguir, são apresentados os detalhes deste repositório

## Publicações

Caso nosso dataset seja utilizado, por favor, nos cite:

```
@inproceedings{sbcup,
 author = {T. Sá and C. Figueiredo},
 title = {Self-driving Vessels: YOLOv5 Approach for Water Surface Object Detection},
 booktitle = {Anais do XIV Simpósio Brasileiro de Computação Ubíqua e Pervasiva},
 location = {Niterói},
 year = {2022},
 keywords = {},
 issn = {2595-6183},
 pages = {31--40},
 publisher = {SBC},
 address = {Porto Alegre, RS, Brasil},
 doi = {10.5753/sbcup.2022.222855},
 url = {https://sol.sbc.org.br/index.php/sbcup/article/view/20608}
}
```

Este trabalho também foi escrito e defendido como trabalho de conclusão de curso (TCC) para obtenção do título de Engenheiro Eletricista.




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

Para mais detalhes sobre pré-requisitos e configurações adicionais, consultar: [YOlov5 Docker](https://docs.ultralytics.com/environments/Docker-Quickstart/)

### 2.2. Treinamento

Para o treinamento do modelo com transfer learning, foram utilizados os pesos obtidos a partir do treinamento com a base de imagens WSODD. Esses pesos estão salvos em: `/weights/wsodd.pt`.

Para realização da etapa de treinamento, utilize o seguinte comando como referência:

> !python train.py --img 416 --batch 16 --epochs 300 --data /path/to/dataset/data.yaml --weights /path/to/weights/wsodd.pt --cache

Para mais detalhes sobre o treinamento dentro de uma instância do jupyter-notebook, consultar o notebook em: `/notebooks/wsodd-ard-training.ipynb`

## 3. Deploy do modelo em um Nvidia Jetson Nano

Após a etapa de treinamento e validação, o modelo é embarcado em um edge device da Nvidia (Jetson Nano). 

### 3.1. Configuração da Plataforma Jetson Nano Nvidia

Para mais detalhes sobre como realizar a primeira configuração desse dispostivo embarcado, consultar: [Jetson Nano Developer Kit](https://developer.nvidia.com/embedded/jetson-nano-developer-kit)

Atualmente, infelizmente a versão mais recente do sistema operacional do Jetson Nano não é suportado pela última versão do YOLOv5. Dessa forma, para rodar um modelo treinado a partir do YOLOv5 em um Jetson Nano, é necessário utilizar uma versão mais antiga. 

Para essa etapa, recomendo fortemente seguir a documentação descrita em: [Docker image for object detection using YOlov5 on Nvidia Jetson Plataform](https://github.com/otamajakusi/dockerfile-yolov5-jetson)

### 3.2. Detecção de objetos usando YOLOv5 na Plataforma Jetson Nano Nvidia

Após essa a etapa de configuração acima, com uma câmera USB plugada no Jetson Nano, executar o seguinte comando de referência em um terminal do Jetson Nano:

```
docker run -it --rm \
           --runtime nvidia \
           --network host \
           --device /dev/video0:/dev/video0:mrw \
           -e DISPLAY=$DISPLAY \
           -e LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libgomp.so.1 \
           -v /tmp/.X11-unix/:/tmp/.X11-unix \
           -v /path/to/weights:/weights \
           yolov5 python3.8 detect.py --source 0 --weights /weights/wsodd-arv.pt.pt
```
Observe que é utilizado o peso wsodd-arv.pt localizado em `/weights/wsodd-arv.pt`. 


Ilustração da câmera USB plugada no dispositivo Jetson:

![alt text](https://github.com/rTiagoS/water-surface-object-detection/blob/main/images/roboflow_annotations.png)


### 3.3. Simulação e prototipagem do YOLOv5 com Jetson Nano

Por último, a partir do momento em que o modelo integrado na plataforma embarcada Jetson Nano iniciou o processo de detecção de objetos a partir da captura de imagens com a câmera USB, vídeos de objetos localizados em superfícies de rios da Amazônia foram transmitidos em um monitor de modo que pudessem simular, em tempo real de execução, a captura de uma sequência imagens em um cenário real, conforme ilustra a figura a seguir.

![alt text](https://github.com/rTiagoS/water-surface-object-detection/blob/main/images/roboflow_annotations.png)
