

# 2023中职物联网技术应用与维护赛项（学生组样卷）
*上面的每一个压缩包都是此样题*
*注意，SBRH1指的是题目1，SBRH2指的是题目2，前面的为工程地址*

## 题目1：环境监测子系统

新建 Android 项目，利用提供的软件资源和图片资源，完成小区环境监测子系统。

### 任务要求：

设计如下图界面，实时获取温度信息、湿度信息、二氧化碳和噪音信息（APP 图示数据是参考，不是真实数据）。具体实现如下功能：
（1）程序要求通过串口服务器获取数据、控制执行器，串口服务器采用 TCP 模式进行通讯；
（2）启动程序，程序正常运行，点击“运行”，实时采集数据，运行状态显示“运行”；
（3）点击“发送”，程序数据发送给 LED 屏幕显示文字“温度:xxx,湿度:xxx,二氧化碳:xxx, 噪音:xxx”,其中xxx为APP实时采集的数据；
（4）点击“停止”，程序停止数据采集，运行状态显示“停止”

<img src="https://github.com/Kanyooooo/2023-Jiangsu-Vocational-College-Skills-Competition-Sample-volume-of-secondary-vocational-group/blob/main/photos/1.png" title="" alt="" width="201">

完成以上任务后请做以下步骤：
（1）开发完成后，请将程序以“环境监测子系统”命名，发布到
移动互联终端，并连接好网络。
（2）把源码拷贝到 U 盘“提交资料\任务五\题 2”目录下。

### 解题：

界面很简单，用线性布局看就是7个横着的块叠在一起：

<img src="https://github.com/Kanyooooo/2023-Jiangsu-Vocational-College-Skills-Competition-Sample-volume-of-secondary-vocational-group/blob/main/photos/2.png" title="" alt="" data-align="center">

在LinearLayout里，再进行文字布局就行。
这样是为了修改的数据更少，方便我写.java部分而准备的。
如上图所示，取一黄色内的布局作参考。

```xml
<LinearLayout  
 android:layout_width="wrap_content"  
 android:layout_height="wrap_content"  
 android:layout_margin="10dp">

<TextView        
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:text="温度："  
    android:textColor="#000000"  
    android:textSize="20dp" />  

<TextView        
    android:id="@+id/temp_tv"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:text="10℃"  
    android:textColor="#000000"  
    android:textSize="20dp" />  
</LinearLayout>
```

实际上，也可以用如下方式：

<img src="https://github.com/Kanyooooo/2023-Jiangsu-Vocational-College-Skills-Competition-Sample-volume-of-secondary-vocational-group/blob/main/photos/3.png" title="" alt="" data-align="center">

倒数第二行的LinearLayout是为三个Button控件做准备的。其余一律TextView。
代码我没有写过（以后应该会测试速度，到时候再说）

大概代码如下：

```xml
<TextView        
    android:id="@+id/temp_tv"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:text="温度：10℃"  
    android:textColor="#000000"  
    android:textSize="20dp" /> 
```

反正，在布局的时候省的时间，会按照一定比例在写后端的付出时间。具体这个比值大于或小于1，就要写代码来测试了。（测试完后后面会补上。）

**总之，我用的是第一种布局方法。**

把最后的蓝图展示一下：

<img title="" src="https://github.com/Kanyooooo/2023-Jiangsu-Vocational-College-Skills-Competition-Sample-volume-of-secondary-vocational-group/blob/main/photos/4.png" alt="" data-align="center" width="430">

代码就自己下载下来自己看吧。



接下来是后端。实际上没啥难的。

接下来是导入jar包，我把这个部分放在最后 *(因为每一道题都需要用到，我会让我的学弟先从这道题开始练，所以这个什么jar包我就把他放在最后面，方便需要时候可以参考)* 。

**必须先导入jar包，否则接下来的内容你没法完成**  , 所以跳到最后面先看导入jar吧(qaq)。

然后正式开始写后端的代码。

逻辑就自己看吧，我主要讲一下jar包的用法。首先是声明对象和初始化:

```java

public class MainActivity extends AppCompatActivity {
///---------------------------------------------------------
//记住，声明对象一定要在类里面，方法(函数)外面声明
//让这两个对象作为全局的对象。
MD4017 md4017;          //modbus4017  读取温湿度这些数据的那个设备
LedScreen ledScreen;    //led屏
//----------------------------------------------------------
 @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //---------------------------------------------------------------------------------
        //在主函数中初始化，否则不可使用此对象，并且会有空指针错误。
        ledScreen = new LedScreen(DataBusFactory.newSocketDataBus("10.11.11.16", 6002));
        md4017 = new MD4017(DataBusFactory.newSocketDataBus("10.11.11.16", 6004));
        //---------------------------------------------------------------------------------
}
```

接着是使用。没啥问题，自己去代码里面看吧。

