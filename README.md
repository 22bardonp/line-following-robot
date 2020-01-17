# line-following-robot

#!/usr/bin/env pybricks-micropython

from pybricks import ev3brick as brick
from pybricks.ev3devices import (Motor, TouchSensor, ColorSensor,
                                 InfraredSensor, UltrasonicSensor, GyroSensor)
from pybricks.parameters import (Port, Stop, Direction, Button, Color,
                                 SoundFile, ImageFile, Align)
from pybricks.tools import print, wait, StopWatch
from pybricks.robotics import DriveBase

# Write your program here
motorA=Motor(Port.A)
motorB=Motor(Port.B)
sensor=ColorSensor(Port.S1)
color = sensor.color()
delay = 4000

# function to move the robot forward when it senses a color
def drive():
    color = sensor.color()
    print(color)
    motorB.run(100)
    motorA.run(100)
    for i in range(50):
        wait(1)
        if color == None:
            print(color)
            scanAfor()
    wait(50)
    motorA.run(0)
    motorB.run(0)
    wait(500)

# functions to make the robot turn left and right to scan for color when it passes the colored line
def scanAfor():
    print('scanAfor')    
    motorA.run(100)
    for i in range(100):
        color = sensor.color()
        wait(1000)
        print(color)
        if color != None:
            print(color)
            return
        wait(delay)
        motorA.run(0)
        wait(200)
        scanAback()

def scanAback():
    print('scanAback')
    motorA.run(-100)            
    for i in range(100):
        color = sensor.color()
        wait(1000)
        print(color)
        if color != None:
            print(color)
            return
        wait(delay)
        motorA.run(0)
        wait(200)
        scanBfor()

def scanBfor():
    print('scanBfor')
    motorB.run(100)
    color = sensor.color()
    for i in range(100):
        wait(1000)
        print(color)
        if color != None:
            print(color)
            return
        wait(delay)
        motorB.run(0)
        wait(200)
        scanBback()

def scanBback():
    print('scanBback')
    motorB.run(-100)
    color = sensor.color()
    for i in range(100):
        wait(1000)
        print(color) 
        if color != None:
            print(color)
            return
        wait(delay)
        motorB.run(0)
        wait(200)
        scanAfor()

# tells the robot what to do upon initialization (if it senses color, run drive. If it doesm't run the first scan function.)
while True: 
    color = sensor.color()
    print(color)
    if color == None:
        scanAfor()
    
    else:  
        drive()
