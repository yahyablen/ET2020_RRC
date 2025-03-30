# ET2020_RRC
#include <IRremote.h>


// Khai báo các chân kết nối
#define CLOCK_PIN 12
#define DATA_PIN 11
#define LATCH_PIN 8
#define IR_PIN 2


// Mã của phím
#define KEY_1 0xFFA25D
#define KEY_2 0xFF629D
#define KEY_3 0xFFE21D
#define KEY_4 0xFF22DD
#define KEY_5 0xFF02FD
#define KEY_6 0xFFC23D
#define KEY_0 0xFF9867
#define KEY_OK 0xFF38C7


IRrecv irrecv(IR_PIN);
void turnRedAllLEDs();
void turnGreenAllLEDs();
void turnBlueAllLEDs();
void blinkRedAllLEDs();
void blinkGreenAllLEDs();
void blinkBlueAllLEDs();
void turnOffAllLEDs();
char blink_red;
char blink_green;
char blink_blue;
decode_results results;


void setup() {
  // Cấu hình các chân xuất tín hiệu
  pinMode(CLOCK_PIN, OUTPUT);
  pinMode(DATA_PIN, OUTPUT);
  pinMode(LATCH_PIN, OUTPUT);
  // Bắt đầu nhận tín hiệu IR
  irrecv.enableIRIn();
}


void translateIR(){
  switch(results.value){
    case KEY_1:
     turnRedAllLEDs(); // Gọi hàm bật đỏ tất cả LED
     blink_red=0;
     blink_green=0;
     blink_blue=0;
     break;
    case KEY_2:
     turnGreenAllLEDs(); // Gọi hàm bật xanh lục tất cả LED
     blink_red=0;
     blink_green=0;
     blink_blue=0;
     break;
    case KEY_3:
     turnBlueAllLEDs();// Gọi hàm bật xanh lam tất cả LED
     blink_red=0;
     blink_green=0;
     blink_blue=0;
     break;
    case KEY_4:
     blink_red=1;
     blink_green=0;
     blink_blue=0;
     break;
    case KEY_5:
     blink_red=0;
     blink_green=1;
     blink_blue=0;
     break;
    case KEY_6:
     blink_red=0;
     blink_green=0;
     blink_blue=1;
     break;
    case KEY_0:
     turnWhiteAllLEDs(); // Gọi hàm bật trắng tất cả LED
     blink_red=0;
     blink_green=0;
     blink_blue=0;
     break;
    case KEY_OK:
     turnOffAllLEDs(); // Gọi hàm tắt tất cả LED
     blink_red=0;
     blink_green=0;
     blink_blue=0;
     break;
  }
}
 
void loop() {
  if (irrecv.decode(&results)) {
    translateIR();
    irrecv.resume(); // Chuẩn bị nhận tín hiệu tiếp theo
  }
 if(blink_red){
   blinkRedAllLEDs(); // Gọi hàm nháy đỏ tất cả LED
 }
 if(blink_green){
   blinkGreenAllLEDs(); // Gọi hàm nháy xanh lục tất cả LED
 }
 if(blink_blue){
   blinkBlueAllLEDs(); // Gọi hàm nháy xanh lam tất cả LED
 }
}


// Hàm điều khiển tất cả LED
void turnRedAllLEDs() {
  digitalWrite(LATCH_PIN, LOW);
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b01001000); //bit 8-15
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b10010010); //bit 0-7
  digitalWrite(LATCH_PIN, HIGH);
}


void turnGreenAllLEDs() {
  digitalWrite(LATCH_PIN, LOW);
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b00100100); //bit 8-15
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b01001001); //bit 0-7
  digitalWrite(LATCH_PIN, HIGH);
}
void turnBlueAllLEDs() {
  digitalWrite(LATCH_PIN, LOW);
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b10010010); //bit 8-15
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b00100100); //bit 0-7
  digitalWrite(LATCH_PIN, HIGH);
}


void blinkRedAllLEDs() {
  digitalWrite(LATCH_PIN, LOW);
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b01001000); //bit 8-15
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b10010010); //bit 0-7
  digitalWrite(LATCH_PIN, HIGH);
  delay(500);
  digitalWrite(LATCH_PIN, LOW);
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0x00); //bit 8-15
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0x00); //bit 0-7
  digitalWrite(LATCH_PIN, HIGH);
  delay(500);
}


void blinkGreenAllLEDs() {
  digitalWrite(LATCH_PIN, LOW);
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b00100100); //bit 8-15
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b01001001); //bit 0-7
  digitalWrite(LATCH_PIN, HIGH);
  delay(500);
  digitalWrite(LATCH_PIN, LOW);
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0x00); //bit 8-15
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0x00); //bit 0-7
  digitalWrite(LATCH_PIN, HIGH);
  delay(500);
}


void blinkBlueAllLEDs() {
  digitalWrite(LATCH_PIN, LOW);
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b10010010); //bit 8-15
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b00100100); //bit 0-7
  digitalWrite(LATCH_PIN, HIGH);
  delay(500);
  digitalWrite(LATCH_PIN, LOW);
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0x00); //bit 8-15
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0x00); //bit 0-7
  digitalWrite(LATCH_PIN, HIGH);
  delay(500);
}


void turnWhiteAllLEDs() {
  digitalWrite(LATCH_PIN, LOW);
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b11111110); //bit 8-15
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0b11111111); //bit 0-7
  digitalWrite(LATCH_PIN, HIGH);
}


void turnOffAllLEDs() {
  digitalWrite(LATCH_PIN, LOW);
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0x00); //bit 8-15
   shiftOut(DATA_PIN, CLOCK_PIN, LSBFIRST, 0x00); //bit 0-7
  digitalWrite(LATCH_PIN, HIGH);
}
