# Arduino操控舵机

## 原理：舵机的转动的角度是通过调节==PWM==（脉冲宽度调制）信号的占空比来实现的，标准PWM（脉冲宽度调制）信号的周期固定为==20ms==（50Hz），理论上脉宽分布应在1ms到2ms 之间，但是，事实上脉宽可由0.5ms 到2.5ms 之间，脉宽和舵机的转角0°～180°相对应。

## 操控：1.通过Arduino 的普通数字传感器接口产生占空比不同的方波，模拟产生PWM 信号进行舵机定位 

## 2.直接利用Arduino 自带的==Servo==函数进行舵机的控制，这种控制方法的优点在于程序编写，缺点是只能控制2 路舵机，因为Arduino 自带函数只能利用数字9、10 接口。Arduino 的驱动能力有限，所以当需要控制1 个以上的舵机时需要外接电源。

```int myangle;//定义角度变量0-180
int servopin=11;//定义数字接口9 连接伺服舵机信号线
int myangle;//定义角度变量0-180
int pulsewidth;//定义脉宽变量
int val; //0-9

void servopulse(int servopin,int myangle)//定义一个脉冲函数
{
pulsewidth=(myangle*11)+500;//将角度转化为500-2480 的脉宽值

digitalWrite(servopin,HIGH);//将舵机接口电平至高

delayMicroseconds(pulsewidth);//延时脉宽值的微秒数weimiao

digitalWrite(servopin,LOW);//将舵机接口电平至低

delay(20-pulsewidth/1000);
}

void setup()
{
pinMode(servopin,OUTPUT);//设定舵机接口为输出接口

Serial.begin(9600);//连接到串行端口，波特率为9600

Serial.println("servo=o_seral_simple ready" ) ;
}

void loop()//将0 到9 的数转化为0 到180 角度，并让LED 闪烁相应数的次数
{
val=Serial.read();//读取串行端口的值

if(val>'0'&&val<='9')
{
val=val-'0';//将特征量转化为数值变量
val=val*(180/9);//将数字转化为角度
Serial.print("moving servo to ");
//DEC:十进制形式输出 b 的 ASCII 编码值，并同时跟随一个回车和换行符
Serial.print(val,DEC);
Serial.println();
for(int i=0;i<=50;i++) //给予舵机足够的时间让它转到指定角度
{
servopulse(servopin,val);//引用脉冲函数
}
}
}
————————————————

                          
                        

```

![image-20241028221820756](C:\Users\a1551\AppData\Roaming\Typora\typora-user-images\image-20241028221820756.png)