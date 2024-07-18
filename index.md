# Motion and Hand Gesture Controlled Robotic Car

In this project, I assembled a robotic car that can be controlled by hand motions and signs. This is possible because of an accelerometer that is attached to the back of a user's hand that allows the movements of the user's hand to be tracked and flex sensors on each of the user's four main fingers to detect flexing of the fingers. With these detection systems, the user can control the car by tilting their hand and flexing their fingers into certain signs to change the car's movement and speed.
The biggest challenges I encountered were the complex circuitry and connections between the boards and parts and coding with the Arduino. I had no experience with either of these things, but I am proud to say that after completing this project, I now know quite a bit about these two concepts.
If I had more time to work on this project, I would try to make the movements a bit cleaner and adjust some of the code to make the glove's controlling system easier to use and more accurate. 

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Chloe G. | Carondelet High School | Engineering | Incoming Junior

<!---**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)--->


# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/COc-jXujoRc?si=yAsBBilBZ7alfK2S" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

- Wired and coded the bluetooth modules that allow the glove and car to communicate
- Encountered an issue with wiring (wires got tangled because I got confused by the schematic)
- Began wiring and testing motors
- Encountered a second issue with a lack of voltage in the motors (motors would only turn when device was wired to my computer; battery given to us lacked sufficient voltage to get them to spin)
- Tasks before next milestone:
- Receive a new battery to replace the defective one in hopes that this will fix the motor problem
- Assemble the car chassis
- Write the code for the car and motion control sensor


# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/VfdK9m_uzHs?si=8wXze18zXyGmTyZD" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

- Assembled the car chassis by attaching the motors to fasteners with screws and attaching the top and bottom parts of the car together
- Accidentally broke the USB port on my Arduino Micro (had to get a new one shipped)
- Tested the new battery and thankfully, the motors worked
- Adjusted my car code so the car would turn left and right instead of just spinning randomly
- Adjusted threshholds for the accelerometer to make it easier to move the car
- Tasks before next milestone:
- Wire up flex sensors to the motion control glove
- Alter code to accommodate new flex input


# Final Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- Changed original code to take in input from the flex sensors and send that info to the car
- Added new commands to change the car's speed when flex motion detected
- Added a new "dance" command that would allow the car to do a little dance when a certain symbol was made with my hand (detected with the flex sensors)
- Completed wiring of flex sensors to glove
- Changed the motor pin wiring to allow their movement speed to be altered
- Finally, my project was finished!

This project was very fun to complete, but also one of the most challenging things I have ever taken on. Throughout the course of this project, I had to learn how to code with Arduino and how to create circuits, two things I had zero experience with before. It was also very difficult to figure out how to follow the schematics and instructions given to us, as I didn't know how to read circuit diagrams or how to use any of the parts sent to me. This was the biggest challenge I encountered, because the circuitry was very complicated to me and had to be quite precise for the project to work. Also, it was difficult for me to figure out what was wrong with my circuits whenever my project wouldn't work, because the wires were quite hard to follow and the diagrams were often unclear at best. However, with the help of my instructors and through perseverance, I managed to grasp these new concepts and complete my project. By the end of this program, I now know how to code with Arduino, how to create circuits with Arduino boards, how to use breadboards, and how to create circuits with motors. I am very proud of the growth I have experienced throughout this program, from not even knowing how a breadboard works to being able to wire up circuits by myself. I honestly didn't think I would finish this project when I originally saw how complicated it was, but I am very proud that I did. Going forward, I hope to use these newfound skills to learn more about robotics and coding. This program provided me with great exposure to engineering and the engineering mindset, and I hope to pursue and explore this field further going forward. Hopefully, the skills I have acquired here at Bluestamp will allow me to make my own technological advancements as a future engineer. 

<!--For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE
-->


# Schematics 
<!--Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. -->
![Car Schematic](/ChloeG_BlueStampPortfolio/assets/schematics(1).jpeg)
![Glove Schematic](/ChloeG_BlueStampPortfolio/assets/schematics.jpeg)


# Code
<!--Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. --->

