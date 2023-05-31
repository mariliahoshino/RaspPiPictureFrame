Project photos and video
https://www.instagram.com/p/B849sLOpGmW/?utm_source=ig_web_copy_link

Install the library with the resolution settings already defined by the commands below, I'm using a 5" display

<code>sudo rm -rf LCD-show  </code> <br>
<code>git clone https://github.com/goodtft/LCD-show.git </code><br>
<code>chmod -R 755 LCD-show </code><br>
<code>cd LCD-show/ </code><br>
<code>sudo ./LCD5-show </code><br>

If the screen is not in the expected position, it is possible to rotate it with these commands below
in place of 90 you can put the following values 0, 90, 180, 270
<code> cd LCD-show/ </code><br>
<code> sudo ./rotate.sh 90 </code><br>

For the calibration of the touch screen, the commands below are required

<code>sudo rm -rf LCD-show </code><br>
<code>git clone https://github.com/goodtft/LCD-show.git </code><br>
<code>chmod -R 755 LCD-show </code><br>
<code>cd LCD-show/ </code><br>
<code>sudo dpkg -i -B xinput-calibrator_0.7.5-1_armhf.deb </code><br>

Then the command
<code> DISPLAY=:0.0 xinput_calibrator </code><br>
Touch points will appear on the screen for calibration

Then run the command
<code> sudo nano /etc/X11/xorg.conf.d/99-calibration.conf </code><br>

CTRL + X to exit
Y to save
and Enter to confirm

restart with
<code>sudo reboot </code><br>

audio output
Then, as the screen has no sound output, it is necessary to enable the Raspberry P2 output

1 = analog, 2 = HDMI, 0 = auto detectable
<code>amixer cset numid=3 X </code><br> X= 0, 1, 2 (in my situation I used "1" analog)

Configure no screen blanking
To disable screen blanking, open the lightdm.conf file.
<code>sudo nano /etc/lightdm/lightdm.conf</code><br>

Now here add the following line anywhere below the <b>[SeatsDefaults]</b> line.
<code>xserver-command=X -s 0 –dpms</code><br>

CTRL + X to exit
Y to save
and Enter to confirm

Now restart the Pi and the screen should no longer turn off after 10 minutes of inactivity. To restart, perform the following:
<code>sudo reboot</code><br>

Set to auto run to show photos
To install the package, use the following line:
<code>sudo apt-get install feh</code><br>

Now, to test that it works, type the following line. Replace /home/pi/desktop/Fotos with the directory that contains your image.

<code>DISPLAY=:0.0 XAUTHORITY=/home/pi/.Xauthority /usr/bin/feh --quiet --preload --randomize --full-screen --reload 60 -Y --slideshow-delay 15.0 /home/pi/desktop/Fotos & </code><br>

Extra 3. Now if you want we can use short tags to make this command much shorter. You can read more about all the flags you can use in the feh manpage, or use the one above normally.
<code>DISPLAY=:0.0 XAUTHORITY=/home/pi/.Xauthority /usr/bin/feh -q -p -Z -F -R 60 -Y -D 15.0 /home/pi/desktop/Fotos & </code><br>

Now, as you'll notice, this crashes the command line bar. To fix this, add the <code>&</code> after the command and the script/process will start in the background.
So now let's store this in a simple script file. That way you can add or change it later. To make the file, type the following command:
<code>sudo nano /home/pi/start-picture-frame.sh</code><br>

Here, type the following lines.
<code>#!/bin/bash </code><br>
<code>DISPLAY=:0.0 XAUTHORITY=/home/pi/.Xauthority /usr/bin/feh --quiet --preload --randomize --full-screen --reload 60 -Y --slideshow-delay 15.0 /home/pi/desktop/Fotos </code> <br>
I prefer without the <code>--preload</code>, you can test

Now ready, you can test it by running the following command.
<code>bash /home/pi/start-picture-frame.sh </code><br>

Configuring to start together with Raspbian
Open the file <code>/etc/profile</code>, with command <code> sudo nano /etc/profile</code>

At the end of the file, insert the command that will run, for example:
<code>bash /home/pi/start-picture-frame.sh & </code> & is necessary

Salve e saia
Reiniciando, deverá executar automaticamente
Demora um puco até carregar as fotos e iniciar os slides de fotos, pode observar o cache do processador carregando

save and exit
Rebooting should automatically run
It takes a while to load the photos and start the photo slides, you can see the processor cache loading, to open almost immediately I prefer without the <code>--preload </code> available in the library

Source
http://www.lcdwiki.com/5inch_HDMI_Display
http://www.lcdwiki.com/res/Show_Direction_and_Touch/How_to_calibrate_the_resistance_touch_screen-V1.2.pdf
https://devblog.drall.com.br/raspberry-pi-raspbian

https://linux.die.net/man/1/feh

https://pimylifeup.com/raspberry-pi-photo-frame/

https://cadernodelaboratorio.com.br/2015/06/10/inicializando-um-programa-automaticamente-no-raspberrypi/