```java
send_bt.setOnClickListener(new View.OnClickListener() {  
    @Override  
    public void onClick(View view) {  
        try {  
            ledScreen.sendTxt(vals[0] +Md4017VIN.TEM.getUnit()+"      "+  
                    vals[1] +Md4017VIN.HUM.getUnit()+"      "+  
                    vals[2] +Md4017VIN.CO2.getUnit()+"      "+  
                    vals[3] +"db      ", PlayType.LEFT, ShowSpeed.SPEED1,0,999);  
        } catch (Exception e) {  
            e.printStackTrace();  
        }  
    }  
});
```

```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
        //······一些初始化的代码
        handler = new Handler();  
        handler.post(runnable);  
    }  
    boolean b = true;
    double vals[] = new double[4];
    Runnable runnable = new Runnable() {
        @Override
        public void run() {
            if (b == true) {
                try {
                    md4017.getVin(new MD4017ValListener() {
                        @Override
                        public void onVal(int[] ints) {
                            vals[0] = MD4017ValConvert.getRealValByType(Md4017VIN.TEM, ints[0]);
                            vals[1] = MD4017ValConvert.getRealValByType(Md4017VIN.HUM, ints[1]);
                            vals[2] = MD4017ValConvert.getRealValByType(Md4017VIN.CO2, ints[2]);
                            vals[3] = MD4017ValConvert.getRealValByType(Md4017VIN.AIR, ints[3]);
                            temp_tv.setText(vals[0] +Md4017VIN.TEM.getUnit());
                            hum_tv.setText(vals[1] +Md4017VIN.HUM.getUnit());
                            co2_tv.setText(vals[2] +Md4017VIN.CO2.getUnit());
                            noise_tv.setText(vals[3] +"db");
                        }
                    });
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            handler.postDelayed(runnable, 2000);
        }
    };
```

我少了很多代码在里面。具体的就去看工程吧（工程没写多少注释）

###结果
20221026SBRH1.zip    费间20:10.16



## 导入jar包：

**注意，如果你从头看到尾发现我上面没有让你导入jar包，那你一定是****

*如果你是因为看到我让你先来导入jar包而来的，就当我上面那句话没说*

1、当你的工程目录查看方式为Android时候（![](https://github.com/Kanyooooo/2023-Jiangsu-Vocational-College-Skills-Competition-Sample-volume-of-secondary-vocational-group/blob/main/photos/5.png)），找到manifests文件夹（也是个文件，直接双击打开），然后设置网络权限。：

<img src="https://github.com/Kanyooooo/2023-Jiangsu-Vocational-College-Skills-Competition-Sample-volume-of-secondary-vocational-group/blob/main/photos/6.png" title="" alt="" data-align="center">

这一步不是导入jar包所必要的条件，但是确实是让jar包可以正常使用的条件。你导入的那几个玩意只要用到串口服务器，都需要这个网络权限。

同理，如果做的软件需要上网或者通过网络获取数据，就需要加这一条。为了防止你(我自己)忘记，我就把他和导入jar包写在一起。





2、将工程目录查看方式设置为Project（![](https://github.com/Kanyooooo/2023-Jiangsu-Vocational-College-Skills-Competition-Sample-volume-of-secondary-vocational-group/blob/main/photos/7.png)），找到app目录下的libs。将资料包内的内容复制在工程中libs目录下：

<img src="https://github.com/Kanyooooo/2023-Jiangsu-Vocational-College-Skills-Competition-Sample-volume-of-secondary-vocational-group/blob/main/photos/8.png" title="" alt="" data-align="center">

3、在app目录下的build.gradle文件中的 *android{  }* 下 加入如下代码：

```java
    sourceSets {   
        main{
            jniLibs.srcDirs = ['libs']
        }
    }
```

<img src="https://github.com/Kanyooooo/2023-Jiangsu-Vocational-College-Skills-Competition-Sample-volume-of-secondary-vocational-group/blob/main/photos/9.png" title="" alt="" data-align="center">

4、最后一步，在右上角有一个项目管理器（网上都这么说，但是原意是项目结构？）

<img src="https://github.com/Kanyooooo/2023-Jiangsu-Vocational-College-Skills-Competition-Sample-volume-of-secondary-vocational-group/blob/main/photos/10.png" title="" alt="" data-align="center">

我是习惯性用快捷键的。

进去之后，如下操作：

<img src="https://github.com/Kanyooooo/2023-Jiangsu-Vocational-College-Skills-Competition-Sample-volume-of-secondary-vocational-group/blob/main/photos/11.png" title="" alt="" data-align="center">

注意，上面应该少了一个选择导入jar包的步骤，在2~3之间。我就不做演示了，因为很没有必要，太简单了。

记住，有两个jar包。一个一个导入进去就可以了。

最后结果如下。

<img src="https://github.com/Kanyooooo/2023-Jiangsu-Vocational-College-Skills-Competition-Sample-volume-of-secondary-vocational-group/blob/main/photos/12.png" title="" alt="" data-align="center">





## 最后感言：

写这个玩意是因为在网安摸鱼的时候，他们学习记录的方式都是些writeup（WP），然后我也想写，但是程序怎么写捏？
我思考了一段时间，决定用.md的方式写，然后上传在github上。希望以后的我看到这些依旧会欣慰。

*主要是在学校这样子被看到了也不会被当做是摸鱼，太棒了！！！*
