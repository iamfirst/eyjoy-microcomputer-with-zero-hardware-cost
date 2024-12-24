@[TOC](文章目录)

---

# 前言
Proteus是一款功能强大的电子电路设计和仿真软件，可以对单片机进行仿真。本文以单片机Arduino pro mini为例，先在Arduino IED上写好按钮控制流水灯程序，然后导入Proteus进行仿真调试

---
# 一、Arduino pro mini引脚图
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a11253042daf4ada871cd8016729fd67.png#pic_center)

# 二、Arduino IED编写手动控制流水灯程序
代码如下：
```c
#include <MsTimer2.h>

int led1 = 17;//定义LED1引脚为17
int led2 = 16;//定义LED2引脚为16
int led3 = 15;//定义LED3引脚为15
int led4 = 14;//定义LED4引脚为14
int ledOn = 0;
//char *ledName[] = {"led1","led2","led3","led4"};

int button = 18;//定义按钮开关引脚为7
int buttonVal = HIGH;//定义变量,用来存储按钮状态

int buzzer = 13;

int pwm = 6;
int pwmVal = 0;//0~255

void setup() {
  // initialize diital pin LED_BUILTIN as an output.
  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);
  pinMode(led3, OUTPUT);
  pinMode(led4, OUTPUT);
  pinMode(button, INPUT);
  digitalWrite(button, HIGH);
  pinMode(pwm, OUTPUT);
  
  //buzzer
  pinMode(buzzer, OUTPUT);
  MsTimer2::set(2000, timerBuzzer);
  MsTimer2::start();
}

void timerBuzzer(){
  static  boolean buzzerVal = HIGH;
  digitalWrite(buzzer, buzzerVal);
  buzzerVal = !buzzerVal;
}

void allLedOff(){
  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
  analogWrite(pwm, 0);
}

// the loop function runs over and over again forever
void loop() {
    buttonVal = digitalRead(button);
    //如果按钮电平被拉低，就是接地
    if (buttonVal == LOW)
    {
      allLedOff();
      ledOn = ledOn > 4 ? 0 : ledOn + 1;
      //digitalWrite(ledName[ledOn], HIGH);
      switch(ledOn){
        case 1: digitalWrite(led1, HIGH); analogWrite(pwm, 60); break;
        case 2: digitalWrite(led2, HIGH); analogWrite(pwm, 120); break;
        case 3: digitalWrite(led3, HIGH); analogWrite(pwm, 180); break;
        case 4: digitalWrite(led4, HIGH); analogWrite(pwm, 255); break;
        //default: ledOn = 0;
      }


      delay(650);
      digitalWrite(button, HIGH);//按钮恢复高电平
    }
    //else 
    //{
    //  digitalWrite(led, LOW);   // turn the LED off (LOW is the voltage level)
    //}     
    
    //pwm
    //analogWrite(pwm, pwmVal);
}

```
# 三、Arduino IED编译好程序，并导出为hex

# 四、仿真软件中验证程序
Proteus中画好电路，并在单片机中导入前面的hex文件，运行如下所示。当每次按下按钮S1，可见不同的LED灯被点亮，实现了人肉切换的效果。稍微修改一下逻辑也可以达到自动流水灯的效果，这里就不做展开了。

![circuit](https://github.com/iamfirst/eyjoy-microcomputer-with-zero-hardware-cost/blob/main/202412190008.gif)
---
# 总结
文章叙述得比较粗略，其它一些单片机同理，想要验证想法，也可以按这个大概流程在仿真软件中实施验证
