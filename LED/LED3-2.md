# Lab 3-2: 超音波感測器 + LED (紅色LED:亮<70cm, 緑色LED: 亮>270cm, 藍色LED:亮, 介於70cm ~ 270cm之間) + RS232 Output
![image](https://user-images.githubusercontent.com/89329219/134792608-a4255df5-553c-4049-811b-36d9810a90a7.png)
![image](https://user-images.githubusercontent.com/89329219/134792613-dfd680d8-8d64-4b59-bb9f-61b4dfcfa374.png)
![image](https://user-images.githubusercontent.com/89329219/134792624-668554a1-6beb-4041-a1ee-58c2f432c53a.png)
![image](https://user-images.githubusercontent.com/89329219/135738212-2cbb34e1-4b18-4696-9cb3-6ab072588747.png)

````C
int inches = 0;
int GLED = 8;
int RLED = 12;
int BLED = 10;
int cm = 0;

void LED(int RH, int GH, int BH)
{
  digitalWrite(RLED, RH);  
  digitalWrite(GLED, GH);
  digitalWrite(BLED, BH);   
}

long readUltrasonicDistance(int triggerPin, int echoPin)
{
  pinMode(triggerPin, OUTPUT);  // Clear the trigger
  digitalWrite(triggerPin, LOW);
  delayMicroseconds(2);
  // Sets the trigger pin to HIGH state for 10 microseconds
  digitalWrite(triggerPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(triggerPin, LOW);
  pinMode(echoPin, INPUT);
  // Reads the echo pin, and returns the sound wave travel time in microseconds
  return pulseIn(echoPin, HIGH);
}

void setup()
{
  Serial.begin(9600);
  digitalWrite(GLED, OUTPUT);
  digitalWrite(RLED, OUTPUT);  
  digitalWrite(BLED, OUTPUT);    
  
}

void loop()
{
  // measure the ping time in cm
  cm = 0.01723 * readUltrasonicDistance(7, 7);
  // convert to inches by dividing by 2.54
  inches = (cm / 2.54);
  Serial.print(inches);
  Serial.print("in, ");
  Serial.print(cm);
  
  if(cm<70){
    LED(1, 0, 0); //int RH, int GH, int BH    
  }
  else if(cm>270){
    LED(0, 0, 1); //int RH, int GH, int BH      
  }
  else{
    LED(0, 1, 0); //int RH, int GH, int BH      
  }
    
  Serial.println("cm");
  delay(100); // Wait for 100 millisecond(s)
   
}
````

if else 補充:https://www.lazytomatolab.com/as-04/
