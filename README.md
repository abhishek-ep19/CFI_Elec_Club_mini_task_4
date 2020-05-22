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

<br />
&nbsp;
<p align="center">
  <img width="450" height="360" src="https://user-images.githubusercontent.com/64124723/82667230-0946a780-9c55-11ea-8d12-f990b4582a58.png">
  <br />How data transfer takes place through Bluetooth module
</p>
<br />
&nbsp;

This image is enough of showing how bluetooth module works so we have to check that whether the bluetooth module is working properly or not. Since, we have ideated differently in mini project 2 for this we will use NodeMCU/ESP32 for directly transferring data to AWS cloud.
<br />But for reference, if we are just replacing RPi with Arduino/ESP32:
##### Testing Bluetooth 

```
char Incoming_value = 0;                //Variable for storing Incoming_value
void setup() 
{
  Serial.begin(9600);         //Sets the data rate in bits per second (baud) for serial data transmission
}
void loop()
{
  if(Serial.available() > 0)  
  {
  
    Incoming_value = Serial.read();      //Read the incoming data and store it into variable Incoming_value
    Serial.print(Incoming_value);        //Print Value of Incoming_value in Serial monitor
    Serial.print("\n");        //New line 
  }                            
} 
```
Put this code into your Arduino IDE and Upload the code. ( !! REMEMBER to Unplug the TX , RX from Arduino always when you upload code ) 
Now , Open your Robo Remo App and The Serial Monitor of Arduino and see if you can 'f' ,'b'  when you press the up key and back key respectively. 
<br />If you can , then Great , your Bluetooth Module is Working Properly. Move on to the Next Step. 

<br />Since here we don't need to use Bluetooth module we can check ESP32 only for all purposes after receiving data from Weather Sensor.

<br />During this state, the MCU reads data from the weather sensors and updates the characteristic values.
<br />What we can do is to connect the sensor with the arduino board or ESP32 to check whether the sensor is working properly or not. If the the values are not correctly receiving the values we can again receive the values.
<br />Connect the ESP32 to Wifi to upload the code.
```

WiFi.begin(SSID,PASSWORD);

#include <WiFi.h>        // Include the Wi-Fi library
 
const char* ssid     = "SSID";         // The SSID (name) of the Wi-Fi network you want to connect to
const char* password = "PASSWORD";     // The password of the Wi-Fi network
 
void setup() {
  Serial.begin(115200);         // Start the Serial communication to send messages to the computer
  delay(10);
  Serial.println('\n');
  
  WiFi.begin(ssid, password);             // Connect to the network
  Serial.print("Connecting to ");
  Serial.print(ssid);
 
  while (WiFi.status() != WL_CONNECTED) { // Wait for the Wi-Fi to connect
    delay(500);
    Serial.print('.');
  }
 
  Serial.println('\n');
  Serial.println("Connection established!");  
  Serial.print("IP address:\t");
  Serial.println(WiFi.localIP());         // Send the IP address of the ESP8266 to the computer
}
 
void loop() { 
  
}

```
Now if this is also working properly we can upload the code in ESP32 to upload the sensor values to AWS cloud and if it still not works check your code again till it works.

---