Code for initial bluetooth pairing and testing:
```c++
//Code for Arduino Micro

void setup()
{
  Serial.begin(38400);
  Serial1.begin(38400);
}

void loop()
{
  if (Serial1.available())
  {
    Serial.print((char)Serial1.read());  
  }
  if (Serial.available())
  {
    Serial1.write(Serial.read());
  }
}

//Code for Arduino Uno

#include <SoftwareSerial.h>

#define tx 2
#define rx 3

SoftwareSerial configBt(rx, tx);
long tm, t, d;

void setup()
{
  Serial.begin(38400);
  configBt.begin(38400);
  pinMode(tx, OUTPUT);
  pinMode(rx, INPUT);
}

void loop()
{
  if (configBt.available())
  {
    Serial.print((char)configBt.read());
  }
  if (Serial.available())
  {
    configBt.write(Serial.read());
  }
}
```

Code for the glove/controller:
```c++
#include <Wire.h>

#define MPU6050_ADDRESS 0x68

int16_t accelX, accelY, accelZ;

//analog pins that flex sensors are in
int f1 = 4;
int f2 = 5;
int f3 = 0; 
int f4 = 1;

//variables to store flex reading
int r1;
int r2;
int r3;
int r4;


void setup() 
{
  Wire.begin();
  Serial1.begin(38400);

//initializes accelerometer
  Wire.beginTransmission(MPU6050_ADDRESS);
  Wire.write(0x6B);
  Wire.write(0);
  Wire.endTransmission(true);

//allows accelerometer to stabilize
  delay(100);
}

void loop() {
  //main code
  readAccelerometerData();
  readFsr();
  determineGesture();
  delay(500);
}

void readAccelerometerData()
{
  Wire.beginTransmission(MPU6050_ADDRESS);
  Wire.write(0x3B);
  Wire.endTransmission(false);
  Wire.requestFrom(MPU6050_ADDRESS, 6, true);

  //reads accel data and stores in variables
  accelX = Wire.read() << 8 | Wire.read();
  accelY = Wire.read() << 8 | Wire.read();
  accelZ = Wire.read() << 8 | Wire.read();
}

void determineGesture()
{
  //determines command based on accel or flex sensor data
  if (r1 < 250 && r2 > 200 && r3 > 270 && r4 > 220)
  {
    Serial1.write('1');
  }
  else if (r1 < 250 && r2 < 210 && r3 > 270 && r4 > 230)
  {
    Serial1.write('2');
  }
  else if (r1 < 250 && r2 < 210 && r3 < 280 && r4 > 230)
  {
    Serial1.write('3');
  }
  else if (r1 < 230 && r2 > 210 && r3 > 270 && r4 < 215)
  {
    Serial1.write('d');
  }
  else if (accelX >= 8000)
  {
    Serial1.write('b');
  }
  else if (accelX <= -8000)
  {
    Serial1.write('f');
  }
  else if (accelY <= -4000)
  {
    Serial1.write('l');
  }
  else if (accelY >= 5000)
  {
    Serial1.write('r');
  }
  else
  {
    Serial1.write('s');
  }
}

void readFsr()
{
  //reads flex sensor input
  r1 = analogRead(f1);
  r2 = analogRead(f2);
  r3 = analogRead(f3);
  r4 = analogRead(f4);
}
```

