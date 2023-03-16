

# **MICRO CHALLENGE II** 

Our area of interest is the social aspect of the weak signal of drug assumption at music festivals. Substance use at music festivals, dance clubs, and raves is a well-known fact, and we do not intend to justify it. Instead, we focus on creating a non-judgmental and open-minded environment that encourages pragmatic conversation around the use of psychoactive substances. To achieve this, we want to use interactive design and tools to gather data and personal experiences related to the topic. We want to increase dialogue around the topic, which can help reduce stigma and discrimination, mitigating harm. 


The main idea is to develop a safe and inclusive space for discussing the use of psychoactive substances. Through a physical installation and a complementary VR experience, users can share their personal experiences anonymously and learn from others' stories in a controlled environment. Our aim is to foster an immersive and collaborative dialogue on this topic.
We wanna help people to communicate their opinion and experiences about drugs in a way that can allow them to feel comfortable in sharing personal opinions and experiences. 

During the micro challenge II we developed the physical installation called "The Confessional". We're developing the VR indipendently.
The Confessional is a wood structure (CnC, lasercut) that can host one person at a time. It's not supposed to fully cover the person, cause we don't believe in shame or guilt in drug assumption, but just to isolate in order to create a personal space that helps a flow of thoughs. As the user walks in, a blu led turns on and the recording starts. 
We give people 100 seconds to share their experience, the limit of time is needed because the recordings are then gonna be used in the VR immersive installation. 





*REFERENCES*

- STRUCTURE: The whole idea is based on the concept or implement the dialogue about drug consuption as hard reduction method. The "Dutch way", liberal and open minded, was our main inspiration. The lack of communication and dialogue brings a huge lack of awareness and information in the topic itself. Also, sharing personal experiences can prevent harm to other people.
The traditional confessionals are a space for people to tell their sins to a priest, be listened and receive holy forgivness. We transalted this to modern concept, where there's no priest listening to you but yourself, and a microphone that records. 


https://theinternationalangle.com/index.php/2019/04/12/the-normality-of-drug-use-at-festivals-in-the-netherlands/

https://dutchreview.com/dutch-quirks/dutch-quirk-56-have-a-liberal-attitude-towards-party-drugs/


https://harmreduction.org/about-us/principles-of-harm-reduction/

- RASPBERRY PI:

https://makersportal.com/blog/2018/8/23/recording-audio-on-the-raspberry-pi-with-python-and-a-usb-microphone

https://iot4beginners.com/recording-audio-on-your-raspberry-pi/

https://roboticsbackend.com/raspberry-pi-control-led-python-3/





Since that we are a group or two, we decided to devide the tasks: 
- physical structure: we already had the CnC and laser cut parts. Since that we're planning to move the structure around, we have to make it removable cause it would be too heavy to carry around as one whole piece. We're gonna use screws but we don't want to ruin the wood, so we decided to 3D print support pieces for the planks with inserters for the screws.
- rasbperry pi and python to program the fuction or light, recording and distance, using a LED, a ultrasonic distance detector and a microphone. 






# **PHYSICAL STRUCTURE**

Material needed: 
- plywood 4mm (lasercut)
- wood 15mm (CnC) 
- 3D printed plastic junctions
- screws
- m5 inserts
- m4 inserts

We first designed together the physical structure on Rhino, struggling with the quantity of material that we had available and the average sizes of human beings. (170 - 180 cm). In order not to waste material, we manage to optimise the space available on the board.

