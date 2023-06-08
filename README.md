# PID Project
For this project we were asked to make a project that uses PID in some way. We choose a simple but difficult project. A box with a wheel in which a photo interrupter would read the amount of rotations and display on an LCD screen. A potentiometer would be used to change the wheels speed.
## Table of Contents
- [Potential Obstacles](#Potential-Obstacles)
- [Milestones](#Milestones)
- [Materials](#Materials)
- [CAD](#CAD)
- [Parts Design](#Parts-Design)
- [Code](#Code)
- [Wiring](#Wiring)
- [Evidence](#Evidence)
- [Reflection](#Reflection)
## Planning
Here are some sketches we made for planning.

<img src="https://github.com/matthewbowling123/PID/assets/112979288/6cf79025-4b5f-4382-aebb-074ca1e655b1" alt="wiring2" style="width:318px;">

## Potential Obstacles
| Problem  | Solution |
| ------------- | ------------- |
| Figuring out how to code PID | Several students have already done this and can help teach us. Also there are many online resources.|
| making sure the design works for the objective | We make a proof of concept and check with Mr. Dierof for approval. Thankfully he did approve of our design. |
| Learning new CPython Code. | Though we have used CPython all this year, this assignment is more difficult code in general. We will be researching PID throughout the creation of the project and ask for help when 
## Milestones
4/21

Start planning to figure out what we're going to do.And start cad

4/28 Have CADprototype made,other person work on code 

5/4 Start printing the box and run into problems with design, other person still researches code and helps the other person as they need.

5/11 Have coding ideas, if you don’t keep researching there a good chance that your not close in coding. Have an updated version of the PID design.

5/18 Fix any problems with coding and if there's any problem with the box get it done 

5/25 Fix any problems with the code

6/2 Have Project done

## Materials
This project requires a 3d printer and laser cutter as well as these parts.
* [One Adafruit Metro Express](https://learn.adafruit.com/adafruit-metro-m0-express/circuitpython)
* one Photointerrupter
* a Breadboard
* an LCD (If you want it displayed on one)
* A Tip 120
* Several wires
* a switch

## CAD
[Here is a link to our CAD](https://cvilleschools.onshape.com/documents/add9f72dd6248fd21fa5ebc4/w/ac75cde634b3c854afb69390/e/851fd580e5be618cd14de4cc)
![Screenshot 2023-05-31 150505](https://github.com/matthewbowling123/PID/assets/112979288/a2c612d7-2942-46ae-8800-166f9458ef86)
This CAD was comprised of a box held together by brackets which contained a Metro, An LCD screen, a Photointeruptor, and a Motor. There was also a battery pack outside the box. Because of how simple the design is, there was very minimal difficulty creating this CAD. Thankfully only one part needed to be 3D printed which ultimately limited the cost of the project.
## Parts Design
| **Wheel** | **Main Box** |
|--------|--------------|
| **Description:** This Wheel was designed to fit on a DC Motor and have a Potentiometer read the time between each gap. It is then multiplied by 4 to get the full RPM | **Description:** This main Box is just 4 walls held together by brackets. It also includes holes for the Motor, Photointeruptor, and wire for the Metro. The 3D part is what holds the Motor in place |
| <img src="https://github.com/matthewbowling123/PID/assets/112979288/b335818e-2e63-44ad-b080-75fae758f11f" alt="wiring2" style="width:318px;"> | <img src="https://github.com/matthewbowling123/PID/assets/112979288/01a4756b-cbb0-41dc-900e-8ac99ba3ce53" alt="wiring2" style="width:318px;">
  
```python
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
pid = PID(1, 0, 0, setpoint=setpoint)  # Sets the setpoint and the motor speed variables
pid.output_limits=(20000, 50000)

Bvalue = False
AverageT = 0
Period= 0
Tcount = 0
Processed = True       # Set variables that will be used in later code
Diffrence = 0
Currenttime = 0
Pasttime = 0
Interupts = 0
# pid = PidLib.PID(5, 0.01, 0.1, setpoint= setpoint)

motor = pwmio.PWMOut(board.D7)   # Motor Pin
motor.duty_cycle = 0


#i2c = board.I2C()
#lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)  # Set up LCD

inter = DIO.DigitalInOut(board.D11)
inter.direction = DIO.Direction.INPUT
inter.pull = DIO.Pull.UP

#motor =  pwmio.PWMOut(board.D7)
#motor.duty_cycle = 65535``        # Set up motor and show it runs on serial monitor.
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
    #print("Interupts", interrupts)   #Prints the amount of interupts every time the Photointeruptior reads a hole in the wheel.
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
    if inter.value == True and Processed == True:       # Set the differant time variables
        Currenttime = Truetime
        Diffrence = Currenttime - Pasttime
        Pasttime = Currenttime
        Processed = False
        Tcount = Tcount + 1
        print(Tcount*4)               # There are 4 evenly dispersed holes so we mulitply by 4 to get full RPM.
        print(Diffrence)
        print(1/(Diffrence*4/60))
    if inter.value == False:
        AverageT = AverageT + Diffrence
        Processed = True
    
    control = int(pid(Tcount*4))
    print(control)
    motor.duty_cycle = control

```

though this code is not 100% someone else's, it is mostly taken from other students' Githubs. Most notably Jakob Weder who I got the RPM code from.
## Wiring
![Screenshot 2023-05-31 145847](https://github.com/matthewbowling123/PID/assets/112979288/045763e9-960e-40c3-aca9-a4106132aca4)
## Evidence


https://github.com/matthewbowling123/PID/assets/112979288/06107892-84c8-41d0-a37a-12b61f05deea

<img src="https://github.com/matthewbowling123/PID/assets/112979288/accf2bc5-f720-49bc-b269-8552db595657" alt="wiring2" style="width:500px;">

## Reflection

### Reflection Matthew

This assignment ultimately proved itself very difficult for my partner and I. Though the Cad design itself was very simple, The code took a lot of effort to create and much of it was borrowed from other people. A big thanks to all three Weders for their code and their brilliant documentations which I learned a lot from. I think ultimately if we had more time planning out the project we would have gotten further. 

Here are GitHubs that were helpful:
* [Paul](https://github.com/japhero)
* [Anton and Sophie](https://github.com/aweder05/PID-Project-Engineering-3-Sophie-Anton)
* [Jakob](https://github.com/Jweder06/Centrifuge)
### Reflection Aaron

Everything was going smooth except for the code.Me and my partner couldn’t find anything online about the code.We research for a couple weeks and couldn’t find anything till we did.But it took us to the last minute to get it working.It works good now.That went a little off of the goal we had but we adjusted and made improvements, we expect for us to struggle with this sense were not that good a python coding .The planning and CAD was easy for us we got the final version done in just a couple weeks.With no big problems to deal with.


