# CFI_Elec_Club_mini_task_4
Debugging two Projects
### [ 1. Raspberry Pi radio](https://www.instructables.com/id/Raspberry-Pi-Radio/)
Rotary encoder==> ESP32==> Audio amplifier==> Speaker
<br />First we have to check whether all parts are working individually or not. 


<br />For rotary encoder connect VCC of rotary encoder to pin 1 and gnd to pin 9, clk to pin 5 & DT (Data transfer) pin  to pin 7 of rotary encoder. We can also check it with ESP32 connecting its VCC and GND respectively and for DT and clk pin we can program ESP32. Since we have to check Volume button also we can check it by running code in RPi/ESP32 and receiving values on serial monitor.

```

import RPi.GPIO as GPIO
import os
import time, shlex, subprocess 

GPIO.setmode(GPIO.BOARD)
CurChannel = 0
# Put here all radio channels. Format: Name, URL (Name will be pronounced by TTS)
Channels = [
   'Radio 1', 'http://icecast.omroep.nl/radio1-sb-mp3', # Radio 1
   'Radio 2', 'http://icecast.omroep.nl/radio2-sb-mp3', # Radio 2
   '3 FM','http://icecast.omroep.nl/3fm-sb-mp3', # 3FM
   'Radio 4','http://icecast.omroep.nl/radio4-sb-mp3', # Radio 4
   'Radio 5','http://icecast.omroep.nl/radio5-sb-mp3', # Radio 5
   'Bay N Err','-playlist http://bnr.cdp.triple-it.nl/playlists/bnr_mp3_96_06.pls', # BNR
   'L 1','-playlist http://icecast.omroep.nl/l1-radio-bb-mp3.m3u', # L1 (Omroep Limburg)   
   #'Studio Brussell','http://mp3.streampower.be/stubru-high.mp3', # Studfio Brussel
   #'Gelderland','http://stream.omroepgelderland.nl/radiogelderland', # Omroep Gelderland
   'Q Music','http://icecast-qmusic.cdp.triple-it.nl/Qmusic_nl_live_96.mp3' # QMusic
]

PIN_A = 5 # CLK Geel
PIN_B = 7 # CLK Oranje

VOL_UP = 38
VOL_DOWN = 40

Last=0
Prev=0

def shutdownRadio(dummy):
    #print "Houdoe"
    #raise SystemExit
    os.system("sudo halt")

def Rot(channel):
    global Last
    global Prev
    
    level = GPIO.input(channel)
    Last = str(channel)+":"+str(level)
    print str(Prev)+" - "+Last
    #if Last<>Prev: ChangeRadioChannel(1)
    if Last=="5:0" and Prev=="7:0": ChangeRadioChannel(1)
    #elif Last=="7:0" and Prev=="7:0":
    #else: ChangeRadioChannel(-1)
    Prev = Last

```

<br />2. Second step would be to check whether ESP32/RPi is working properly or not for the transmission or receiving of data and the pins we are using. If not, then get a new RPi/ESP32 board.

<br />3. Now we can check the speakers by connecting the wires with an ordinary device like smartphone or computer and check whether it is receiving or not and if not get new speakers.

<br />4. Fourth step would be to check whether after connecting RPi to the speakers it is working or not.

<br />5. Check the connections properly.

<br />6. Now, if the radio after connecting parts properly still doesn't work, it might be a problem because of code doesn't working properly.

---

### [2. Connect BLE Weather Sensor to the Cloud](https://www.hackster.io/ble-weather-aws/connect-ble-weather-sensor-to-the-cloud-e79d9d)













