# INSTALAÇÃO DO S.O
- Primeiramente baixe:
  - [Etcher](https://www.balena.io/etcher/)
  - [Jetson Nano Developer Kit SD Card Image| V4.6](https://developer.nvidia.com/embedded/l4t/r32_release_v6.1/jeston_nano/jetson-nano-jp46-sd-card-image.zip)
    - Caso o link não funcione, baixe direto do Jetson download Center : https://developer.nvidia.com/embedded/downloads#?search=nano

- Use o etcher para instalar a imagem Jetson Nano Developer Kit SD Card Image no cartão SD:
![Example 0](https://github.com/hewertonfl/Tutorial_jetson_nano/blob/d910cdba80ab1e9c77cedecdfaee7b37392ec441/img_tut/Captura%20de%20tela%202022-07-13%20130141.jpg)
  - Selecione flash from file e escolha o arquivo Jetson Nano Developer Kit SD Card Image.
  - Selecione o cartão SD.
  - Selecione Flash e aguarde a instalação do sistema.

# Instalação das dependências
- Python 3.9
- Abra o terminal do ubuntu e insira os comandos abaixo
```Shell
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.9
```
- Troque o python padrão para 3.9
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
- Instalação
 ```Shell
git clone https://github.com/ultralytics/yolov5
cd yolov5
pip install -r requirements.txt
 ```
