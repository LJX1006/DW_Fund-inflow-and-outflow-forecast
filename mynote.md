# DW_Fund-inflow-and-outflow-forecast
组队学习
## 数据探索与分析  
(一)时间序列图  
(二)数据分布可视化  
(三)变量间相关性分析与独立性分析

1.关于时间序列分析：  
获取用户申购赎回数据，并为其添加时间戳，添加时间戳的方式比较统一，主要为如下方式：  
data['date']=pd.to_datetime(data['xxx'],format="%Y%m%d")  
data['day']=data['date'].dt.day  
data['month']=data['date'].dt.month  
data['year']=data['date'].dt.year  
data['week']=data['week'].dt.week  
data['weekday']=data['weekday'].dt.weekday  
当然还有其他时间维度，比如：  
data['quarter']=data['date'].dt.quarter  
data['dayofwork']=data['date'].dt.dayofwork  
data['is_year_start']=data['date'].dt.is_year_start  
data['is_year_end']=data['date'].dt.is_year_end  
data['is_month_start']=data['date'].dt.is_month_start  
data['is_month_end']=data['date'].dt.is_month_end  
1)主要想对总购买量和总赎回量进行相关分析，因此根据时间进行聚合操作，并统计分析每日总购买量和赎回量的时序图，此处的时间序列图是采用matplotlib的函数进行绘制：  
plt.plot(x,y,ls='-',lw=2,label)  
x:x轴上的数值  y:y轴上的数值  ls:折线图的线条风格 lw:折线图的线条宽度 label:标记图内容的标签文本  
2）可以分别按不同时间维度进行数据分析，比如某个月前后的每日/总购买量/赎回量、某月中每天的购买量和赎回量以及每个翌日（每周）的总购买量和总赎回量的数据分布，可使用的绘图函数：  
箱型图：seaborn.boxplot(x=None, y=None, hue=None, data=None, order=None, hue_order=None, orient=None, color=None, palette=None, saturation=0.75, width=0.8, dodge=True, fliersize=5, linewidth=None, whis=1.5, notch=False, ax=None, **kwargs)（箱型图也可以用来分析异常数据）  
小提琴图：seaborn.violinplot(x=None, y=None, hue=None, data=None, order=None, hue_order=None, bw='scott', cut=2, scale='area', scale_hue=True, gridsize=100, width=0.8, inner='box', split=False, dodge=True, orient=None, linewidth=None, color=None, palette=None, saturation=0.75, ax=None, **kwargs)  
violinplot与boxplot扮演类似的角色，它显示了定量数据在一个（或多个）分类变量的多个层次上的分布，这些分布可以进行比较。不像箱形图中所有绘图组件都对应于实际数据点，小提琴绘图以基础分布的核密度估计为特征。  
3）重点：相关性分析及独立性分析！！！  
这里在对每月的购买量和赎回量进行分析还用到了seaborn中的kdeplot核密度估计图：  
核密度估计(kernel density estimation)是在概率论中用来估计未知的密度函数，属于非参数检验方法之一。通过核密度估计图可以比较直观的看出数据样本本身的分布特征。具体用法如下：  
seaborn.kdeplot(data,data2=None,shade=False,vertical=False,kernel='gau',bw='scott',gridsize=100,cut=3,clip=None,legend=True,cumulative=False,shade_lowest=True,cbar=False, cbar_ax=None, cbar_kws=None, ax=None, *kwargs)  
4）在绘制图的过程中若在某个时间点有较明显的浮动（异常数据也包括），应该考虑到该时间点是否为假期或假期邻近/刚过的时间。  
5）对于异常值的分析：  
用箱型图最为合适，怎么根据箱型图判断出的异常数据点进行处理也是一个重要的问题（后续应该会涉及到...）  
在时间序列数据中，重点考虑的属性特征主要包括：  
·工作日  
·是否是周末  
·是否是假期  
·距周一有多长时间  
·距周末有多长时间  
·距月初有多长时间  
·距月末有多长时间  
·上个月同一周数据的均值/最大值/最小值  
·上个月月末的数据值  

2.关于用户相关的分析：  
首先获取用户申购赎回数据为dataframe格式，并为该数据表添加时间戳，分别为"day","month","year","week","weekday"，为时间序列数据添加时间戳是比较重要的一点，后期可以按需根据不同的时间  
粒度对数据进行聚合分析等。此部分主要以考虑用户的相关特征对资金流入流出情况的影响，因此还需读取用户信息表，并将用户申购赎回数据中重点考虑的特征"今日总购买量"和"今日总赎回量"按"date"时间  
维度进行汇总。  
1）按总购买量或总赎回量超过100万的用户为大额用户，其余的用户为小额用户，可以以大额用户和小额用户为基准统计二者在日交易总额上的差异；  
2）将用户信息表和用户申购赎回数据表进行合并，并分析用户的其他属性（如性别、星座、城市）对总购买量和总赎回量及日交易总额的影响。  
3.关于支付宝数据的相关分析：  
首先仍然是拿到数据可以先对其加时间戳，在这里还是分别添加"day","month","year","week","weekday"维度;  
1)分析支付宝利率与每日利息的增长/直接购买量的时序图；  
2）分析支付宝利率与交易额的时序图；  
3）考虑到有两种不同的购买方式：支付宝余额购买和银行卡购买，所以需要根据购买方式的不同分析其对日购买量和日赎回量的影响：经统计分析可以得出的结论是，在日购买量上，使用支付宝余额购买的方式远远多于银行卡购买方式，在日赎回量上情况也相同。  
