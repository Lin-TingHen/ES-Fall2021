# Lab 6-1 用16X2 LCD 顯示器來顯示4X4鍵盤輸入的數字 (0, 1, 2, .., 9), 若輸入的字數≥16則換到下一列, 若兩皆滿, 則清除劃面重新由Row=0, Col=0開始

## ![image](https://user-images.githubusercontent.com/89329219/139567392-80426f8b-a8b9-475b-ac3a-ed171792aaa2.png)



![image](https://user-images.githubusercontent.com/89329219/139566815-68b3b256-155b-437f-b086-08ae972d7cda.png)

````C
// For Embedded System course, VNU, Fall 2021 

#include <Keypad.h>
#include <LiquidCrystal.h>
// 5:RS, 4:E, 3:DB4, 2:DB5, A4:DB6, A5: DB7
// LCD若只顯示文字，只須使用4-bit模式即可 (LCD腳位DB0, DB1, DB2, DB3就不用接了。)
LiquidCrystal lcd(5, 4, 3, 2, A4, A5);

const byte ROWS = 4; // 4列數(橫的)
const byte COLS = 4; // 4行數(直的)
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};

byte rowPins[ROWS] = {A0, A1, 11, 10}; //定義列的腳位
byte colPins[COLS] = {9, 8, 7, 6}; //定義行的腳位

int LCDCol = 0;
int LCDRow = 0;

Keypad keypad = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS );

void setup(){
   Serial.begin(9600);
   lcd.begin(16, 2);
   lcd.setCursor(LCDCol, LCDRow);
   }
  
void loop(){
  char key = keypad.getKey(); //讀取 Keypad 的輸入

if (key){   
    Serial.println(key);
 if ( LCDCol > 15  )
    {   
     ++LCDRow; // Move to the 2nd row
      
      if (LCDRow>1)
      { LCDRow=0; LCDCol = 0 ;  lcd.clear(); } // clear and move back the 1st row
   
    LCDCol = 0 ;
    }
         
    lcd.setCursor (LCDCol, LCDRow); 
    
       lcd.print(key);
    
    ++LCDCol;
    
  }
}
````
## Datasheet: 4X4 Matrix Membrane Keypad 
https://cdn.sparkfun.com/assets/f/f/a/5/0/DS-16038.pdf
