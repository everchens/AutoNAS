# AutoNAS
A simple automatic trading strategy for NAS100 in FXCM MT4

Platform: FXCM MT4

Feature: This script is totally automatic. No manual intervention is necessary. It can be applied for NAS100 and USOil, with a few parameters adjusted. According to my experience, the results for NAS100 is more stable.

Known issue: The program could halt when orders openning continuously failed. This may happen when the server is busy.

You can use this script however you like.

# AutoNAS
一个简单的期货自动交易策略


两年前写过一个自动交易策略，运行结果还不错，与大家共享。


介绍：

1、平台：FXCM MT4。

2、不需要任何金融知识。

3、全自动运行，不需要人工参与。

4、适用范围：（1）USOil和NAS100的历史数据验证结果很好，其中NAS100更稳定。（2）NAS100实操3个月的结果还可以。

5、存在问题：系统繁忙时会无法下单，导致程序卡死，产生无法预料的结果，希望高手继续优化。

使用方法：

1、下载并安装MT4，注册模拟账号或者真实账号，登录。

2、将代码放入指定文件夹。

3、导入代码、运行。

4、如果跑历史数据，需安装FXCM MT5。

原理：

以一个上涨周期为例，特征值为a。a是一个自定义变量，后续所有的判断阈值都和a相关。

1、初始状态，没有任何订单，记录商品的初始价格为back_price。

2、监测实时价格，并和back_price比较，判断差值是否超越阈值，此时阈值为1.5×a。

3、如价格上涨超过1.5×a，则程序会认为还将继续上涨，于是新建买单，同时更新back_price为下单时的价格。

4、建立买单后，继续监测价格波动：此时需要用到3个价格：（1）下单时的价格back_price，（2）实时价格new_price，（3）自下单后记录到的最高价格middle_price。将最高价格与下单价格之间的价差记作最大价差gap=middle_price-back_price，将实时价格与最高价格之间的回撤记作retracement=new_price-middle_price，将实时价格与下单价格之间的价差记作波动fluctuation=new_price-back_price。

5、若最大价差gap没有超过2×a，而此时的实时价格已经低于下单价格，且波动fluctuation超过了-1.5×a，则程序判断这是一次伪上涨，并即将进入下行通道，于是关闭买单，新建卖单。

6、若最大价差gap超过了2×a，但同时回撤retracement也超过了gap/2，则程序判断此次上涨周期已经结束，并即将进入下行通道，于是关闭买单，新建卖单。

7、若最大价差gap超过了4×a，则程序判断即将进入新一轮上涨周期，于是更新下单价格back_price=middle_price-1×a，并同时更新gap和fluctuation。

8、若下单时间超过6小时，且过去6小时的均值波动不超过1.3×a，且fluctuation的绝对值超过了1×a，则程序认为此次上涨周期已经结束，关闭订单，不建立新订单，并以最近1小时的均值价格作为新的back_price。

9、下降周期的判断逻辑与上涨类似，不再赘述。

原理很简单，无需金融知识，几个阈值的判断而已。


浅尝辄止，欢迎验证，不到之处请拍砖。
