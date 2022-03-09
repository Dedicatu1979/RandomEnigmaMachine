# RandomEnigmaMachine
A simple random rotors Enigma machine with Python.

一个简易的使用Python编写的随机转子恩尼格玛机。

这是一个模拟转子可随机生成的三转子恩尼格玛机的程序，
本程序可以作为小程序直接运行交互式使用，也可作为模块导入非交互式使用。

本程序可通过修改随机数生成种子来达到模拟修改转子内部接线的功能，故本程序模拟的恩尼格玛机**与实际的恩尼格玛机加密结果基本完全不同**。

本程序提供两种恩尼格玛机的类型，一种加密范围为26罗马字母（经典模式），另一种的范围是94ASCII（ASCII模式）。在加密范围外的字符将不被加密。

在经典模式下，大小写不敏感且结果一律为大写字母。非经典模式下，为了防止空格间的混淆，" "(Space ASCII 32)将会被替换为"_"(Underline ASCII 95)。

其中转子的实现原理可以看这里：(https://www.bilibili.com/read/cv14852910)

## 简易使用方法说明
非交互模式：
```
import enigma as enm          # 建议将此文件作为模块导入。
enm.setting(114514, False)    # setting中是用于设置主要配置的，114514设置的是种子，种子可以是任意非0的整数，False代表非经典模式。
enm.setSite(1, 1, 4)          # 设置码盘位置，也可直接在setting中设置。
text = r'''Hello world!!!'''  # 设置想要加密的内容，推荐在ascii模式下使用ascii内的字母，经典模式下同理，使用原始字符串是为了防止出现奇奇怪怪的问题
cipher = enm.run(text)
print(cipher)
```
以上返回结果：“rm[(h*|^mTU{C%”

交互模式：
```
# 当前恩尼格玛机的种子为 114514，码盘位置为：(0, 0, 0)，模式为经典模式。
# 可以通过"/help"获取更多信息。              # /xxxx可以表示输入指令。
:->                                        # ':->'即表示到了由用户进行输入的回合了
>>> Hello World!!!               
FWHMS OYPGG!!!                              # 系统返回的加密值。
# 当前码盘位置为： (0, 0, 10)                 # 系统返回的当前码盘值。
:-> 
>>> /site(0,0,0)                           # 使用site指令修改码盘值。
-------------------------------             # 使用指令后会显示分割线。
# 当前码盘位置为： (0, 0, 0)
:->   
>>> /swap('l','o')                         # 调换字母指令，即类似于实际恩尼格玛机的接线板的作用，详细信息请查看setSwap函数的文档。
# 当前交换组为： [('L', 'O')] 
-------------------------------
# 当前码盘位置为： (0, 0, 0)
:-> 
>>> Hello World!!!
FWZPU LFPAG!!!
# 当前码盘位置为： (0, 0, 10)
:->
```
交互模式下，默认为经典模式，种子为114514，码盘位置为零位，若需要修改，请使用/setting()。不清楚如何使用指令可以使用/help查看指令用法。

在交互模式下，若需要加密的文字首字母为"/"，则双写表示转义，若斜杠不是首字母，则不需要转义，例如："//baidu.com/"表示的是"/baidu.com/"。

在交互模式下使用指令，若指令本身没有参数需要输入或者允许不输入参数，则仅需要输入函数名即可例如"/getSeed"不需要写成"/getSeed()"，若指令需要输入参数，则使用语法同Python本身的语法，例如"/setting(1919810, classical=True, code_site = (1, 1, 4))"


### 函数功能说明
| 函数         |    函数说明            |
| ----------   | -------------- |
| setting      | 定义初始设定，其包括种子，是否为经典模式以及码盘位置 |
| setSite      | 设置转子位置，按转子左中右的顺序写入转子码盘左中右当前的值         |
| getSite      | 返回当前的转子位置，顺序为转子0，1，2 左中右 |
| isClassical  | 返回是否为经典模式，True表示经典模式 |
| getSeed      | 返回当前的随机数种子 |
| setSwap      | 设置交换的字母，其模拟的是恩尼格玛机的接线板 |
| getSwap      | 以列表形式返回当前交换的字母  |
| setGroupSwap | setSwap的组函数版，可以一次性交换多组字母    |
| clearSwap    | 将所有已创建的字母交换值清空  |
| deleteGroupSwap | 删除一组交换字母，一组内可以有多组不同的交换字母 |
| swap | 与setSwap函数功能相同，建议仅在交互模式下使用本函数 |
| site | 与setSite函数功能一样，建议仅在交互模式下使用本函数 |
| run  | 输入需要加密的文字，以及返回加密后的文字 |
| help | 用于帮助使用者了解函数的使用方法 |