*Problem 1*: At first, we were planning to do it all with the CnC but due to the limit of material available we decided to do the top part laser cut in plywood.
![](https://i.imgur.com/nh6Yxwu.jpg)


![](https://i.imgur.com/xGg95it.jpg)
![](https://i.imgur.com/FbmIEGA.png)

*Problem 2*: Once that it was printed we had to reduce the size of average 20cm ish, fortunately we designed the botton part so that I could be shorten up. 

![](https://i.imgur.com/CY4ahKm.jpg)

*Problem 3*: We tried to put the pieces together, the structure was pretty solid and stable but for safety reasons we preferred to add some screws BUT we still wanted the structure to be adjustable on the spot (we plan to carry it around and as a whole would be too heavy and not very smart). We decided to 3D print support pieces for the screws, not to damage the wood and so that the strcture is easily transportable because designed to be disasseblmed.

![](https://i.imgur.com/93B7VDH.jpg)


*Problem 4*: We run out of inserts. Bought them very last minute.

*Problem 5*: When we tried to drill in the screws on the 4mm plywood, it was either breaking or slipping aside. We fixed the Rhino file adding holes to the design. 

![](https://i.imgur.com/o2Jovg2.jpg)

<img width="1219" alt="Screenshot 2023-03-16 at 15 36 53" src="https://user-images.githubusercontent.com/115195638/225655469-a6c00a13-efb4-4e02-8ff6-6383b473932f.png">

*Problem 6*: Some of our structure 3D Printed pieces were too brittle, we printed them again with a greater amount of walls.

![](https://i.imgur.com/aXNwuW4.jpg)

# **RASPBERRY PI**

Material needed: 

- raspberyy pi
- breadboard
- USB microphone
- 3 330 Ohm resistors
- 1 led
- male to female wires


![](https://i.imgur.com/j6AHWOi.png)

![IMG_3500](https://user-images.githubusercontent.com/115195638/225609342-7b38a2bf-aecb-4302-971a-409af2cd643a.JPG)

We started working on three different files, one for the distance detector, one for the led, one for the microphone to then blend them together in a final one. 
- In the first one we connected the detector to measure distances, and we set an area that represents our area of interest when we want the recording to    start. Since that the lateral measure of our box is 40 cm we set the area of interest as 35 cm, so that the area is defined once that the person is already inside. 
<img width="367" alt=" " src="https://user-images.githubusercontent.com/115195638/225652500-6762d770-9ef6-43a9-99c5-8d1712a7bc7f.png">

**distance measurement code**

#Libraries
import RPi.GPIO as GPIO
import time
 
#GPIO Mode (BOARD / BCM)
GPIO.setmode(GPIO.BCM)
 
#set GPIO Pins
GPIO_TRIGGER = 18
GPIO_ECHO = 24
 
#set GPIO direction (IN / OUT)
GPIO.setup(GPIO_TRIGGER, GPIO.OUT)
GPIO.setup(GPIO_ECHO, GPIO.IN)
 
def distance():
    # set Trigger to HIGH
    GPIO.output(GPIO_TRIGGER, True)
 
    # set Trigger after 0.01ms to LOW
    time.sleep(0.00001)
    GPIO.output(GPIO_TRIGGER, False)
 
    StartTime = time.time()
    StopTime = time.time()
 
    # save StartTime
    while GPIO.input(GPIO_ECHO) == 0:
        StartTime = time.time()
 
    # save time of arrival
    while GPIO.input(GPIO_ECHO) == 1:
        StopTime = time.time()
 
    # time difference between start and arrival
    TimeElapsed = StopTime - StartTime
    # multiply with the sonic speed (34300 cm/s)
    # and divide by 2, because there and back
    distance = (TimeElapsed * 34300) / 2
 
    return distance
 
 if __name__ == '__main__':
    try:
        while True:
            dist = distance()
            print ("Measured Distance = %.1f cm" % dist)
            if dist <= 35:
                print ("we are in the area of interest")
            time.sleep(1)
 
         Reset by pressing CTRL + C
    except KeyboardInterrupt:
        print("Measurement stopped by User")
        GPIO.cleanup()

- The script for the led was the most simple one, basically just connecting the LED with an on and off function.
- The last script is the one that triggered us the most.
  *Problem 1*: We first started with a microphone that unfortunately didn't work. Then we switched to usb headphones with microphone (we're just using the microphone). We managed to actually record, to play the recording we first moved them in a USB and then in the laptop to check. 
  
**Microphone code**   

Import pyaudio
import wave
 
FORMAT = pyaudio.paInt16
CHANNELS = 1
RATE = 44100
CHUNK = 1024
RECORD_SECONDS = 5
WAVE_OUTPUT_FILENAME = "file.wav"
 
audio = pyaudio.PyAudio()
 
 start Recording
stream = audio.open(format=FORMAT, channels=CHANNELS,
                input_device_index = 1, rate=RATE, input=True,
                frames_per_buffer=CHUNK)
print ("recording...")
frames = []
 
for i in range(0, int(RATE / CHUNK * RECORD_SECONDS)):
    data = stream.read(CHUNK)
    frames.append(data)
print ("finished recording")
 
 
 stop Recording
stream.stop_stream()
stream.close()
audio.terminate()
 
waveFile = wave.open(WAVE_OUTPUT_FILENAME, 'wb')
waveFile.setnchannels(CHANNELS)
waveFile.setsampwidth(audio.get_sample_size(FORMAT))
waveFile.setframerate(RATE)
waveFile.writeframes(b''.join(frames))
waveFile.close()





- Meshing all the files in one only. We realized that we must be very very very tidy when writing a code, otherwise it's really complicated to understand what's going on especially on a long code composed of different elements and parts. 
*Problem 1*: pyaudio segmentation fault - after that the recording starts, works and ends it won't start a new one automatically. We solved the problem by writing a new script that restart the function
<img width="311" alt="Screenshot 2023-03-16 at 17 47 41" src="https://user-images.githubusercontent.com/115195638/225692858-16d4872e-6670-4e47-bb11-23ddfdd8afc7.png">


To run it: sh testrerun.sh
To check the WiFi and add new ones: sudo nano /etc/wpa_supplicant/wpa_supplicant.conf



# **FURTHER DEVELOPMENTS**
All the stories shared in The Confessional will then be translated into a VR experience, which we are going to exhibit at Mostra Festival and we aim to exhibit at Sonar+D. The whole project is called "Unfolding Conversations". We recognize that the use of substances is both literary and personal, where one's experience is only partially narrated by literature on a substance and its effects. Therefore, we believe that personal conversations are essential when discussing the topic. Unfolding Conversation thus aims to provide a safe space for people to share their experiences, creating a database of personal narratives which will be exhibited in subsequent installations. Through the power of storytelling, we hope to promote understanding, reduce stigma and discrimination, and create a more accepting and open attitude towards psychoactive substance consumption, especially in a music fest environment where people might be more keen to be involved in the topic, looking for further safety and comfort. 
