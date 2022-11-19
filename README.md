[![PyPI - Python](https://img.shields.io/pypi/pyversions/iconsdk?logo=pypi)](https://pypi.org/project/iconsdk)
[![IBM - Node-RED](https://node-red-zhhhd-2022-10-21.eu-gb.mybluemix.net/red/red/images/node-red.svg)](https://node-red-zhhhd-2022-10-21.eu-gb.mybluemix.net/)
# IBM-Project-17183-1659630098
### Project Title: Personal Assistance for Seniors Who Are Self-Reliant
### Team ID: PNT2022TMID21393<br>
### Team Members: <br>
  👨‍💻:Nishok Rajan P (917719D060)<br>
  👨‍💻:Karthikeyan B (917719D036)<br>
  🧑‍💻:Saravanaprakash S (917719D081)<br>
  🧑‍💻:Viswa Bharathi K (917719D112)<br>

## :thought_balloon: Objective:<br>
:star2: Sometimes elderly people forget to take their medicine at the correct time.<br>
:star2: They also forget which medicine He / She should take at that particular time.<br>
:star2: And it is difficult for doctors/caretakers to monitor the patients around the clock. To avoid this problem, this medicine reminder system is developed.<br>
:star2: An app is built for the user (caretaker) which enables him to set the desired time and medicine. These details will be stored in the IBM Cloudant DB.<br>
:star2: If the medicine time arrives the web application will send the medicine name to the IoT Device through the IBM IoT platform.<br>
:star2: The device will receive the medicine name and notify the user with voice commands.<br>

## 🏁Project flow:<br>
<img width= "720" alt="GFG" src="https://user-images.githubusercontent.com/106460690/202840737-8e59d4fc-670b-48ca-a6bc-468a4c4c6fba.png"><br>
## 🔄:Node-RED:
<img width= "720" alt="GFG" src="https://user-images.githubusercontent.com/106460690/202841261-ade9135b-2f99-4312-9340-e333fc839390.png"><br>
## User UI Dashboard:
<img width= "720" alt="GFG" src="https://user-images.githubusercontent.com/106460690/202841382-e7e9b03e-c44c-438f-b676-ed763d932f51.png"><br>
## 💻Project Code
```python
import time
from ibm_watson import TextToSpeechV1
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator
import ibmiotf.device
import pygame
pygame.init() # initiate pygame

config={
    "org":"hg0hll",             # Device Organization
    "type" :"123",              # Device Type
    "id":"abcd",                # Device ID
    "auth-method":"token",      # Device Authentication Method
    "auth-token":"123456789"    # Device Authentication Token
}
url="https://api.eu-gb.text-to-speech.watson.cloud.ibm.com/instances/8e5bc662-02f5-4cc3-b2a3-27086673e789"  # TextToSpeech URL Link
api="QGXbVq1lTgSFNn8_7wpT1kGVYIKCHG8NLfHnC1BBXNwj"                                                          # TextToSpeech API Key
client= ibmiotf.device.Client (config) # Save the device Config in a Varible called client
client.connect()                       # Connect with the device

# Load TextToSpeech API Key and URL
auth=IAMAuthenticator(api)
tts=TextToSpeechV1(authenticator=auth)
tts.set_service_url(url)

# callback
def myCommandCallback (cmd):
    a=cmd.data
    c=1
    instruction="Please Take following Medicine. "
    if len(a["command"])==0:
        pass
    else:
        for i in a["command"]:
            instruction+=str(c)+". "
            instruction+=i
            instruction+=". "
            c+=1
        print("Instruction : ",instruction)
        with open("./speech.wav","wb") as audio_file:
            res=tts.synthesize(instruction,accept="audio/mp3",voice='en-US_AllisonExpressive').get_result()
            audio_file.write(res.content)
        play("speech.wav")

def play(a):
    p=pygame.mixer.Sound(a)
    pygame.mixer.Sound.play(p)
    time.sleep(20)
    pygame.mixer.Sound.play(p)
    time.sleep(20)
    pygame.mixer.Sound.play(p)
    time.sleep(20)

while True:
    client.commandCallback = myCommandCallback
client.disconnect()
```
## Demo Vedio
[Vedio Link](https://drive.google.com/file/d/1a27ixFapTklgvbLxf-ivnIPfo3hBbry8/view?usp=sharing)

