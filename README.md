Fotos vídeo do projeto
https://www.instagram.com/p/B849sLOpGmW/?utm_source=ig_web_copy_link

Realizar a instalação da biblioteca com as configurações de resolução já definidas pelos comando abaixo, estou usando um display 5"
sudo rm -rf LCD-show
git clone https://github.com/goodtft/LCD-show.git
chmod -R 755 LCD-show
cd LCD-show/
sudo ./LCD5-show

Caso a tela não fique na posição esperada é possível rotacionar com esses comandos abaixo

no lugar do 90 pode ser colocado os seguintes valores 0, 90, 180, 270
cd LCD-show/
sudo ./rotate.sh 90
Para a calibração do touch screen é necessário os comandos abaixo

sudo rm -rf LCD-show
git clone https://github.com/goodtft/LCD-show.git
chmod -R 755 LCD-show
cd LCD-show/
sudo dpkg -i -B xinput-calibrator_0.7.5-1_armhf.deb

Em seguida o comando
DISPLAY=:0.0 xinput_calibrator

Irá aparecer os pontos de toque na tela para calibração

Depois executar o comando
sudo nano /etc/X11/xorg.conf.d/99-calibration.conf

CTRL + X para sair
Y para salvar
e Enter para confirmar

reiniciar com
sudo reboot

Saída de áudio
Depois como a tela não tem saída de som é necessário habilitar a saída P2 do Raspberry

1 = analógico, 2 = HDMI, 0 = auto detectável
amixer cset numid=3 X X= 0, 1, 2
Configurar do desligamento de tela

Para desativar o apagamento da tela, abra o arquivo lightdm.conf .
sudo nano /etc/lightdm/lightdm.conf

Agora, aqui, adicione a seguinte linha em qualquer lugar abaixo da linha [SeatsDefaults] .
xserver-command=X -s 0 –dpms

Salve e saia pressionando ctrl + x e depois y .

Agora, reinicie o Pi e a tela não deverá mais se desligar após 10 minutos de inatividade. Para reiniciar, execute o seguinte:
sudo reboot

Configurar para auto execução para mostrar as fotos

Para instalar o pacote, use a seguinte linha:
sudo apt-get install feh

Agora, para testar se ele funciona, digite a seguinte linha. Substitua /home/pi/desktop/Fotos pelo diretório que contém toda a sua imagem.

DISPLAY=:0.0 XAUTHORITY=/home/pi/.Xauthority /usr/bin/feh --quiet --preload --randomize --full-screen --reload 60 -Y --slideshow-delay 15.0 /home/pi/desktop/Fotos &

Extra 3. Agora se quiser podemos usar tags curtas para tornar esse comando muito mais curto. Você pode ler mais sobre todas as bandeiras que você pode usar na página de manual feh, ou usar normalmente a de cima
DISPLAY=:0.0 XAUTHORITY=/home/pi/.Xauthority /usr/bin/feh -q -p -Z -F -R 60 -Y -D 15.0 /home/pi/desktop/Fotos &

Agora, como você notará, isso trava a barra da linha de comando. Para corrigir isso, adicione o & após o comando e o script / processo será iniciado em segundo plano.

Então agora vamos armazenar isso em um arquivo de script simples. Dessa forma, você pode adicionar ou alterar posteriormente. Para fazer o arquivo, digite o seguinte comando:
sudo nano /home/pi/start-picture-frame.sh

Aqui, digite as seguintes linhas.
#!/bin/bash
DISPLAY=:0.0 XAUTHORITY=/home/pi/.Xauthority /usr/bin/feh --quiet --preload --randomize --full-screen --reload 60 -Y --slideshow-delay 15.0 /home/pi/desktop/Fotos
Prefiro sem o --preload

Agora pronto, você pode testá-lo executando o seguinte comando.
bash /home/pi/start-picture-frame.sh

Configurando para iniciar junto com o Raspbian
Abra o arquivo /etc/profile, com o comando sudo nano /etc/profile.

No final do arquivo insira o comando que irá executar, por exemplo:
bash /home/pi/start-picture-frame.sh & & é necessário

Salve e saia
Reiniciando, deverá executar automaticamente
Demora um puco até carregar as fotos e iniciar os slides de fotos, pode observar o cache do processador carregando

Fontes
http://www.lcdwiki.com/5inch_HDMI_Display
http://www.lcdwiki.com/res/Show_Direction_and_Touch/How_to_calibrate_the_resistance_touch_screen-V1.2.pdf
https://devblog.drall.com.br/raspberry-pi-raspbian

https://linux.die.net/man/1/feh

https://pimylifeup.com/raspberry-pi-photo-frame/

https://cadernodelaboratorio.com.br/2015/06/10/inicializando-um-programa-automaticamente-no-raspberrypi/
