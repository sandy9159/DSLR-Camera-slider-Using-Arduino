# DSLR-Camera-slider-Using-Arduino



![maxresdefault (3)](https://user-images.githubusercontent.com/19898602/125586770-c9e0a89c-81bf-4c57-9be4-b965bad7fba0.jpg)


Hello friends in this video I have made a 2-Axis camera slider, 

by using arduino stepper motor and some aluminium extrusion profile.


A rotary encoder and 0.96" oled display used for user interface

# VIDEO

This the link of complete video you can watch the video from here where I have demonstrated complete steps

in order to build this cool arduino based DSLR Camera slider.

https://youtu.be/u2gcZInumkk


# COMPONENTS USED

> 2 pieces Nema 17 motor


> 10mt of GT2 open belt


> 1 piece of 200mm GT2 closed belt


> 3 pieces of GT2 pitch toothed pulley, 20 teeth


> 1 piece of smooth idler pulley




> 4 pcs of POM coated ball bearings


> 1 piece of limit switch microswitch


> 1 piece of camera holder


> optional: slot nuts



# CIRCUIT DIAGRAM

![image](https://user-images.githubusercontent.com/19898602/125587814-3d92ed40-ac30-4d44-a9e8-7ac1b98173b4.png)



# CUSTOM PCB

![image](https://user-images.githubusercontent.com/19898602/125588570-5cc527d3-79ea-40f4-8323-70093eb0e1d6.png)


![image](https://user-images.githubusercontent.com/19898602/125588592-7213d3f4-ddb1-425a-ad33-9f070ff8d8a1.png)


L293D = 2 pieces


LM7809 = 1 piece


LM7805 = 1 piece


10.1uF electrolytic = 1 piece


1N5408 = 1 piece


1N4007 = 1 piece


led = 1 piece


resistance of at least 470 Ohm = 1 piece




Arduino Nano = 1 piece


Oled display = 1 piece


Encoder = 1 piece


A4988 controller = 2 pieces




socket 8 + 8 = 1 piece


PCB female pin header connectors


screw connectors for printed circuit boards


I have designe a PCB which is multipurpose and order it from [JLCPCB](https://jlcpcb.com/IAT ) 

I always prefer [JLCPCB.com](https://jlcpcb.com/IAT) for my PCB needs, [JLCPCB.com](https://jlcpcb.com/IAT) have best deals for their customers
$2 for 1-4 Layer PCBs, free SMT assembly monthly.

![image](https://user-images.githubusercontent.com/19898602/159014034-3c9a50c3-61c3-40d2-836d-9cadc2317d33.png)


SMT Assembly service of [JLCPCB.com](https://jlcpcb.com/IAT) is cherry on top now get your PCB fully assembled and save your time and money
Select components for your PCB from there Parts Library of 200k+ in-stock components
they are offering $30 valued New User coupons  & $24 SMT coupons every month
$8.00 setup fee, and $0.0017  per joint

Now no need to order components separately for you PCB and get free from stress of soldering them on PCB just try PCB SMT assembly service and get you PCB with components pre assembled and ready for the project


👉 Try PCBA service of [JLCPCB.com](https://jlcpcb.com/IAT) and save your time and money, get PCB ready for project, 200K+ components in library of [JLCPCB.com](https://jlcpcb.com/IAT) as well as 3rd party         parts to choose from. 
    Assembly will support 10M+ parts from Digikey, mouser
    
👉 $30 valued New User coupons 

👉 $24 SMT coupons every month


For more detials & offers please visit [JLCPCB.com](https://jlcpcb.com/IAT)


[Click here to visit JLCPCB.com](https://jlcpcb.com/IAT)



![MVI_0003 00_02_14_01 Still001](https://user-images.githubusercontent.com/19898602/125606831-8dd6d6c4-ac40-4317-abdf-0f2301e3813a.jpg)


I have used the 12mm birch wood to made this side blocks, the dimensions are 60mm on top 

and 80mm on bottom, and have 20mm grove at the top center,

in this grove we can place our 20x20 alu. extrution profile. 

![MVI_0003 00_02_20_04 Still002](https://user-images.githubusercontent.com/19898602/125608925-fbc0463d-53c4-44e8-9fbc-153dd7f96b47.jpg)

This is the main part of our project this are the 20 x 20 Alu. extrution 
the length is as per your need means how long you want to travel camera you can keep
length as per that.

I have used two aluminium extrusion of 20x20 cross section size you can use 20x40 size as well
![MVI_0003 00_04_12_18 Still003](https://user-images.githubusercontent.com/19898602/125609236-921acb3c-c328-491d-8425-7fc7cb3c664d.jpg)

This is the camera slider platform

it have four V shape wheels on the all four corners this wheel fit in the grove of 20 x 20 alu extrusion profile.

I made this platform using 4 mm wood there is also a provision for nema 17 stepper motor.

this motor will pan the camera in 180 degree horizontally

![MVI_0003 00_10_51_12 Still004](https://user-images.githubusercontent.com/19898602/125610072-df3802de-3f36-40da-947e-5c972665faf4.jpg)


![MVI_0003 00_11_16_09 Still005](https://user-images.githubusercontent.com/19898602/125610244-e7f028b7-8a8d-429f-8e13-673a8012491e.jpg)


This is the control unit box which I 3D printen in PLA filament 

its size is 75 x 55 mm I have placed my PCB inside this box and I placed a rotary encoder and 

small OLED screen on the cover of this control unit box 

This rotary encoder and small OLED screen is helpfull for user interface and to display  data on the screen 

# Arduino Code

```

#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <AccelStepper.h>
#include <MultiStepper.h>
#include "bitmap.h"

#define limitSwitch 11
#define PinSW 2
#define PinCLK 3  
#define PinDT 8
#define OLED_RESET 4
Adafruit_SSD1306 display(OLED_RESET);

AccelStepper stepper2(1, 17, 16); // (Type:driver, STEP, DIR)
AccelStepper stepper1(1, 15, 14);

MultiStepper StepperControl; 

long gotoposition[2]; 

volatile long XInPoint=0;
volatile long YInPoint=0;
volatile long XOutPoint=0;
volatile long YOutPoint=0;  
volatile long totaldistance=0;
int flag=0; 
int temp=0;
int i,j;
unsigned long switch0=0;
unsigned long rotary0=0;
float setspeed=200;
float motorspeed;
float timeinsec;
float timeinmins;
volatile boolean TurnDetected;  
volatile boolean rotationdirection;  


void Switch()  
{
 if(millis()-switch0>50)
 {
 flag=flag+1;
 }
 switch0=millis();
}

void Rotary()  
{
delay(75);
if (digitalRead(PinCLK))
rotationdirection= digitalRead(PinDT);
else
rotationdirection= !digitalRead(PinDT);
TurnDetected = true;
delay(75);
}

void setup() 
{
  display.setRotation(2);
  Serial.begin(9600);
  stepper1.setMaxSpeed(1000);
  stepper1.setSpeed(200);
  stepper2.setMaxSpeed(1000);
  stepper2.setSpeed(200);
  
  pinMode(limitSwitch, INPUT_PULLUP);
  pinMode(PinSW,INPUT_PULLUP); 
  pinMode(PinCLK,INPUT_PULLUP); 
  pinMode(PinDT,INPUT_PULLUP);  
  
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C); 
  display.clearDisplay();
  
  // Create instances for MultiStepper - Adding the 2 steppers to the StepperControl instance for multi control
  StepperControl.addStepper(stepper1);
  StepperControl.addStepper(stepper2);

  attachInterrupt (digitalPinToInterrupt(2),Switch,RISING); // SW connected to D2
  attachInterrupt (digitalPinToInterrupt(3),Rotary,RISING); // CLK Connected to D3
   
  // display Boot logo
  display.drawBitmap(0, 0, CamSlider, 128, 64, 1);
  display.display();
  delay(2000);
  display.clearDisplay(); 
  
  Home(); // Move the slider to the initial position - homing
  
}

void Home()
{
  stepper1.setMaxSpeed(1000);
  stepper1.setSpeed(100);
  stepper2.setMaxSpeed(1000);
  stepper2.setSpeed(100);
  if(digitalRead(limitSwitch)==0)
  {
    display.drawBitmap(0, 0, Homing, 128, 64, 1);
    display.display(); 
  }
  
  while (digitalRead(limitSwitch)== 0) 
  {
    stepper1.setSpeed(-1000);
    stepper1.runSpeed();
    
 }
  delay(100);
  stepper1.setCurrentPosition(0);
    stepper1.moveTo(200);
    while(stepper1.distanceToGo() != 0)
    {
      stepper1.setSpeed(3000);
      stepper1.runSpeed();
    }
    stepper1.setCurrentPosition(0);
    display.clearDisplay();
}

void SetSpeed()
{
  display.clearDisplay();
  while( flag==6)
  {
  if (TurnDetected)  
  {
        TurnDetected = false;  // do NOT repeat IF loop until new rotation detected
        if (rotationdirection) 
        { 
         setspeed = setspeed + 30;
        }
        if (!rotationdirection) 
        { 
         setspeed = setspeed - 30;        
         if (setspeed < 0) 
        { 
         setspeed = 0;
        }
        }

          display.clearDisplay();
          display.setTextSize(2);
          display.setTextColor(WHITE);
          display.setCursor(30,0);
          display.print("Speed");
          motorspeed=setspeed/80;
          display.setCursor(5,16);
          display.print(motorspeed);
          display.print(" mm/s");   
          totaldistance=XOutPoint-XInPoint;
          if(totaldistance<0)
            {
              totaldistance=totaldistance*(-1);
            }
            else
            {
              
            }
          timeinsec=(totaldistance/setspeed);
          timeinmins=timeinsec/60;
          display.setCursor(35,32);
          display.print("Time");
          display.setCursor(8,48);
          if(timeinmins>1)
          {
          display.print(timeinmins);
          display.print(" min");
          }
          else
          { 
          display.print(timeinsec);
          display.print(" sec");
          }
          display.display();
  }
          display.clearDisplay();
          display.setTextSize(2);
          display.setTextColor(WHITE);
          display.setCursor(30,0);
          display.print("Speed");
          motorspeed=setspeed/80;
          display.setCursor(5,16);
          display.print(motorspeed);
          display.print(" mm/s");   
          totaldistance=XOutPoint-XInPoint;
           if(totaldistance<0)
            {
              totaldistance=totaldistance*(-1);
            }
            else
            {
              
            }
          timeinsec=(totaldistance/setspeed);
          timeinmins=timeinsec/60;
          display.setCursor(35,32);
          display.print("Time");
          display.setCursor(8,48);
          if(timeinmins>1)
          {
          display.print(timeinmins);
          display.print(" min");
          }
          else
          { 
          display.print(timeinsec);
          display.print(" sec");
          }
          display.display();
  }
 
}

void stepperposition(int n)
{
  stepper1.setMaxSpeed(1000);
  stepper1.setSpeed(200);
  stepper2.setMaxSpeed(1000);
  stepper2.setSpeed(200);
  if (TurnDetected)  
  {
        TurnDetected = false;  // do NOT repeat IF loop until new rotation detected
     if(n==1)
     {
        if (!rotationdirection) 
        { 
          if( stepper1.currentPosition()-500>0 )
          {
          stepper1.move(-500);
          while(stepper1.distanceToGo() != 0)
            {
              stepper1.setSpeed(-3000);
              stepper1.runSpeed();
            }
          }
            else
            {
                while (stepper1.currentPosition()!=0) 
                 {
                      stepper1.setSpeed(-3000);
                      stepper1.runSpeed();
                 }
            }
        }

        if (rotationdirection) 
        { 
          if( stepper1.currentPosition()+500<61000 )
          {
          stepper1.move(500);
          while(stepper1.distanceToGo() != 0)
            {
              stepper1.setSpeed(3000);
              stepper1.runSpeed();
            }
          }
          else
          {
            while (stepper1.currentPosition()!= 61000) 
             {
                  stepper1.setSpeed(3000);
                  stepper1.runSpeed();
    
             } 
          }
        }
     }
     if(n==2)
     {
       if (rotationdirection) 
       { 
         stepper2.move(-100);
         while(stepper2.distanceToGo() != 0)
           {
             stepper2.setSpeed(-3000);
             stepper2.runSpeed();
           }      
       }
        if (!rotationdirection) 
       { 
         stepper2.move(100);
         while(stepper2.distanceToGo() != 0)
           {
             stepper2.setSpeed(3000);
             stepper2.runSpeed();
           } 
       }
     }
     }
} 


void loop()
{ 
  //Begin Setup
  if(flag==0)
  {
    display.clearDisplay();
    display.drawBitmap(0, 0, BeginSetup, 128, 64, 1);
    display.display(); 
    setspeed=200;
  }
  
  //SetXin
  if(flag==1)
  {
    display.clearDisplay();
    display.setTextSize(2);
    display.setTextColor(WHITE);
    display.setCursor(10,28);
    display.println("Set X In");
    display.display(); 
    while(flag==1)
    {
    stepperposition(1); 
    }
    XInPoint=stepper1.currentPosition();
  }
  //SetYin
  if(flag==2)
  {
    display.clearDisplay();
    display.setTextSize(2);
    display.setTextColor(WHITE);
    display.setCursor(10,28);
    display.println("Set Y In");
    display.display(); 
    while(flag==2)
    {
    stepperposition(2);
    }
    stepper2.setCurrentPosition(0);
    YInPoint=stepper2.currentPosition();
  }
  //SetXout
  if(flag==3)
  {
    display.clearDisplay();
    display.setTextSize(2);
    display.setTextColor(WHITE);
    display.setCursor(10,28);
    display.println("Set X Out");
    display.display(); 
    while(flag==3)
    {
    stepperposition(1);
    Serial.println(stepper1.currentPosition());
    }
    XOutPoint=stepper1.currentPosition();
    
  }
  //SetYout
  if(flag==4)
  {
    display.clearDisplay();
    display.setTextSize(2);
    display.setTextColor(WHITE);
    display.setCursor(10,28);
    display.println("Set Y Out");
    display.display();
    while(flag==4)
    {
    stepperposition(2);
    }
    YOutPoint=stepper2.currentPosition();
    display.clearDisplay();
        
    // Go to IN position
    gotoposition[0]=XInPoint;
    gotoposition[1]=YInPoint;    
    display.clearDisplay();
    display.setCursor(8,28);
    display.println(" Preview  ");
    display.display(); 
    stepper1.setMaxSpeed(1000);
    StepperControl.moveTo(gotoposition);
    StepperControl.runSpeedToPosition();
  }

  //Display Set Speed
  if(flag==5)
  {
    display.clearDisplay();
    display.setCursor(8,28);
    display.println("Set Speed");
    display.display();  
  }
  //Change Speed
  if(flag==6)
  {
    display.clearDisplay();
    SetSpeed();
  }
  //DisplayStart
  if(flag==7)
  {
    display.clearDisplay();
    display.setCursor(30,27);
    display.println("Start");
    display.display();
  }
  //Start
  if(flag==8)
  { 
    display.clearDisplay();
    display.setCursor(20,27);
    display.println("Running");
    display.display();
    Serial.println(XInPoint);
    Serial.println(XOutPoint);
    Serial.println(YInPoint);
    Serial.println(YOutPoint);

    gotoposition[0]=XOutPoint;
    gotoposition[1]=YOutPoint;
    stepper1.setMaxSpeed(setspeed);
    StepperControl.moveTo(gotoposition);
    StepperControl.runSpeedToPosition();
    flag=flag+1;
  }
  //Slide Finish
   if(flag==9)
   {
    display.clearDisplay();
    display.setCursor(24,26);
    display.println("Finish");
    display.display();
   }
  //Return to start
   if(flag==10)
   {
    display.clearDisplay();
    Home();
    flag=0;
   }  
}

```

![MVI_0003_1](https://user-images.githubusercontent.com/19898602/125611317-679003d9-5309-4915-94cb-19981a4d3d8b.gif)

