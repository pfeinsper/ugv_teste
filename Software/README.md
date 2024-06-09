# README - Software do UGV para Monitoramento de Talhões em Fruticultura e Silvicultura

## Visão Geral

Este projeto visa desenvolver um Veículo Terrestre Não Tripulado (UGV) para monitoramento de talhões em fruticultura e silvicultura, inspirado no projeto Open Source Rover da NASA. A seguir, detalhamos a configuração do software utilizado no UGV e os resultados obtidos com o sistema.

## Preparação do Ambiente de Desenvolvimento

### Instalação do Sistema Operacional

1. **Baixar a Imagem do Ubuntu Server**:
   - Acesse o [site oficial do Ubuntu Server](https://ubuntu.com/download/server) e baixe a imagem mais recente.
   - Utilize um software como o Raspberry Pi Imager para gravar a imagem no cartão SD.

2. **Configuração Inicial**:
   - Insira o cartão SD na Raspberry Pi e ligue o dispositivo.
   - Configure a rede Wi-Fi e outras preferências básicas durante a inicialização.
   - Atualize o sistema operacional:
     ```sh
     sudo apt update
     sudo apt upgrade
     ```

### Instalação do ROS Humble

1. **Adicionar Chave do Repositório ROS**:
   ```sh
   sudo apt install software-properties-common
   sudo add-apt-repository universe
   sudo apt update
   sudo apt install curl
   curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
   ```

2. **Adicionar Repositório ROS**:
   ```sh
   sudo sh -c 'echo "deb http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'
   sudo apt update
   sudo apt install ros-humble-desktop
   ```

3. **Configurar Ambiente ROS**:
   Adicione as seguintes linhas ao arquivo `~/.bashrc`:
   ```sh
   source /opt/ros/humble/setup.bash
   ```

### Configuração da Raspberry Pi como Servidor e Comunicação SSH

1. **Modo Servidor**:
   - A configuração do servidor permite que a Raspberry Pi seja acessada remotamente.
   - Conecte-se ao servidor utilizando SSH:
     ```sh
     ssh ubuntu@10.42.0.1
     ```

2. **Instalação do Framework ROS**:
   - Com o ROS instalado, crie um workspace e compile os pacotes necessários:
     ```sh
     mkdir -p ~/ros2_ws/src
     cd ~/ros2_ws
     colcon build
     ```

### Aquisição de Imagens

1. **Instalação de Bibliotecas Necessárias**:
   ```sh
   sudo apt install ros-humble-cv-bridge
   sudo apt install ros-humble-image-transport
   ```

2. **Configuração de Câmeras**:
   - Conecte as câmeras Intel RealSense D435i e Logitech C920 HD PRO na Raspberry Pi.
   - Utilize os seguintes comandos para verificar e ajustar as configurações:
     ```sh
     rs-enumerate-devices
     v4l2-ctl --list-devices
     ```

3. **Publicação e Visualização de Imagens**:
   - Execute os nós ROS para capturar e publicar imagens:
     ```sh
     ros2 run realsense2_camera realsense2_camera_node
     ros2 run image_transport republish raw in:=/camera/color/image_raw out:=/image_raw
     ```

## Resultados

### Visualização e Monitoramento

1. **Interface Gráfica (Rviz2)**:
   - Utilizada para visualização dos tópicos ROS, incluindo as imagens capturadas pelas câmeras.
   - Permite monitorar a posição e o status dos sensores em tempo real.

2. **Aquisição de Imagens**:
   - As câmeras Intel RealSense D435i capturam imagens com profundidade, permitindo a criação de mapas 3D do ambiente.
   - As imagens são publicadas em tópicos ROS e podem ser visualizadas remotamente.

### Desempenho e Processamento

1. **Processamento em Tempo Real**:
   - A configuração em modo servidor da Raspberry Pi, aliada ao ROS, permite o processamento eficiente das imagens capturadas.
   - O sistema foi capaz de lidar com o processamento de múltiplos streams de imagem simultaneamente sem atraso significativo.

2. **Estabilidade e Confiabilidade**:
   - O sistema demonstrou estabilidade durante os testes, com a comunicação SSH permitindo controle remoto seguro e eficaz.
   - A integração com o ROS possibilitou a fácil adição e modificação de nós conforme necessário, garantindo flexibilidade para futuras expansões.

## Contribuidores

- Breno Alencar Araújo
- Bruno Morales Balkins
- Fernando Bichuette Assumpção
- Giulia Carolina Martins de Sampaio

Orientador: Prof. Dr. Vinícius Licks
Mentor: Thiago Teixeira Santos


