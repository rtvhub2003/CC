# [Step1: In minicom terminal]
<br>* check if IP Address is assigned.

<br>`ifconfig`

<br>* If IP Address is not assigned, set a temporary ip address first.

<br>`ifconfig eth0 192.168.3.2 netmask 255.255.248.0`

<br>* Verify IP Address

# [Step2: Laptop terminal]
<br>* Access SSH terminal, and provide ssh-keygen and fingerprints if required

<br>`ssh root@192.168.3.2`

# [Step3: Laptop]
<br>* Visit inside 'CC_G2L' folder and perform below command for file transfer

<br>`./installer.sh 192.168.3.2`

<br>Note: It'll take a few minutes and your CC will be programmed automatically.

# [Step4: minicom terminal]

<br>* `ls`
<br>* `ifconfig`

# [Step5 (not required) : Minicom | For Manual testing]

## remote [music + live]:

`gst-launch-1.0 -v \
udpsrc port=5000 \
caps="application/x-rtp,media=audio,payload=96,clock-rate=12000,encoding-name=L24,channels=2" \
! rtpL24depay \
! audioconvert \
! alsasink`


# [ Step6: Laptop]

## host/laptop [music testing]:

`gst-launch-1.0 -v \
filesrc location=corporatetime-tutorial-tutorial-music-512462.mp3 \
! decodebin \
! audioconvert \
! audioresample \
! audio/x-raw,rate=12000,channels=2 \
! rtpL24pay pt=96 \
! udpsink host=192.168.3.2 port=5000`
<br> And check the audio play whether you're able to hear any sound.

## host/laptop [Live announcement/mic testing]

`gst-launch-1.0 -v \
alsasrc \
! audioconvert \
! audioresample \
! audio/x-raw,rate=12000,channels=2 \
! rtpL24pay pt=96 \
! udpsink host=192.168.3.3 port=5000`

<br> Speak on mic, and check the live announcement


[END]

# ____________________________________
# ____________________________________
# ____________________________________
# USING COMMANDLINE INTERFACE

# [Step1: In minicom terminal]
<br>* check if IP Address is assigned.

<br>`ifconfig`

<br>* If IP Address is not assigned, set a temporary ip address first.

<br>`ifconfig eth0 192.168.3.2 netmask 255.255.248.0`

<br>* Verify IP Address

# [Step2: Laptop terminal]
<br>* Access SSH terminal, and provide ssh-keygen and fingerprints if required

<br>`ssh root@192.168.3.2`

# [Step3: Laptop]
<br> Visit inside 'CC_G2L_Scratch' folder and perform below command for file transfer
### Transfer files To /home/root/
### [Laptop]
`scp button_audio.sh root@192.168.3.2:/home/root/`
`scp cc_audio_control.sh root@192.168.3.2:/home/root/`
`scp cc_volume_server.py root@192.168.3.2:/home/root/`
`scp ip_set_icdu.py root@192.168.3.2:/home/root/`
`scp start_multicastEmu.sh root@192.168.3.2:/home/root/`
`scp testaudio.wav root@192.168.3.2:/home/root/`

### Transfer service files To /etc/systemd/system/
### [Laptop]
`scp button-control.service root@192.168.3.2:/etc/systemd/system/`
`scp cc_audio.service root@192.168.3.2:/etc/systemd/system/`
`scp cc-control.service root@192.168.3.2:/etc/systemd/system/`
`scp cc-volume.service root@192.168.3.2:/etc/systemd/system/`
`scp network-activation.service root@192.168.3.2:/etc/systemd/system/`

### Set execute permissions 
### [Minicom]
`chmod a+x button_audio.sh`
`chmod a+x cc_audio_control.sh`
`chmod a+x start_multicastEmu.sh`

### Activate GPIO pins
### [Minicom]
`echo 144 > /sys/class/gpio/export`
`echo in > /sys/class/gpio/P3_0/direction`
`echo 1 > /sys/class/gpio/P3_0/value`
`echo 162 > /sys/class/gpio/export`
`echo out > /sys/class/gpio/P5_2/direction`
`echo 1 > /sys/class/gpio/P5_2/value`

### Enable and Activate services
### [Minicom]
`systemctl daemon-reload`
`systemctl enable /etc/systemd/system/cc-volume.service`
`systemctl enable /etc/systemd/system/button-control.service`
`systemctl enable /etc/systemd/system/cc-audio.service`
`systemctl enable /etc/systemd/system/cc-control.service`
`systemctl enable /etc/systemd/system/network-activation.service`

### Restart your device to apply changes
### [Minicom]
`reboot`

[END]
