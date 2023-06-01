# PID Project
For this project we were asked to make a project that uses PID in some way. We choose a simple but difficult project. A box with a wheel in which a photointeruptor wuld read the amount of rotationd and display on an LCD screen. A rotary ecoder would be used to change the wheels speed.
## CAD
[Here is a link to our CAD](https://cvilleschools.onshape.com/documents/add9f72dd6248fd21fa5ebc4/w/ac75cde634b3c854afb69390/e/851fd580e5be618cd14de4cc)
![Screenshot 2023-05-31 150505](https://github.com/matthewbowling123/PID/assets/112979288/a2c612d7-2942-46ae-8800-166f9458ef86)
This CAD was comprised of a box held together by brackets which contained a Metro, An LCD screen, a Photointeruptor, and a Motor. There was also a batery pack outside the box. Because of how simple the design is, there was very minimal difficulty creating this CAD. Thankfully only one part needed to be 3D printed which ultimatly limited the cost of the project.
## Code
```

import board
import time
import pwmio
import analogio as AIO
import digitalio as DIO
#from lcd.lcd import LCD
#from lcd.i2c_pcf8574_interface import I2CPCF8574Interface
#import PID_CPY as PidLib

setpoint = 5

Bvalue = False
AverageT = 0
Period= 0
Tcount = 0
Processed = True
Diffrence = 0
Currenttime = 0
Pasttime = 0
Interupts = 0
#pid = PidLib.PID(5, 0.01, 0.1, setpoint= setpoint)

motor = pwmio.PWMOut(board.D7)
motor.duty_cycle = 0


#i2c = board.I2C()
#lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)

inter = DIO.DigitalInOut(board.D11)
inter.direction = DIO.Direction.INPUT
inter.pull = DIO.Pull.UP

#motor =  pwmio.PWMOut(board.D7)
#motor.duty_cycle = 65535``
#print("running")
#time.sleep(0.15)

#  these are the variables needed for the code
intTime =0
interrupts =0
time1 = 0
time2 = 0
RPM = 0
lastVal = False
photoVal = False           
oldPhotoVal = False # Used to make sure we only count the first loop when interupt is broken

while True:
    #print("Interupts", interrupts)
    #motor.duty_cycle = 65000
   # time.sleep(1)
   # motor.duty_cycle = 0
   # time.sleep(5)
    motor.duty_cycle = 5000
   # time.sleep(1)
   # motor.duty_cycle = 0
   # time.sleep(5)

 #  Prevents interrupter from interrupting more than once per interrupt
    photoVal = inter.value 
    
    '''
    though this code is not 100% someone elses, it is mostly taken from other students Githubs. Most notably Jakob Weder who I got the RPM code from.
    if photoVal and (oldPhotoVal == False):
        oldPhotoVal = True
        interrupts += 1
    if not photoVal:
        oldPhotoVal  = False'''

    Truetime = time.monotonic()
    if inter.value == True and Processed == True:       # Encoder
        Currenttime = Truetime
        Diffrence = Currenttime - Pasttime
        Pasttime = Currenttime
        Processed = False
        Tcount = Tcount + 1
        print(Tcount*4)
        print(Diffrence)
    if inter.value == False:
        AverageT = AverageT + Diffrence
        Processed = True

```
## Wiring
![Screenshot 2023-05-31 145847](https://github.com/matthewbowling123/PID/assets/112979288/045763e9-960e-40c3-aca9-a4106132aca4)
## Reflection
This assignment ultimatly proved itself very difficult for my partner and I. Though the Cad design itself was very simple, The code took a lot of effort to create and much of it was borrowed from other people. A big thanks to all three Weders for their code and their brilliant documentations which I learned a lot from. I think ultimatly if we had more time planning out the project we would have gotten further.
