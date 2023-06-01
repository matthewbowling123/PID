# PID Project
For this project we were asked to make a project that uses PID in some way. We choose a simple but difficult project. A box with a wheel in which a photointeruptor wuld read the amount of rotationd and display on an LCD screen. A rotary ecoder would be used to change the wheels speed.
## Milestones
4/21

Start planning to figure out what we're going to do.And start cad

4/28

Have CADprototype made,other person work on code 

5/4

Start printing the box and run into problems with design, other person still researches code and helps the other person as they need.

5/11

Have coding ideas, if you don’t keep researching there a good chance that your not close in coding. Have an updated version of the PID design.

5/18

Fix any problems with coding and if there's any problem with the box get it done 

5/25

Fix any problems with the code

6/2

Have Project done


## CAD
[Here is a link to our CAD](https://cvilleschools.onshape.com/documents/add9f72dd6248fd21fa5ebc4/w/ac75cde634b3c854afb69390/e/851fd580e5be618cd14de4cc)
![Screenshot 2023-05-31 150505](https://github.com/matthewbowling123/PID/assets/112979288/a2c612d7-2942-46ae-8800-166f9458ef86)
This CAD was comprised of a box held together by brackets which contained a Metro, An LCD screen, a Photointeruptor, and a Motor. There was also a batery pack outside the box. Because of how simple the design is, there was very minimal difficulty creating this CAD. Thankfully only one part needed to be 3D printed which ultimatly limited the cost of the project.
## Parts Design
| **Wheel** | **Main Box** |
|--------|--------------|
| **Description:** This Wheel was designed to fit on a DC Motor and have a Potentiometer read the time between each gap. It is then multiplied by 4 to get the full RPM | **Description:** This main Box is just 4 walls held together by brackets. It also includes holes for the Motor, Photointeruptor, and wire for the Metro. The 3D part is what holds the Motor in place. | 
| <img src="https://github.com/matthewbowling123/PID/assets/112979288/79244617-f7f6-4d2d-9497-ed85317591c0)"> |<img src="https://github.com/matthewbowling123/PID/assets/112979288/4e816ce4-3896-4531-8348-2e51169749e2" alt="wiring2" style="width:318px;"> |         
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
from lib.PID import PID


setpoint = .005
pid = PID(1, 0, 0, setpoint=setpoint)
pid.output_limits=(20000, 50000)

Bvalue = False
AverageT = 0
Period= 0
Tcount = 0
Processed = True
Diffrence = 0
Currenttime = 0
Pasttime = 0
Interupts = 0
# pid = PidLib.PID(5, 0.01, 0.1, setpoint= setpoint)

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
    # motor.duty_cycle = 5000
   # time.sleep(1)
   # motor.duty_cycle = 0
   # time.sleep(5)

 #  Prevents interrupter from interrupting more than once per interrupt
    photoVal = inter.value 
    
    '''
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
        print(1/(Diffrence*4/60))
    if inter.value == False:
        AverageT = AverageT + Diffrence
        Processed = True
    
    control = int(pid(Tcount*4))
    print(control)
    motor.duty_cycle = control

```

though this code is not 100% someone elses, it is mostly taken from other students Githubs. Most notably Jakob Weder who I got the RPM code from.
## Wiring
![Screenshot 2023-05-31 145847](https://github.com/matthewbowling123/PID/assets/112979288/045763e9-960e-40c3-aca9-a4106132aca4)
## Evidence


https://github.com/matthewbowling123/PID/assets/112979288/06107892-84c8-41d0-a37a-12b61f05deea

<img src="https://github.com/matthewbowling123/PID/assets/112979288/accf2bc5-f720-49bc-b269-8552db595657" alt="wiring2" style="width:500px;">

## Reflection
This assignment ultimatly proved itself very difficult for my partner and I. Though the Cad design itself was very simple, The code took a lot of effort to create and much of it was borrowed from other people. A big thanks to all three Weders for their code and their brilliant documentations which I learned a lot from. I think ultimatly if we had more time planning out the project we would have gotten further.
