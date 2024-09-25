---
layout:     post
title:      "esp8266可编程手机屏点击器使用说明"
subtitle: ""
date:       2024-09-25 16:54:00
author:     "Mage"
header-img: "img/post-bg-universe.jpg"
tags:
    - Hard
---

## 话不多说，直入主题

## 今天给大家讲解一款多功能点击器
可以用来自动玩游戏（像DNF搬砖、光遇弹琴），工作打卡等一些使用场景（其他使用场景还得靠网友们各显神通一起想想）

以下是该产品的使用方法和具体讲解，文末附代码

本板子支持安卓和苹果手机，几乎支持所有电容触摸类手机屏，因为手机检测频率限制，所以每秒最高点击次数可以达到10次。
![请添加图片描述](https://i-blog.csdnimg.cn/direct/3be302664139453bbbaa0da556f48ba0.png#pic_center)
使用方法：

 1. 用数据线连接电脑和板子，数据线的一头插在**供电口**处
 2. 把点击头连接在接口处，用几个连几个就行
 3. 另一头用纳米胶粘在需要操控的手机屏幕上
-为了触控头能稳定工作，用一根数据线插在手机充电及共地口处，让板子和手机连接
-如果不想让手机充电，就把手机充电开关拨到另一边
 4. 如果你不想用纳米胶粘在手机上，也可以启动WiFi模式来控制手机
 5. 一个板子配一台手机，电脑上USB接口足够多的话，可以实现一台电脑同时操控多台手机
 6. 有想法的朋友们还能靠这个东西建立一些赚钱项目，目前已经有朋友实现了

下面是功能编程的代码
内容比较简单，一看就会

```python
import sys
import serial
import time

SERIALOBJ = None

#'@'工作模式字典
type2Pins = {1:['0','1'],2:['2','3'],3:['4','5'],4:['6','7'],5:['8','9'],6:['a','b'],7:['c','d'],8:['e','f'],9:['g','h'],10:['i','j'],11:['k','l'],12:['m','n'],13:['o','p'],14:['q','r'],15:['s','t'],16:['u','v']}

def sendcmd(t,cmd):
    sendstr = cmd
    print(sendstr)
    s = t.write(sendstr.encode())
    t.flush()

def touch(p):
    global SERIALOBJ
    pstr = type2Pins[p][0]
    sendcmd(SERIALOBJ, pstr)

def untouch(p):
    global SERIALOBJ
    pstr = type2Pins[p][1]
    sendcmd(SERIALOBJ, pstr)


def touchpin(n):
    if n == 0:
        n = 10
    touch(n)
    time.sleep(0.03)
    untouch(n)
    time.sleep(0.03)
def main():
    global SERIALOBJ
    t = serial.Serial('com12',115200,timeout=1)
    SERIALOBJ = t
    if t:
        print(t.name)               #串口名
        print(t.port)               #串口号
        print(t.baudrate)           #波特率
        print('-'*10)
        time.sleep(1)
        sendcmd(t, '@')
        for i in range(10):
            touchpin(1)
            time.sleep(5)
            touchpin(2)
            time.sleep(25)
            touchpin(3)
            time.sleep(155)
            
        

        t.close()
    else:
        print('串口不存在')

if __name__ == '__main__':
    main()
```


方框框起来的这一部分主要是为了刷视频领金币提现，所以我自己写的一个流程参数。可以给大家解释一下像我这个内容怎么编写。

```python
 for i in range(10):
            touchpin(1)
            time.sleep(5)
            touchpin(2)
            time.sleep(25)
            touchpin(3)
            time.sleep(155)
```
我这个流程就是

 - 打开收益页面 
 - 点击右下角开宝箱的金币—等待5s（手机卡顿）—弹出窗口提示看视频 
 - 点击看视频最高再领100金币——等待5s——视频20s
 - 点击右上角领取成功 回到收益页面 等待2分30s——等待5s（手机卡顿） 
 - 循环以上操作10次
 - 实际上你修改为无限循环也可以，让他一直自动刷视频领金币，每天都帮你自动挣钱

当然，这就是一个用法而已，实际上这个板子用法比较多，这部分你可以自己写一些需求，让点击器自动执行。也可以利用板子尝试干一些小项目。


像DNF手游玩家就可以根据游戏进程，把操作模型训练出来写个代码，让点击器自动帮你玩，在一些费时间，动作重复的游戏过程中解放双手。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c130fd6f853941e8914ca4809bdacfad.png)

点击头上有纳米胶，使用时揭掉点击头表面一层保护模后，点击头可以牢固的直接粘附在手机屏上，对手机屏没有伤害，可多次使用。

如果没有纳米双面胶店主将会送一卷纳米胶,可自行贴上纳米双面胶

板子使用说明B站视频链接: [link](https://www.bilibili.com/video/BV1YK4y1Y7g4)

开机灯快闪时按一下按键可进行串口接收指令模式,在串口指令模式时,常按按键10秒以上,可再次切换到WiFi模式,或者在串口模式时发送'$’字符也可以恢复出厂设置.


WiFi模式需要配网:

1).微信配网:微信公众号使用”安信可科技”的微信配网功能进行配网
2).手机app配网工具:可旺旺上问”配网工具”

网盘下载地址:https://pan.baidu.com/s/1drXEkUip]S9jdMa46-ihfg 提取码: reyw



**结尾**：板子是店主自己开发的，感兴趣的朋友们欢迎来购买，懂技术的朋友也可以买回去自己研究学习~

有想了解更多的朋友点击下面链接，和店主直接沟通更赞！

**地址链接**: [link](https://item.taobao.com/item.htm?id=623775223795&pisk=f_jXv_MXXjcjyxT7aZeyRiCLhXx6lsZEhA9OKOnqBnKx5c15tE82HFKRP9f6gR4D0aO1w_OA1O2DBCCO1C74zkWcnhxTTfZUYtfSH7WNGcK9wRp6CceHRGeOnhxT1YoT8k6cOAbpfzWxggM8rrENK7io44YzT04-OIdVVKT8ztdJq3ywnETChQNL6giHlBBvNcFEaNz91QjLaz6l1ZC2E1ZQNhflW1p1wbFwoOQ5NK68d2dfm95l4UDtkTs6GeIvPAiwqtvO6F_uODJX0aTCcZ2iwtCeGwKcQvNVFe_W-LL-CqKV89SMJiEjtQ8HCsRP9knW2FIyqDRQv83sFem6FBy7FV0MZxrfgngUc7LvEKTzF8GASEpkFBy7FV0MkLvXL8wSMNf..&skuId=5111864819596&spm=a21xtw.29178619.product_shelf.2.3408105avn2hTH)


后续将给大家带来更多产品介绍