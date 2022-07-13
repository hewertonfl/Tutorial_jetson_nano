# INSTALAÇÃO DO S.O
- Primeiramente baixe:
  - [Etcher](https://www.balena.io/etcher/)
  - [Jetson Nano Developer Kit SD Card Image| V4.6](https://developer.nvidia.com/embedded/l4t/r32_release_v6.1/jeston_nano/jetson-nano-jp46-sd-card-image.zip)
    - Caso o link não funcione, baixe direto do [Jetson Download Center](https://developer.nvidia.com/embedded/downloads#?search=nano)

- Use o etcher para instalar a imagem Jetson Nano Developer Kit SD Card Image no cartão SD:
![Example 0](https://github.com/hewertonfl/Tutorial_jetson_nano/blob/d910cdba80ab1e9c77cedecdfaee7b37392ec441/img_tut/Captura%20de%20tela%202022-07-13%20130141.jpg)
  - Selecione flash from file e escolha o arquivo Jetson Nano Developer Kit SD Card Image.
  - Selecione o cartão SD.
  - Selecione Flash e aguarde a instalação do sistema.

# Instalação das dependências
- Python 3.9
- Abra o terminal do ubuntu (CTRL+ALT+T) e insira os comandos abaixo:
```Shell
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.9
```
- Troque o python padrão para 3.9:
 ```Shell
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 1
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2
```
- Pytorch 1.9
```Shell
wget https://nvidia.box.com/shared/static/p57jwntv436lfrd78inwl7iml6p13fzh.whl -O torch-1.8.0-cp36-cp36m-linux_aarch64.whl -O torch-1.9.0-cp36-cp36m-linux_aarch64.whl
sudo apt-get install python3-pip libopenblas-base libopenmpi-dev 
pip3 install Cython
pip3 install numpy torch-1.9.0-cp36-cp36m-linux_aarch64.whl
```
- Torchvision
```Shell
https://drive.google.com/uc?id=1tU6YlPjrP605j4z8PMnqwCSoP6sSC91Z
pip3 install torchvision-0.10.0a0+300a8a4-cp36-cp36m-linux_aarch64.whl
```
# Yolov5
- Clone e instale a depedências.
 ```Shell
git clone https://github.com/ultralytics/yolov5
cd yolov5
pip install -r requirements.txt
 ```
# Custom datasets
 - Separe o dataset em pastas de treinamento de validação.
    - Dentro de cada pasta, crie uma para imagens e anotações
    - Divida o dataset numa proporção de 80:20 ou 75:20
 
 # Yolact
 - Clone o repositorio.
 ```Shell
git clone https://github.com/dbolya/yolact.git
cd yolact
```
- Instale outros pacotes:
 ```Shell
# Cython precisa ser instalados antes de pycocotools
pip install cython
pip install opencv-python pillow pycocotools matplotlib
 ```
## Custom Datasets
Você pode criar seu próprio dataset seguindos esses passos:
 - Primeiramente é necessário criar um arquivo JSON com as anotações nas imagens.
  - Para isso utilize o [labelme](https://github.com/wkentaro/labelme)
  - Para usar no Windows é necessario instalar o [Anaconda](https://www.anaconda.com).
  ```Shell
conda create --name=labelme python=3
conda activate labelme
pip install labelme
pip install -U labelme2coco
```
  - Para executar, rode o comando:
 ```Shell 
 labelme
  ```
  - Para anotar uma imagem abra uma imagem ou um diretório de imagens e clique em `Create Polygons`, segmente o objeto de sua preferência.
  - Escolha um nome para a classe e um id para ela.
![Example 1](https://github.com/hewertonfl/Tutorial_jetson_nano/blob/3046e5bc0c84ed53eaa847984f49fcc82d535eaf/img_tut/imagem_2022-07-13_155634959.png)
  - Feito o dataset, é hora de converte-lo para o formato coco.
  - Use o ccomando:
 ```Shell
 labelme2coco caminho/para/labelme/diretorio
  ```
 - Crie uma definição do dataset abaixo de  `dataset_base` em `data/config.py` (Veja os comentários em `dataset_base` para uma explicação de cada campo):
```Python
meu_custom_dataset = dataset_base.copy({
    'name': 'Meu Dataset',

    'train_images': 'caminho_para_imagens_de_treinamento',
    'train_info':   'caminho_para_anotações_de_treinamento',

    'valid_images': 'caminho_para_imagens_de_validação',
    'valid_info':   'caminho_para_anotações_de_validação',

    'has_gt': True,
    'class_names': ('minha_classe_id_1', 'minha_classe_id_2', 'minha_classe_id_3', ...)
})
```
 - NOTAS:
   - Class IDs no arquivo de anotação precisa começar em 1 e aumentar sequencialmente na ordem de `class_names`. Se esse não for o caso, veja o campo `label_map` em `dataset_base`.
   - Se você não quiser dividir uma parte do dataset para validação, use os mesmos caminhos das imagens e anotações de treinamento.
   - Finalmente, em `yolact_base_config` no mesmo arquivo, mude`'dataset'` to `'meu_custom_dataset'` ou qualquer que seja o nome que foi dado anteriormente.
- Treinamento
```Shell 
python train.py --config=tronco_config 
 ```
- Avaliação
  - Os pesos ficam na pasta de weights, para testa-los, apenas precisamos passar o caminho no comando.
  - Se for rodar no ubuntu use `python3` em vez de `python`.
```Shell
python eval.py --trained_model=weights/nome_do_peso.pth --score_threshold=0.7 --top_k=7 --image=caminho_da_imagem_de_entrada.JPEG:caminho_da_imagem_de_saida.png
 ```
