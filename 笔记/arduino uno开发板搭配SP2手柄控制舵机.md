# arduino uno开发板搭配SP2手柄控制舵机

![image-20241028222129624](C:\Users\a1551\AppData\Roaming\Typora\typora-user-images\image-20241028222129624.png)引脚说明

## 1. DI/DAT：信号流向，从手柄到主机，此信号是一个 8bit 的串行数据，同 步传送于时钟的下降沿。信号的读取在时钟由高到低的变化过程中完成。 2. DO/CMD：信号流向，从主机到手柄，此信号和 DI 相对，信号是一个 8bit 的串行数据，同步传送于时钟的下降沿。 3.NC：空端口； 4. GND：电源地； 5.VDD：接收器工作电源，电源范围 3~5V； 6.CS/SEL：用于提供手柄触发信号。在通讯期间，处于低电平； 第 4 页 共 7 页 7.CLK：时钟信号，由主机发出，用于保持数据同步； 8.NC：空端口； 9.ACK：从手柄到主机的应答信号。此信号在每个 8bits 数据发送的最后一 个周期变低并且 CS 一直保持低电平，如果 CS 信号不变低，约 60 微秒 PS 主 机会试另一个外设

```#include <Servo.h>
 
//导入库文件 
#include "PS2X_lib.h"
 
//定义ps2手柄
PS2X ps2x; 
int error;
int pose=0;
Servo myservo;
 
void setup() {
  myservo.attach(9);
  myservo.write(0);
  do { 
    error = ps2x.config_gamepad(13, 11, 10, 12, true, true); 
    if (error == 0) { break; } 
    else { delay(100); } 
  } while (1); 
}
 
void loop(){
  ps2x.read_gamepad(false, 0);//读入手柄状态信息 
 
//   //键位码请看上面对应图
//   if (ps2x.Button(PSB_PAD_UP)) {
//     pose+=5;
   
//     if(pose>=180)
//     {
//       pose=180;
       
//     }
//     myservo.write(pose);
//   }//如果“上”按钮按下 
//  delay(100);
//  if(ps2x.Button(PSB_PAD_DOWN))
//  {
//   pose-=5;
//   if(pose<=0)
//   {
//     pose=0;
//   }
//   myservo.write(pose);
//  } //下按钮
  if (ps2x.Analog(PSS_LY) < 10) {
    pose+=5;
   
    if(pose>=180)
     {
       pose=180;
       
     }
     myservo.write(pose);
  }//如果摇杆LY返回值小于10
  if (ps2x.Analog(PSS_LY)>240){
    pose-=5;
   
    if(pose<=0)
     {
       pose=0;
       
     }
     myservo.write(pose);
  }//如果摇杆LY返回值大于240
  delay(10);//延迟10ms 
}
————————————————

                            
