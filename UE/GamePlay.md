### Actor和Component
- Actor之间的不同主要由Component决定，可以让Actor继承各种Component来定制独特的Actor组件。
- AActor在world中存在的。
### Level和World
- Level可以理解为存放Actor的容器，其本身也是一个Actor。
- World由一个或多个Level组成，这些Level中有一个主Level称为PersistentLevel，它会在world生成时最先生成，玩家就出生在这里。而WorldSetting也以PersistentLevel为主。
- 在多个Level组成一个World后会发现，在WorldOutlinear中可以看见所有Level的Actor对象，这些Actor的物理实体都在World里。
- 编辑器，编辑窗口，以及点击播放口形成的演示窗口本质上都是一个个World。
### WorldContext和GameInstance
- WorldContext保存着切换World时的上下文信息，以及Level的切换信息。之所以不直接保存到World中是因为当我们OpenLevel一个新的PersistentLevel时，会先释放掉当前的World再新建一个，如果把下一个Level的信息放到World中，则需要在释放World前拷贝一下这个信息，影响性能。而LoadStreamLevel时则不会有这个问题。
- GameInstance用来存储WorldContext和其他整个游戏的信息，它是一个比World更高的层次。
### PlayerState
- 可以跨关卡保存数据。
### Player
- UPlayer因为要在不同的level和world之间穿梭，所以直接继承自UObject，而不是AActor；并且是比world更高一级的存在。
### GameInstance
- 作为Engine下面的一级存在，掌握着全局的话事权。
- 用于存储不同World以及Levle之间切换的逻辑。
- 用于动态添加，删除Player。
- 存储一些全局配置，如控制台命令。
- 也可以存储自己编写的第三方逻辑，如网络通信，自定义配置文件，程序算法。
## 一图胜千言：
![image](https://github.com/zhthero/LearningRecord/assets/73262783/b2c183e7-3ee3-42bd-9dc7-aa6011f41d28)
### UE的代码生成方案：
- UE的UHT方案是在源文件中实现空的宏做标记，然后用UHT生成generated.h/cpp文件，然后再一起编译。
- 相当于UHT会识别特定的宏，如UCLASS(), UPROPERTY(), UFUNCTION()等；然后生成boilerplate代码，用于实现反射系统，让UE可以在运行过程中获取这个对象的属性和方法，以此可以修改一些参数，属性如Actor的各类数值。也可以实现对游戏的保存等功能，可以说这是由C++代码转换成UE功能的桥梁。