Code for the car:
```c++
#include <SoftwareSerial.h>
#define tx 2
#define rx 3

SoftwareSerial BT_Serial(rx, tx); // RX pin, TX pin

char receive = ""; //variable that command is stored in

int AIA = 4; //front left speed
int AIB = 9; //front left directional
int BIA = 12; //back left speed
int BIB = 11; //back left directional
int aia = 10; //back right speed
int aib = 8; //back right directional
int bia = 6; //front right speed
int bib = 7; //front right directional

int sp = 255; //speed begins at max but can be changed

void setup() 
{
  //begins communication between bluetooth modules
  Serial.begin(38400);
  BT_Serial.begin(38400);
  pinMode(tx, OUTPUT);
  pinMode(rx, INPUT);

  //sets all motor pins to output
  pinMode(AIA, OUTPUT);
  pinMode(AIB, OUTPUT);
  pinMode(BIA, OUTPUT);
  pinMode(BIB, OUTPUT);
  pinMode(aia, OUTPUT);
  pinMode(aib, OUTPUT);
  pinMode(bia, OUTPUT);
  pinMode(bib, OUTPUT);
}

void loop() {
  //reads accel sent data
  if(BT_Serial.available())
  {
    receive = (char) BT_Serial.read();
    Serial.println(receive);
  }

  //switches receive variable to whatever case is determined
  switch(receive)
  {
    case '1':
      accel(150);
      break;

    case '2':
      accel(200);
      break;
    
    case '3':
      accel(255);
      break;
    
    case 'd':
      dance();
      break;
    
    case 'f':
      forward();
      break;

    case 'b':
      backward();
      break;
    
    case 'r':
      turnRight();
      break;
    
    case 'l':
      turnLeft();
      break;
    
    case 's':
      stop();
  }

}


//turns motors off and on accordingly per movement
void forward()
{
  analogWrite(AIB, sp);
  digitalWrite(AIA, LOW);
  analogWrite(BIB, sp);
  digitalWrite(BIA, LOW);
  analogWrite(aia, sp);
  digitalWrite(aib, LOW);
  analogWrite(bia, sp);
  digitalWrite(bib, LOW);
}

void backward()
{
  digitalWrite(AIB, LOW);
  digitalWrite(AIA, HIGH);
  digitalWrite(BIB, LOW);
  digitalWrite(BIA, HIGH);
  digitalWrite(aia, LOW);
  digitalWrite(aib, HIGH);
  digitalWrite(bia, LOW);
  digitalWrite(bib, HIGH);
}

void turnLeft()
{
  digitalWrite(AIA, HIGH);
  digitalWrite(AIB, LOW);
  digitalWrite(BIA, HIGH);
  digitalWrite(BIB, LOW);
  digitalWrite(aia, HIGH);
  digitalWrite(aib, LOW);
  digitalWrite(bia, HIGH);
  digitalWrite(bib, LOW);
}

void turnRight()
{
  digitalWrite(AIA, LOW);
  digitalWrite(AIB, HIGH);
  digitalWrite(BIA, LOW);
  digitalWrite(BIB, HIGH);
  digitalWrite(aia, LOW);
  digitalWrite(aib, HIGH);
  digitalWrite(bia, LOW);
  digitalWrite(bib, HIGH);
}

void stop()
{
  digitalWrite(AIA, LOW);
  digitalWrite(AIB, LOW);
  digitalWrite(BIA, LOW);
  digitalWrite(BIB, LOW);
  digitalWrite(aia, LOW);
  digitalWrite(aib, LOW);
  digitalWrite(bia, LOW);
  digitalWrite(bib, LOW);
}

//extra dance command
void dance()
{
  for(int i = 0; i < 2; i++)
  {
  turnLeft();
  delay(1000);
  stop();
  delay(500);
  turnRight();
  delay(1250);
  }
}

//changes speed of car based on parameter (s is in bytes; 0 - 255)
void accel(int s)
{
  sp = s;
}
```


# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Perseids Smart Car 4 Wheel Chassis Kit | Kit is used for the car chassis and its wheels | $21.29 | <a href="https://www.amazon.com/perseids-Chassis-Encoder-Wheels-Battery/dp/B07DNXBFQN/ref=sr_1_1?crid=3MBV9FBZKY5FH&dib=eyJ2IjoiMSJ9.f7mtFhVYnUqArHQfkJlxe5f4RIF0mYndSG-eL3X8mmauOP3AQ0l9h9SkBDo-SwBkXl4xSj4Bq9-Cj-yLzg8lDoevhOsweqarVwLF3L76g4t2Dw8ZacN5NWeW1AbwmyTBSLNWHU2Pq7nnf5xEoZV9WlRWI08vXsn958GtutQgr2iz-1_JKo_sjOo5fCPJGvRqIudagoVqOguKUq5EmnkT-3aEDzgY6D8WkVqbQRKJ1jzHXtiLowRGex1qLs6srGtZVr1VdUH1m4pNVUDc6XD0wqDTiAsgWOnVkM6Q8oUFh34.MkUiaMN2tOm7wh8CBeRvPVkhCS4WSiMCNu27y-GYydo&dib_tag=se&keywords=The%2Bperseids%2BDIY%2BRobot%2BSmart%2BCar%2BChassis%2BKit%2BEducational%2BToy%2Bwith%2BSpeed%2BEncoder%2C&qid=1719947204&sprefix=%2Caps%2C109&sr=8-1&th=1"> Link </a> |
| Screwdriver Set | Used to assemble car chassis | $6.99 | <a href="https://www.amazon.com/Small-Screwdriver-Set-Mini-Magnetic/dp/B08RYXKJW9/"> Link </a> |
| Motor Drivers | Controls motors | $7.79 | <a href="https://www.amazon.com/HiLetgo-H-bridge-Stepper-Controller-Arduino/dp/B00M0F243E/ref=pd_lpo_sccl_1/142-4739935-9789822?pd_rd_w=5sLcA&content-id=amzn1.sym.4c8c52db-06f8-4e42-8e56-912796f2ea6c&pf_rd_p=4c8c52db-06f8-4e42-8e56-912796f2ea6c&pf_rd_r=KR0X36CCWH30E7T108Y9&pd_rd_wg=1cZ0l&pd_rd_r=420e26d6-ea71-4fc7-adcf-6cf8ffb38079&pd_rd_i=B00M0F243E&psc=1"> Link </a> |
| Elegoo Arduino Uno Board | Receives input from the motion controller and gives orders to car | $16.99 | <a href="https://www.amazon.com/ELEGOO-Board-ATmega328P-ATMEGA16U2-Compliant/dp/B01EWOE0UU/ref=sr_1_2_sspa?crid=3A6NCD2X9JEMJ&dib=eyJ2IjoiMSJ9.AcWZy-Yg4mDTnhzEHozxzPZdVC5-KUL2tW-OQewDKpBB4brSpD-p4bn74WcXiW3KarYertgpNaLJ0VHKx0qsPqolKAhiz1GRG5BwJQl73cEvrlXIXNmqlpSvU7uu2aRVSwAZi9Gj2AjSPLM3esW1Gzy9xEiQ9oiR5LCNjh4MlYDx5mTm5sI4rsD4CFTipJnF572qXlickl35FRcCj8oMXQotumgqI4yEIq0HobOtIlEnNhtVB51JMBHhqtmmF_PC9WeHJ4ySUVVcv_gq3_VeG1aAEbdm4NXmmT6NOYPw4Qo.1PFdgFT22oqO5Mg6-6j_aUL_EV8tUPuaFrB5N9oaEX0&dib_tag=se&keywords=elegoo+arduino&qid=1716856465&s=electronics&sprefix=elegoo+arduino%2Celectronics%2C99&sr=1-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
| 8 Pcs Gearbox Motors | Motors that control the wheels | $10.99 | <a href="https://www.amazon.com/AEDIKO-Motor-Gearbox-200RPM-Ratio/dp/B09N6NXP4H/ref=sr_1_4?crid=1JP29NIWBLH2M&dib=eyJ2IjoiMSJ9.Wq3jKgOLbqtEP772vMD4pV5f-w3PLBdEpKqguykXOb0JFO14f4Dq0m_VDVUMUFtR8WFINUEticI3GXcoGqwXPqK9yIh04PhCktgccMz9zAUiKXMJPwmOTUp_6av3XuFD0lXo9WngN9iKI6YgZrhEEs9qnqbcB1GnvgntCdKz8Q1dFuNu61NgSE6Z8vBk3FRpaNcr1lCI7FApTiNi0Qce8gbfmMn6oUggZQHpIOKKZ6s.M7WsZ_ZZtm3rm93kKgw0NOxt1McVBYX6m55oGxu1xxI&dib_tag=se&keywords=dc+motor+with+gearbox&qid=1715911706&sprefix=dc+motor+with+gearbox%2Caps%2C126&sr=8-4"> Link </a> |
| Electronics Components Kit | Used for wiring and circuitry | $13.99 | <a href="https://www.amazon.com/Smraza-Electronics-Potentiometer-tie-Points-Breadboard/dp/B0B62RL725/ref=sxts_b2b_sx_reorder_acb_business?content-id=amzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f%3Aamzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f&crid=2IC3T44H3U3WG&cv_ct_cx=breadboard+kit&dib=eyJ2IjoiMSJ9.TUd5tu2T8rmms7ZuJ0UzmbtpLL1zsu93bQM0PzwnP4E.sT0V0vL_QtbYv8ymVTCcRkhFNgBtRvRiT7G4FT1oGTE&dib_tag=se&keywords=breadboard+kit&pd_rd_i=B0B62RL725&pd_rd_r=67e1f4ff-e3b9-44e4-b441-b4ae282f036b&pd_rd_w=UjFaP&pd_rd_wg=0xRoC&pf_rd_p=f63a3b0b-3a29-4a8e-8430-073528fe007f&pf_rd_r=BFGP77H27ZN31W4PZAW6&qid=1715911733&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=breadboard+kit%2Caps%2C109&sr=1-2-9f062ed5-8905-4cb9-ad7c-6ce62808241a"> Link </a> |
| 4 Pcs Breadboards | Used for circuitry | $8.88 | <a href="https://www.amazon.com/Breadboards-Solderless-Breadboard-Distribution-Connecting/dp/B07DL13RZH/ref=sxts_b2b_sx_reorder_acb_business?content-id=amzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f%3Aamzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f&crid=1RAL6PA1TZ81Q&cv_ct_cx=breadboard+kit&dib=eyJ2IjoiMSJ9.TUd5tu2T8rmms7ZuJ0UzmbtpLL1zsu93bQM0PzwnP4E.sT0V0vL_QtbYv8ymVTCcRkhFNgBtRvRiT7G4FT1oGTE&dib_tag=se&keywords=breadboard+kit&pd_rd_i=B07DL13RZH&pd_rd_r=1e3e6f57-5578-4452-b230-90d43c79b5d3&pd_rd_w=rFN6B&pd_rd_wg=3mMuA&pf_rd_p=f63a3b0b-3a29-4a8e-8430-073528fe007f&pf_rd_r=JC9D7T4VYRDQ9HJVY5X8&qid=1715912837&s=electronics&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=breadboard+kit%2Celectronics%2C102&sr=1-1-9f062ed5-8905-4cb9-ad7c-6ce62808241a"> Link </a> |
| Arduino Micro | Used to collect input from motion sensors and sends input to Arduino Uno | $22.48 | <a href="https://www.amazon.com/Arduino-Micro-Headers-A000053-Controller/dp/B00AFY2S56/ref=sxts_b2b_sx_reorder_acb_business?content-id=amzn1.sym.44ecadb3-1930-4ae5-8e7f-c0670e7d86ce%3Aamzn1.sym.44ecadb3-1930-4ae5-8e7f-c0670e7d86ce&cv_ct_cx=arduino%2Bmicro&keywords=arduino%2Bmicro&pd_rd_i=B00AFY2S56&pd_rd_r=3c265d26-c144-45b4-b645-a19f57187069&pd_rd_w=ZWCox&pd_rd_wg=dgTyS&pf_rd_p=44ecadb3-1930-4ae5-8e7f-c0670e7d86ce&pf_rd_r=SRN3W01Y55A8M3VF2PXJ&qid=1686186926&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sr=1-1-62d64017-76a9-4f2a-8002-d7ec97456eea&th=1"> Link </a> |
| Micro USB Cables | Used to connect Arduino Micro to computer to input code | $5.49 | <a href="https://www.amazon.com/Charging-Transfer-Android-Trustable-MYFON/dp/B098DW7485/ref=sr_1_6?crid=3USJU0DMSZB2S&keywords=micro%2Busb&qid=1686187078&s=electronics&sprefix=micro%2Busb%2Celectronics%2C106&sr=1-6&th=1"> Link </a> |
| Pre-Soldered Accelerometer | Collects motion input that is used to control car | $9.90 | <a href="https://www.amazon.com/Pre-Soldered-Accelerometer-Raspberry-Compatible-Arduino/dp/B0BMY15TC4/ref=sr_1_5?crid=8EDYBVQQY7E2&dib=eyJ2IjoiMSJ9.ID40hq0zMYWtG7Um61yZ63xnujgA2opJZN4n7Ear4a7PVz0kChoZQvMielgIQHXUTy4_yuQvwgK7S5aC7H8U6s5ChRMOd0Iba7IZDg_ySpKnO5uemH-09l_GS1vcaiACgMnHA4JltsdzdfsSBwKgUFAhFhLuvIKnY6G3lrVGfynAdqGHpq4kg53C83MmKTRP8583zcZvMNE8N9pGZr9m2_ctic429UEwmpvof0hrhug.bBXCol9-0Y3cd8LQBcW01jRrDORIYOXF6HAJOn6LUjY&dib_tag=se&keywords=accelerometer+arduino&qid=1715912788&sprefix=accelerometer+arduino%2Caps%2C110&sr=8-5"> Link </a> |
| Bluetooth Module | Attached to both the Micro and UNO to allow the two to communicate | $9.99 | <a href="https://www.amazon.com/DSD-TECH-HC-05-Pass-through-Communication/dp/B01G9KSAF6/ref=sr_1_3?crid=2J833J7AYQJA&keywords=hc05&qid=1686187263&sprefix=hc0%2Caps%2C112&sr=8-3"> Link </a> |
| 3 Pc Power Supply Modules | Used to connect battery input to circuits | $7.99 | <a href="https://www.amazon.com/ALAMSCN-Solderless-Breadboard-Battery-Arduino/dp/B08JYPMCZY/ref=sxts_b2b_sx_reorder_acb_business?content-id=amzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f%3Aamzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f&crid=Z2S8NZU0KN1S&cv_ct_cx=breadboard+power+supply&dib=eyJ2IjoiMSJ9.nJ_euybTOUu9E6yyDpnEqg.NgztCYPGkG96eXyyFxpvxOVw5ykdTUq6oziUQnvf51E&dib_tag=se&keywords=breadboard+power+supply&pd_rd_i=B08JYPMCZY&pd_rd_r=f2beb6df-6d77-44a3-8b72-83255f19ca20&pd_rd_w=r1wmq&pd_rd_wg=ToFNq&pf_rd_p=f63a3b0b-3a29-4a8e-8430-073528fe007f&pf_rd_r=R5ZMMGW4CXRBP3PWAYMA&qid=1715912515&s=electronics&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=breadboard+power+s%2Celectronics%2C114&sr=1-1-9f062ed5-8905-4cb9-ad7c-6ce62808241a"> Link </a> |
| 10 Pc 9V Batteries | Powers circuits | $8.88 | <a href="https://www.amazon.com/dp/B00ZTS55Y4/ref=sspa_dk_detail_0?psc=1&pd_rd_i=B00ZTS55Y4&pd_rd_w=BBggT&content-id=amzn1.sym.c4606765-78ec-444e-9319-716ceb6c5a61&pf_rd_p=c4606765-78ec-444e-9319-716ceb6c5a61&pf_rd_r=28J17622MJVZBASAP1W4&pd_rd_wg=dnaox&pd_rd_r=484ff4ad-b545-49fc-ae3d-859edc6ba760&s=electronics&sp_csd=d2lkZ2V0TmFtZT1zcF9kZXRhaWxfdGhlbWF0aWM"> Link </a> |
| 16 Pc Velcro Strips | Used to create band to attach accelerometer mechanism to user's hand | $6.99 | <a href="https://www.amazon.com/Art3d-Sticky-Double-Sided-Command-Adhesive/dp/B0B58FGF8H/ref=sr_1_1_sspa?crid=2N0JOMEZLJ2DS&dib=eyJ2IjoiMSJ9.qGUGB_MXfmbL0MW7bqNJbxvZC9pzliDJ9KYyRNNrctnh03kCcUXONRrcPYdGeo7Jwzrm83HyF8Jsb1RkcdlLPAw-8RkxbTCMiW6UI1Fpnjv9GjXUg9VBOLxmLVUbmMp5J7gFXKKLTWQ-w_L4Q9rykEUqKmjv-v6GRykMMZLY2cVt__lLxMIlwr6qBnQLWpHiklifUJwjiURxO--TTt2VReYgmN0z7118ifSucrkvRrg.mwA0L4zMSlJP2RO8IBba7dVqwa1Lkr8KvY1JmeQEfCg&dib_tag=se&keywords=velcro%2Btape%2Bpieces&qid=1716734034&sprefix=velcro%2Btape%2Bpiece%2Caps%2C89&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| Multimeter Voltage Testing | Used for fixing circuits by measuring voltage | $12.99 | <a href="https://www.amazon.com/AstroAI-Digital-Multimeter-Voltage-Tester/dp/B01ISAMUA6/ref=sxin_17_pa_sp_search_thematic_sspa?content-id=amzn1.sym.e8da13fc-7baf-46c3-926a-e7e8f63a520b%3Aamzn1.sym.e8da13fc-7baf-46c3-926a-e7e8f63a520b&cv_ct_cx=digital+multimeter&dib=eyJ2IjoiMSJ9.5LQumrfBR8l0mKnJCJlRg73dxpou0gqYD_ffU3srgs0Utegwth8GcQCSVXVzeZeLSJx5J3itz5TLdmJHsrVITQ.-00jRPoT-bBy26YC4LzQ-S4cYdztgmSMGb83_WEm6HY&dib_tag=se&keywords=digital+multimeter&pd_rd_i=B01ISAMUA6&pd_rd_r=e1ff2570-7e4a-4906-bc55-6f819d48d1bc&pd_rd_w=h7HgL&pd_rd_wg=0ZcFH&pf_rd_p=e8da13fc-7baf-46c3-926a-e7e8f63a520b&pf_rd_r=R6YKX3NXTDQ1PQP4H8RM&qid=1715911879&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sr=1-1-7efdef4d-9875-47e1-927f-8c2c1c47ed49-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&psc=1"> Link </a> |


<!---
# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
--->
