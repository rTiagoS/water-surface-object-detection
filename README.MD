# Self-driving Vessels: YOLOv5 Approach for Water Surface Object Detection in Amazon Rivers

The use of Computer Vision techniques for Water Surface Object Detection tasks has been rising as a strong trend in autonomous vessels context. In this work, we propose um modelo inédito, baseado no algoritmo YOLOv5, para detecção de objetos localizados, especificamente, em superfícies de rios da Amazônia. Para esse modelo, são utilizadas técnicas de Transfer Learning a fim de aproveitar a base de imagens WSODD.  Além disso, o deploy do modelo é realizado em uma plataforma embarcada (plataforma Jetson Nano) a fim de simular um cenário real de utilização. A seguir, são apresentados os detalhes deste repositório

## Dataset WSOD-ARD (Water Surface Object Detection - Amazon Rivers Dataset)

O dataset está disponível publicamente por meio deste link: [WSOD-ARD Download](https://ueaedubr-my.sharepoint.com/:u:/g/personal/trs_eng17_uea_edu_br/EZymPOCqBTRIvAG5Hfl2MW4BejByGrN72PmM1VPhJHPT9A?e=IC1Wd0)

> *password*: **wsodard2022**

![alt text](https://github.com/rTiagoS/water-surface-object-detection/blob/main/images/wsod-ard-example.jpeg)


### Método para coleta das imagens

As imagens foram coletadas a partir do cruzamento de combinações de strings em repositórios online de imagens e vídeos, conforme ilustra a figura abaixo.

![alt text](https://github.com/rTiagoS/water-surface-object-detection/blob/main/images/coleta-de-imagens.png)

As imagens foram todas anotadas por meio da ferramenta roboflow.

![alt text](https://github.com/rTiagoS/water-surface-object-detection/blob/main/images/roboflow_annotations.png)

## Transfer Learning com YOLOv5