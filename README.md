
volatile unsigned int temp, counter = 10000; //This variable will increase or decrease depending on the rotation of encoder

/*Example sketch to control a stepper motor with A4988 stepper motor driver and Arduino without a library. More info: https://www.makerguides.com */
// Define stepper motor connections and steps per revolution:
#define dirPin 4
#define stepPin 5
#define stepsPerRevolution 200
void setup() {
  // Declare pins as output:
  pinMode(stepPin, OUTPUT);
  pinMode(dirPin, OUTPUT);


  Serial.begin (9600);
  pinMode(2, INPUT_PULLUP); // internal pullup input pin 2 
  pinMode(3, INPUT_PULLUP); // internalเป็น pullup input pin 3
//Setting up interrupt
  //A rising pulse from encodenren activated ai0(). AttachInterrupt 0 is DigitalPin nr 2 on moust Arduino.
  attachInterrupt(0, ai0, RISING);
   
  //B rising pulse from encodenren activated ai1(). AttachInterrupt 1 is DigitalPin nr 3 on moust Arduino.
  attachInterrupt(1, ai1, RISING);

Serial.println ("start");

}
void loop() {
  // Set the spinning direction clockwise:
  digitalWrite(dirPin, HIGH);
  // Spin the stepper motor 1 revolution slowly:
  int thestart=counter;
  int thestop=counter-1200;
  while(counter>thestop) {
    // These four lines result in 1 step:
    digitalWrite(stepPin, HIGH);
    delayMicroseconds(1000);
    digitalWrite(stepPin, LOW);
    delayMicroseconds(1000);
  }
  delay(500);

  digitalWrite(dirPin, LOW);
 thestart=counter;
 thestop=counter+1200;
  while(counter<thestop) {
    // These four lines result in 1 step:
    digitalWrite(stepPin, HIGH);
    delayMicroseconds(1000);
    digitalWrite(stepPin, LOW);
    delayMicroseconds(1000);
  }
  delay(500);


}

  void ai0() {
  // ai0 is activated if DigitalPin nr 2 is going from LOW to HIGH
  // Check pin 3 to determine the direction
  if(digitalRead(3)==LOW) {
  counter++;
  }else{
  counter--;
  }
  }
   
  void ai1() {
  // ai0 is activated if DigitalPin nr 3 is going from LOW to HIGH
  // Check with pin 2 to determine the direction
  if(digitalRead(2)==LOW) {
  counter--;
  }else{
  counter++;
  }
  }
