```python
import requests
import pandas as pd
from lxml import etree

html = 'https://ncov.dxy.cn/ncovh5/view/pneumonia'
html_data = requests.get(html)
html_data.encoding = 'utf-8'
html_data = etree.HTML(html_data.text, etree.HTMLParser())
html_data = html_data.xpath(
    '//*[@id="getListByCountryTypeService2true"]/text()')  # xpath方法选择疫情的数据集合
ncov_world = html_data[0][49:-12]
ncov_world = ncov_world.replace('true', 'True')
ncov_world = ncov_world.replace('false', 'False')
ncov_world = eval(ncov_world)

country = []
confirmed = []
lived = []
dead = []

for i in ncov_world:  # 分离国家名称，确诊人数，治愈人数和死亡人数并存入dataframe里备用
    country.append(i['provinceName'])
    confirmed.append(i['confirmedCount'])
    lived.append(i['curedCount'])
    dead.append(i['deadCount'])
    
data_world = pd.DataFrame()
data_world['国家名称'] = country
data_world['确诊人数'] = confirmed
data_world['治愈人数'] = lived
data_world['死亡人数'] = dead
data_world.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>国家名称</th>
      <th>确诊人数</th>
      <th>治愈人数</th>
      <th>死亡人数</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>法国</td>
      <td>29583616</td>
      <td>368023</td>
      <td>149044</td>
    </tr>
    <tr>
      <th>1</th>
      <td>德国</td>
      <td>26200663</td>
      <td>4328400</td>
      <td>138781</td>
    </tr>
    <tr>
      <th>2</th>
      <td>韩国</td>
      <td>18053287</td>
      <td>336548</td>
      <td>24103</td>
    </tr>
    <tr>
      <th>3</th>
      <td>英国</td>
      <td>22455392</td>
      <td>6491069</td>
      <td>178880</td>
    </tr>
    <tr>
      <th>4</th>
      <td>西班牙</td>
      <td>12311477</td>
      <td>150376</td>
      <td>106105</td>
    </tr>
  </tbody>
</table>
</div>




```python
import pandas as pd
data_world = pd.read_csv('https://labfile.oss.aliyuncs.com/courses/2791/data_world.csv')
data_world.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>国家名称</th>
      <th>确诊人数</th>
      <th>治愈人数</th>
      <th>死亡人数</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>法国</td>
      <td>27626578</td>
      <td>368023</td>
      <td>144130</td>
    </tr>
    <tr>
      <th>1</th>
      <td>德国</td>
      <td>23376879</td>
      <td>4328400</td>
      <td>132929</td>
    </tr>
    <tr>
      <th>2</th>
      <td>韩国</td>
      <td>16212751</td>
      <td>336548</td>
      <td>20889</td>
    </tr>
    <tr>
      <th>3</th>
      <td>英国</td>
      <td>21819851</td>
      <td>6491069</td>
      <td>171560</td>
    </tr>
    <tr>
      <th>4</th>
      <td>西班牙</td>
      <td>11662214</td>
      <td>150376</td>
      <td>103266</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_economy = pd.read_csv(
    "https://labfile.oss.aliyuncs.com/courses/2791/gpd_2016_2020.csv", index_col=0)
time_index = pd.date_range(start='2016', periods=18, freq='Q')
data_economy.index = time_index
data_economy
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>国内生产总值</th>
      <th>第一产业增加值</th>
      <th>第二产业增加值</th>
      <th>第三产业增加值</th>
      <th>农林牧渔业增加值</th>
      <th>工业增加值</th>
      <th>制造业增加值</th>
      <th>建筑业增加值</th>
      <th>批发和零售业增加值</th>
      <th>交通运输、仓储和邮政业增加值</th>
      <th>住宿和餐饮业增加值</th>
      <th>金融业增加值</th>
      <th>房地产业增加值</th>
      <th>信息传输、软件和信息技术服务业增加值</th>
      <th>租赁和商务服务业增加值</th>
      <th>其他行业增加值</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-03-31</th>
      <td>162410.0</td>
      <td>8312.7</td>
      <td>61106.8</td>
      <td>92990.5</td>
      <td>8665.5</td>
      <td>53666.4</td>
      <td>45784.0</td>
      <td>7763.0</td>
      <td>16847.5</td>
      <td>7180.5</td>
      <td>3181.6</td>
      <td>15340.4</td>
      <td>11283.0</td>
      <td>5128.8</td>
      <td>4985.3</td>
      <td>28368.1</td>
    </tr>
    <tr>
      <th>2016-06-30</th>
      <td>181408.2</td>
      <td>12555.9</td>
      <td>73416.5</td>
      <td>95435.8</td>
      <td>13045.5</td>
      <td>60839.2</td>
      <td>52378.3</td>
      <td>12943.8</td>
      <td>17679.8</td>
      <td>8295.0</td>
      <td>3112.3</td>
      <td>14811.7</td>
      <td>12209.7</td>
      <td>5130.7</td>
      <td>5075.1</td>
      <td>28265.4</td>
    </tr>
    <tr>
      <th>2016-09-30</th>
      <td>191010.6</td>
      <td>17542.4</td>
      <td>75400.5</td>
      <td>98067.8</td>
      <td>18162.2</td>
      <td>61902.5</td>
      <td>52468.3</td>
      <td>13870.6</td>
      <td>18513.0</td>
      <td>8591.6</td>
      <td>3473.2</td>
      <td>14945.4</td>
      <td>12615.3</td>
      <td>4662.3</td>
      <td>5452.4</td>
      <td>28822.1</td>
    </tr>
    <tr>
      <th>2016-12-31</th>
      <td>211566.2</td>
      <td>21728.2</td>
      <td>85504.1</td>
      <td>104334.0</td>
      <td>22577.8</td>
      <td>68998.4</td>
      <td>58878.4</td>
      <td>16921.5</td>
      <td>20684.1</td>
      <td>8961.6</td>
      <td>3840.7</td>
      <td>14866.4</td>
      <td>13861.4</td>
      <td>5202.3</td>
      <td>6015.8</td>
      <td>29636.1</td>
    </tr>
    <tr>
      <th>2017-03-31</th>
      <td>181867.7</td>
      <td>8205.9</td>
      <td>69315.5</td>
      <td>104346.3</td>
      <td>8595.8</td>
      <td>60909.3</td>
      <td>51419.7</td>
      <td>8725.3</td>
      <td>18608.9</td>
      <td>8094.5</td>
      <td>3536.5</td>
      <td>16758.8</td>
      <td>13047.0</td>
      <td>5915.2</td>
      <td>5811.9</td>
      <td>31864.3</td>
    </tr>
    <tr>
      <th>2017-06-30</th>
      <td>201950.3</td>
      <td>12644.9</td>
      <td>82323.0</td>
      <td>106982.4</td>
      <td>13204.2</td>
      <td>68099.8</td>
      <td>58172.1</td>
      <td>14574.4</td>
      <td>19473.6</td>
      <td>9397.7</td>
      <td>3440.9</td>
      <td>15856.3</td>
      <td>14059.0</td>
      <td>5977.9</td>
      <td>5868.4</td>
      <td>31998.1</td>
    </tr>
    <tr>
      <th>2017-09-30</th>
      <td>212789.3</td>
      <td>18255.8</td>
      <td>84574.1</td>
      <td>109959.5</td>
      <td>18944.2</td>
      <td>69327.2</td>
      <td>58632.6</td>
      <td>15590.1</td>
      <td>20342.9</td>
      <td>9688.7</td>
      <td>3838.5</td>
      <td>16290.4</td>
      <td>14054.9</td>
      <td>5539.8</td>
      <td>6464.6</td>
      <td>32708.0</td>
    </tr>
    <tr>
      <th>2017-12-31</th>
      <td>235428.7</td>
      <td>22992.9</td>
      <td>95368.0</td>
      <td>117067.8</td>
      <td>23915.8</td>
      <td>76782.9</td>
      <td>65652.1</td>
      <td>19015.8</td>
      <td>22731.1</td>
      <td>9940.9</td>
      <td>4240.1</td>
      <td>15938.8</td>
      <td>15925.1</td>
      <td>6376.0</td>
      <td>7128.4</td>
      <td>33433.7</td>
    </tr>
    <tr>
      <th>2018-03-31</th>
      <td>202035.7</td>
      <td>8575.7</td>
      <td>76598.2</td>
      <td>116861.8</td>
      <td>9005.8</td>
      <td>66905.6</td>
      <td>56631.9</td>
      <td>10073.8</td>
      <td>20485.5</td>
      <td>8806.5</td>
      <td>3887.8</td>
      <td>18050.6</td>
      <td>14863.5</td>
      <td>7212.2</td>
      <td>6879.5</td>
      <td>35864.9</td>
    </tr>
    <tr>
      <th>2018-06-30</th>
      <td>223962.2</td>
      <td>13003.8</td>
      <td>91100.6</td>
      <td>119857.8</td>
      <td>13662.2</td>
      <td>75122.1</td>
      <td>64294.9</td>
      <td>16404.3</td>
      <td>21374.2</td>
      <td>10174.9</td>
      <td>3779.6</td>
      <td>17401.0</td>
      <td>16176.1</td>
      <td>7309.6</td>
      <td>6885.3</td>
      <td>35673.1</td>
    </tr>
    <tr>
      <th>2018-09-30</th>
      <td>234474.3</td>
      <td>18226.9</td>
      <td>93112.5</td>
      <td>123134.9</td>
      <td>18961.8</td>
      <td>76239.6</td>
      <td>64348.2</td>
      <td>17294.5</td>
      <td>22334.1</td>
      <td>10582.3</td>
      <td>4212.6</td>
      <td>17780.6</td>
      <td>15914.0</td>
      <td>6690.9</td>
      <td>7533.3</td>
      <td>36930.6</td>
    </tr>
    <tr>
      <th>2018-12-31</th>
      <td>258808.9</td>
      <td>24938.7</td>
      <td>104023.9</td>
      <td>129846.2</td>
      <td>25929.0</td>
      <td>82822.1</td>
      <td>70662.1</td>
      <td>21720.4</td>
      <td>24710.0</td>
      <td>10773.5</td>
      <td>4640.6</td>
      <td>17378.1</td>
      <td>17669.5</td>
      <td>7520.8</td>
      <td>8170.4</td>
      <td>37474.6</td>
    </tr>
    <tr>
      <th>2019-03-31</th>
      <td>218062.8</td>
      <td>8769.4</td>
      <td>81806.5</td>
      <td>127486.9</td>
      <td>9249.4</td>
      <td>71064.5</td>
      <td>60357.1</td>
      <td>11143.1</td>
      <td>21959.2</td>
      <td>9386.6</td>
      <td>4234.9</td>
      <td>19650.1</td>
      <td>15979.2</td>
      <td>8424.8</td>
      <td>7665.1</td>
      <td>39306.0</td>
    </tr>
    <tr>
      <th>2019-06-30</th>
      <td>242573.8</td>
      <td>14437.6</td>
      <td>97315.6</td>
      <td>130820.6</td>
      <td>15108.7</td>
      <td>79820.7</td>
      <td>68041.8</td>
      <td>17954.2</td>
      <td>23097.0</td>
      <td>10861.3</td>
      <td>4123.0</td>
      <td>19064.9</td>
      <td>17484.4</td>
      <td>8395.6</td>
      <td>7596.7</td>
      <td>39067.3</td>
    </tr>
    <tr>
      <th>2019-09-30</th>
      <td>252208.7</td>
      <td>19798.0</td>
      <td>97790.4</td>
      <td>134620.4</td>
      <td>20629.0</td>
      <td>79501.8</td>
      <td>66823.8</td>
      <td>18734.6</td>
      <td>23993.6</td>
      <td>11310.2</td>
      <td>4610.5</td>
      <td>19388.3</td>
      <td>17369.0</td>
      <td>7528.1</td>
      <td>8409.1</td>
      <td>40734.5</td>
    </tr>
    <tr>
      <th>2019-12-31</th>
      <td>278019.7</td>
      <td>27461.6</td>
      <td>109252.8</td>
      <td>141305.2</td>
      <td>28579.9</td>
      <td>86721.6</td>
      <td>73952.4</td>
      <td>23072.4</td>
      <td>26795.9</td>
      <td>11244.0</td>
      <td>5071.2</td>
      <td>18973.8</td>
      <td>18798.9</td>
      <td>8341.3</td>
      <td>9262.5</td>
      <td>41158.2</td>
    </tr>
    <tr>
      <th>2020-03-31</th>
      <td>206504.3</td>
      <td>10186.2</td>
      <td>73638.0</td>
      <td>122680.1</td>
      <td>10708.4</td>
      <td>64642.0</td>
      <td>53852.0</td>
      <td>9377.8</td>
      <td>18749.6</td>
      <td>7865.1</td>
      <td>2820.9</td>
      <td>21346.8</td>
      <td>15268.3</td>
      <td>8928.0</td>
      <td>7137.9</td>
      <td>39659.6</td>
    </tr>
    <tr>
      <th>2020-06-30</th>
      <td>250110.1</td>
      <td>15866.8</td>
      <td>99120.9</td>
      <td>135122.3</td>
      <td>16596.4</td>
      <td>80402.4</td>
      <td>69258.8</td>
      <td>19156.8</td>
      <td>23696.1</td>
      <td>10650.0</td>
      <td>3481.3</td>
      <td>20954.7</td>
      <td>18593.6</td>
      <td>9573.0</td>
      <td>7174.4</td>
      <td>39831.4</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_area = pd.read_csv('https://labfile.oss.aliyuncs.com/courses/2791/DXYArea.csv')
data_news = pd.read_csv('https://labfile.oss.aliyuncs.com/courses/2791/DXYNews.csv')
```


```python
data_area = data_area.loc[data_area['countryName'] == data_area['provinceName']]
data_area_times = data_area[['countryName', 'province_confirmedCount',
                             'province_curedCount', 'province_deadCount', 'updateTime']]

time = pd.DatetimeIndex(data_area_times['updateTime'])  # 根据疫情的更新时间来生成时间序列
data_area_times.index = time  # 生成索引
data_area_times = data_area_times.drop('updateTime', axis=1)
data_area_times.head(5)

data_area_times.isnull().any()  # 查询是否有空值
```




    countryName                False
    province_confirmedCount    False
    province_curedCount        False
    province_deadCount         False
    dtype: bool




```python
data_news_times = data_news[['pubDate', 'title', 'summary']]
time = pd.DatetimeIndex(data_news_times['pubDate'])
data_news_times.index = time  # 生成新闻数据的时间索引
data_news_times = data_news_times.drop('pubDate', axis=1)
data_news_times.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>summary</th>
    </tr>
    <tr>
      <th>pubDate</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-07-17 05:40:08</th>
      <td>美国新增71434例新冠肺炎确诊病例，累计确诊超354万例</td>
      <td>据美国约翰斯·霍普金斯大学统计数据显示，截至美东时间7月16日17:33时（北京时间17日0...</td>
    </tr>
    <tr>
      <th>2020-07-17 06:06:49</th>
      <td>巴西新冠肺炎确诊病例破201万，近六成大城市确诊病例加速增长</td>
      <td>截至当地时间7月16日18时，巴西新增新冠肺炎确诊病例45403例，累计确诊2012151例...</td>
    </tr>
    <tr>
      <th>2020-07-16 22:31:00</th>
      <td>阿塞拜疆新增493例新冠肺炎确诊病例 累计确诊26165例</td>
      <td>当地时间7月16日，阿塞拜疆国家疫情防控指挥部发布消息，在过去24小时内，阿塞拜疆新增新冠肺...</td>
    </tr>
    <tr>
      <th>2020-07-16 22:29:48</th>
      <td>​科威特新增791例新冠肺炎确诊病例 累计确诊57668例</td>
      <td>科威特卫生部当地时间16日下午发布通告，确认过去24小时境内新增791例新冠肺炎确诊病例，同...</td>
    </tr>
    <tr>
      <th>2020-07-16 21:26:54</th>
      <td>罗马尼亚新增777例新冠肺炎确诊病例 累计确诊35003例</td>
      <td>据罗马尼亚政府7月16日公布的数据，过去24小时对19097人进行新冠病毒检测，确诊777例...</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(data_world.isnull().any())
print(data_economy.isnull().any())
print(data_area_times.isnull().any())
print(data_news_times.isnull().any())  # 确认各个数据集是否空集
```

    国家名称    False
    确诊人数    False
    治愈人数    False
    死亡人数    False
    dtype: bool
    国内生产总值                False
    第一产业增加值               False
    第二产业增加值               False
    第三产业增加值               False
    农林牧渔业增加值              False
    工业增加值                 False
    制造业增加值                False
    建筑业增加值                False
    批发和零售业增加值             False
    交通运输、仓储和邮政业增加值        False
    住宿和餐饮业增加值             False
    金融业增加值                False
    房地产业增加值               False
    信息传输、软件和信息技术服务业增加值    False
    租赁和商务服务业增加值           False
    其他行业增加值               False
    dtype: bool
    countryName                False
    province_confirmedCount    False
    province_curedCount        False
    province_deadCount         False
    dtype: bool
    title      False
    summary    False
    dtype: bool
    


```python
import matplotlib.pyplot as plt
import matplotlib
import os

%matplotlib inline
# 指定中文字体
fpath = os.path.join(r"C:\Users\崔梦茹\Documents\作业\数据技术分析技术作业/NotoSansCJK.otf")
myfont = matplotlib.font_manager.FontProperties(fname=fpath)
# 绘图
data_world = data_world.sort_values(by='确诊人数', ascending=False)  # 按确诊人数进行排序
data_world_set = data_world[['确诊人数', '治愈人数', '死亡人数']]
data_world_set.index = data_world['国家名称']
data_world_set.head(10).plot(kind='bar', figsize=(15, 10))  # 对排序前十的国家数据进行绘图
plt.xlabel('国家名称', fontproperties=myfont)
plt.xticks(fontproperties=myfont)
plt.legend(fontsize=30, prop=myfont)  # 设置图例
```




    <matplotlib.legend.Legend at 0x1c55be085b0>




    
![png](output_7_1.png)
    



```python
from pyecharts.charts import Map
from pyecharts import options as opts
from pyecharts.globals import CurrentConfig, NotebookType

CurrentConfig.NOTEBOOK_TYPE = NotebookType.JUPYTER_NOTEBOOK
name_map = {  # 世界各国数据的中英文对比
    'Singapore Rep.': '新加坡',
    'Dominican Rep.': '多米尼加',
    'Palestine': '巴勒斯坦',
    'Bahamas': '巴哈马',
    'Timor-Leste': '东帝汶',
    'Afghanistan': '阿富汗',
    'Guinea-Bissau': '几内亚比绍',
    "Côte d'Ivoire": '科特迪瓦',
    'Siachen Glacier': '锡亚琴冰川',
    "Br. Indian Ocean Ter.": '英属印度洋领土',
    'Angola': '安哥拉',
    'Albania': '阿尔巴尼亚',
    'United Arab Emirates': '阿联酋',
    'Argentina': '阿根廷',
    'Armenia': '亚美尼亚',
    'French Southern and Antarctic Lands': '法属南半球和南极领地',
    'Australia': '澳大利亚',
    'Austria': '奥地利',
    'Azerbaijan': '阿塞拜疆',
    'Burundi': '布隆迪',
    'Belgium': '比利时',
    'Benin': '贝宁',
    'Burkina Faso': '布基纳法索',
    'Bangladesh': '孟加拉国',
    'Bulgaria': '保加利亚',
    'The Bahamas': '巴哈马',
    'Bosnia and Herz.': '波斯尼亚和黑塞哥维那',
    'Belarus': '白俄罗斯',
    'Belize': '伯利兹',
    'Bermuda': '百慕大',
    'Bolivia': '玻利维亚',
    'Brazil': '巴西',
    'Brunei': '文莱',
    'Bhutan': '不丹',
    'Botswana': '博茨瓦纳',
    'Central African Rep.': '中非',
    'Canada': '加拿大',
    'Switzerland': '瑞士',
    'Chile': '智利',
    'China': '中国',
    'Ivory Coast': '象牙海岸',
    'Cameroon': '喀麦隆',
    'Dem. Rep. Congo': '刚果民主共和国',
    'Congo': '刚果',
    'Colombia': '哥伦比亚',
    'Costa Rica': '哥斯达黎加',
    'Cuba': '古巴',
    'N. Cyprus': '北塞浦路斯',
    'Cyprus': '塞浦路斯',
    'Czech Rep.': '捷克',
    'Germany': '德国',
    'Djibouti': '吉布提',
    'Denmark': '丹麦',
    'Algeria': '阿尔及利亚',
    'Ecuador': '厄瓜多尔',
    'Egypt': '埃及',
    'Eritrea': '厄立特里亚',
    'Spain': '西班牙',
    'Estonia': '爱沙尼亚',
    'Ethiopia': '埃塞俄比亚',
    'Finland': '芬兰',
    'Fiji': '斐',
    'Falkland Islands': '福克兰群岛',
    'France': '法国',
    'Gabon': '加蓬',
    'United Kingdom': '英国',
    'Georgia': '格鲁吉亚',
    'Ghana': '加纳',
    'Guinea': '几内亚',
    'Gambia': '冈比亚',
    'Guinea Bissau': '几内亚比绍',
    'Eq. Guinea': '赤道几内亚',
    'Greece': '希腊',
    'Greenland': '格陵兰',
    'Guatemala': '危地马拉',
    'French Guiana': '法属圭亚那',
    'Guyana': '圭亚那',
    'Honduras': '洪都拉斯',
    'Croatia': '克罗地亚',
    'Haiti': '海地',
    'Hungary': '匈牙利',
    'Indonesia': '印度尼西亚',
    'India': '印度',
    'Ireland': '爱尔兰',
    'Iran': '伊朗',
    'Iraq': '伊拉克',
    'Iceland': '冰岛',
    'Israel': '以色列',
    'Italy': '意大利',
    'Jamaica': '牙买加',
    'Jordan': '约旦',
    'Japan': '日本',
    'Kazakhstan': '哈萨克斯坦',
    'Kenya': '肯尼亚',
    'Kyrgyzstan': '吉尔吉斯斯坦',
    'Cambodia': '柬埔寨',
    'Korea': '韩国',
    'Kosovo': '科索沃',
    'Kuwait': '科威特',
    'Lao PDR': '老挝',
    'Lebanon': '黎巴嫩',
    'Liberia': '利比里亚',
    'Libya': '利比亚',
    'Sri Lanka': '斯里兰卡',
    'Lesotho': '莱索托',
    'Lithuania': '立陶宛',
    'Luxembourg': '卢森堡',
    'Latvia': '拉脱维亚',
    'Morocco': '摩洛哥',
    'Moldova': '摩尔多瓦',
    'Madagascar': '马达加斯加',
    'Mexico': '墨西哥',
    'Macedonia': '马其顿',
    'Mali': '马里',
    'Myanmar': '缅甸',
    'Montenegro': '黑山',
    'Mongolia': '蒙古',
    'Mozambique': '莫桑比克',
    'Mauritania': '毛里塔尼亚',
    'Malawi': '马拉维',
    'Malaysia': '马来西亚',
    'Namibia': '纳米比亚',
    'New Caledonia': '新喀里多尼亚',
    'Niger': '尼日尔',
    'Nigeria': '尼日利亚',
    'Nicaragua': '尼加拉瓜',
    'Netherlands': '荷兰',
    'Norway': '挪威',
    'Nepal': '尼泊尔',
    'New Zealand': '新西兰',
    'Oman': '阿曼',
    'Pakistan': '巴基斯坦',
    'Panama': '巴拿马',
    'Peru': '秘鲁',
    'Philippines': '菲律宾',
    'Papua New Guinea': '巴布亚新几内亚',
    'Poland': '波兰',
    'Puerto Rico': '波多黎各',
    'Dem. Rep. Korea': '朝鲜',
    'Portugal': '葡萄牙',
    'Paraguay': '巴拉圭',
    'Qatar': '卡塔尔',
    'Romania': '罗马尼亚',
    'Russia': '俄罗斯',
    'Rwanda': '卢旺达',
    'W. Sahara': '西撒哈拉',
    'Saudi Arabia': '沙特阿拉伯',
    'Sudan': '苏丹',
    'S. Sudan': '南苏丹',
    'Senegal': '塞内加尔',
    'Solomon Is.': '所罗门群岛',
    'Sierra Leone': '塞拉利昂',
    'El Salvador': '萨尔瓦多',
    'Somaliland': '索马里兰',
    'Somalia': '索马里',
    'Serbia': '塞尔维亚',
    'Suriname': '苏里南',
    'Slovakia': '斯洛伐克',
    'Slovenia': '斯洛文尼亚',
    'Sweden': '瑞典',
    'Swaziland': '斯威士兰',
    'Syria': '叙利亚',
    'Chad': '乍得',
    'Togo': '多哥',
    'Thailand': '泰国',
    'Tajikistan': '塔吉克斯坦',
    'Turkmenistan': '土库曼斯坦',
    'East Timor': '东帝汶',
    'Trinidad and Tobago': '特里尼达和多巴哥',
    'Tunisia': '突尼斯',
    'Turkey': '土耳其',
    'Tanzania': '坦桑尼亚',
    'Uganda': '乌干达',
    'Ukraine': '乌克兰',
    'Uruguay': '乌拉圭',
    'United States': '美国',
    'Uzbekistan': '乌兹别克斯坦',
    'Venezuela': '委内瑞拉',
    'Vietnam': '越南',
    'Vanuatu': '瓦努阿图',
    'West Bank': '西岸',
    'Yemen': '也门',
    'South Africa': '南非',
    'Zambia': '赞比亚',
    'Zimbabwe': '津巴布韦',
    'Comoros': '科摩罗'
}

map = Map(init_opts=opts.InitOpts(width="1900px", height="900px",
                                  bg_color="#ADD8E6", page_title="全球疫情确诊人数"))  # 获得世界地图数据
map.add("确诊人数", [list(z) for z in zip(data_world['国家名称'], data_world['确诊人数'])],
        is_map_symbol_show=False,  # 添加确诊人数信息
        # 通过name_map来转化国家的中英文名称方便显示
        maptype="world", label_opts=opts.LabelOpts(is_show=False), name_map=name_map,
        itemstyle_opts=opts.ItemStyleOpts(color="rgb(49,60,72)"),
        ).set_global_opts(
    visualmap_opts=opts.VisualMapOpts(max_=1000000),  # 对视觉映射进行配置
)
map.render_notebook()  # 在notebook中显示
```





<script>
    require.config({
        paths: {
            'echarts':'https://assets.pyecharts.org/assets/echarts.min', 'world':'https://assets.pyecharts.org/assets/maps/world'
        }
    });
</script>

        <div id="eda3dcc3d0564ebe825459d694d37909" style="width:1900px; height:900px;"></div>

<script>
        require(['echarts', 'world'], function(echarts) {
                var chart_eda3dcc3d0564ebe825459d694d37909 = echarts.init(
                    document.getElementById('eda3dcc3d0564ebe825459d694d37909'), 'white', {renderer: 'canvas'});
                var option_eda3dcc3d0564ebe825459d694d37909 = {
    "backgroundColor": "#ADD8E6",
    "animation": true,
    "animationThreshold": 2000,
    "animationDuration": 1000,
    "animationEasing": "cubicOut",
    "animationDelay": 0,
    "animationDurationUpdate": 300,
    "animationEasingUpdate": "cubicOut",
    "animationDelayUpdate": 0,
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597"
    ],
    "series": [
        {
            "type": "map",
            "name": "\u786e\u8bca\u4eba\u6570",
            "label": {
                "show": false,
                "position": "top",
                "margin": 8
            },
            "mapType": "world",
            "data": [
                {
                    "name": "\u7f8e\u56fd",
                    "value": 80632301
                },
                {
                    "name": "\u5370\u5ea6",
                    "value": 43040947
                },
                {
                    "name": "\u5df4\u897f",
                    "value": 30247302
                },
                {
                    "name": "\u6cd5\u56fd",
                    "value": 27626578
                },
                {
                    "name": "\u5fb7\u56fd",
                    "value": 23376879
                },
                {
                    "name": "\u82f1\u56fd",
                    "value": 21819851
                },
                {
                    "name": "\u4fc4\u7f57\u65af",
                    "value": 18041927
                },
                {
                    "name": "\u97e9\u56fd",
                    "value": 16212751
                },
                {
                    "name": "\u610f\u5927\u5229",
                    "value": 15595302
                },
                {
                    "name": "\u571f\u8033\u5176",
                    "value": 14987583
                },
                {
                    "name": "\u897f\u73ed\u7259",
                    "value": 11662214
                },
                {
                    "name": "\u8d8a\u5357",
                    "value": 10394533
                },
                {
                    "name": "\u963f\u6839\u5ef7",
                    "value": 9059944
                },
                {
                    "name": "\u8377\u5170",
                    "value": 8109600
                },
                {
                    "name": "\u65e5\u672c",
                    "value": 7284737
                },
                {
                    "name": "\u4f0a\u6717",
                    "value": 7204049
                },
                {
                    "name": "\u54e5\u4f26\u6bd4\u4e9a",
                    "value": 6089381
                },
                {
                    "name": "\u5370\u5ea6\u5c3c\u897f\u4e9a",
                    "value": 6038664
                },
                {
                    "name": "\u6ce2\u5170",
                    "value": 5983864
                },
                {
                    "name": "\u58a8\u897f\u54e5",
                    "value": 5725476
                },
                {
                    "name": "\u6fb3\u5927\u5229\u4e9a",
                    "value": 5345438
                },
                {
                    "name": "\u4e4c\u514b\u5170",
                    "value": 5040518
                },
                {
                    "name": "\u9a6c\u6765\u897f\u4e9a",
                    "value": 4372697
                },
                {
                    "name": "\u5965\u5730\u5229",
                    "value": 4036813
                },
                {
                    "name": "\u4ee5\u8272\u5217",
                    "value": 4025943
                },
                {
                    "name": "\u6cf0\u56fd",
                    "value": 4012184
                },
                {
                    "name": "\u6bd4\u5229\u65f6",
                    "value": 3972963
                },
                {
                    "name": "\u6377\u514b",
                    "value": 3880556
                },
                {
                    "name": "\u8461\u8404\u7259",
                    "value": 3719485
                },
                {
                    "name": "\u5357\u975e",
                    "value": 3717067
                },
                {
                    "name": "\u83f2\u5f8b\u5bbe",
                    "value": 3677924
                },
                {
                    "name": "\u52a0\u62ff\u5927",
                    "value": 3620217
                },
                {
                    "name": "\u745e\u58eb",
                    "value": 3568616
                },
                {
                    "name": "\u79d8\u9c81",
                    "value": 3554006
                },
                {
                    "name": "\u667a\u5229",
                    "value": 3525845
                },
                {
                    "name": "\u5e0c\u814a",
                    "value": 3224479
                },
                {
                    "name": "\u4e39\u9ea6",
                    "value": 3095605
                },
                {
                    "name": "\u7f57\u9a6c\u5c3c\u4e9a",
                    "value": 2880276
                },
                {
                    "name": "\u65af\u6d1b\u4f10\u514b",
                    "value": 2504931
                },
                {
                    "name": "\u745e\u5178",
                    "value": 2495996
                },
                {
                    "name": "\u4f0a\u62c9\u514b",
                    "value": 2322938
                },
                {
                    "name": "\u585e\u5c14\u7ef4\u4e9a",
                    "value": 2222448
                },
                {
                    "name": "\u5b5f\u52a0\u62c9\u56fd",
                    "value": 1952224
                },
                {
                    "name": "\u5308\u7259\u5229",
                    "value": 1879480
                },
                {
                    "name": "\u7ea6\u65e6",
                    "value": 1694957
                },
                {
                    "name": "\u683c\u9c81\u5409\u4e9a",
                    "value": 1652707
                },
                {
                    "name": "\u5df4\u57fa\u65af\u5766",
                    "value": 1527151
                },
                {
                    "name": "\u7231\u5c14\u5170",
                    "value": 1496910
                },
                {
                    "name": "\u632a\u5a01",
                    "value": 1419096
                },
                {
                    "name": "\u54c8\u8428\u514b\u65af\u5766",
                    "value": 1394143
                },
                {
                    "name": "\u6469\u6d1b\u54e5",
                    "value": 1164296
                },
                {
                    "name": "\u65b0\u52a0\u5761",
                    "value": 1155581
                },
                {
                    "name": "\u4fdd\u52a0\u5229\u4e9a",
                    "value": 1148971
                },
                {
                    "name": "\u514b\u7f57\u5730\u4e9a",
                    "value": 1112491
                },
                {
                    "name": "\u53e4\u5df4",
                    "value": 1099042
                },
                {
                    "name": "\u9ece\u5df4\u5ae9",
                    "value": 1095406
                },
                {
                    "name": "\u7acb\u9676\u5b9b",
                    "value": 1047563
                },
                {
                    "name": "\u7a81\u5c3c\u65af",
                    "value": 1038668
                },
                {
                    "name": "\u65af\u6d1b\u6587\u5c3c\u4e9a",
                    "value": 995515
                },
                {
                    "name": "\u5c3c\u6cca\u5c14",
                    "value": 978648
                },
                {
                    "name": "\u767d\u4fc4\u7f57\u65af",
                    "value": 973206
                },
                {
                    "name": "\u82ac\u5170",
                    "value": 949583
                },
                {
                    "name": "\u8499\u53e4",
                    "value": 920119
                },
                {
                    "name": "\u73bb\u5229\u7ef4\u4e9a",
                    "value": 903776
                },
                {
                    "name": "\u4e4c\u62c9\u572d",
                    "value": 895451
                },
                {
                    "name": "\u963f\u8054\u914b",
                    "value": 895018
                },
                {
                    "name": "\u5384\u74dc\u591a\u5c14",
                    "value": 865585
                },
                {
                    "name": "\u54e5\u65af\u8fbe\u9ece\u52a0",
                    "value": 844892
                },
                {
                    "name": "\u5371\u5730\u9a6c\u62c9",
                    "value": 837377
                },
                {
                    "name": "\u65b0\u897f\u5170",
                    "value": 818882
                },
                {
                    "name": "\u62c9\u8131\u7ef4\u4e9a",
                    "value": 812686
                },
                {
                    "name": "\u963f\u585e\u62dc\u7586",
                    "value": 792320
                },
                {
                    "name": "\u5df4\u62ff\u9a6c",
                    "value": 768470
                },
                {
                    "name": "\u6c99\u7279\u963f\u62c9\u4f2f",
                    "value": 752396
                },
                {
                    "name": "\u65af\u91cc\u5170\u5361",
                    "value": 662794
                },
                {
                    "name": "\u5df4\u52d2\u65af\u5766",
                    "value": 656617
                },
                {
                    "name": "\u5df4\u62c9\u572d",
                    "value": 648446
                },
                {
                    "name": "\u79d1\u5a01\u7279",
                    "value": 630641
                },
                {
                    "name": "\u7f05\u7538",
                    "value": 612527
                },
                {
                    "name": "\u591a\u7c73\u5c3c\u52a0",
                    "value": 578663
                },
                {
                    "name": "\u7231\u6c99\u5c3c\u4e9a",
                    "value": 566923
                },
                {
                    "name": "\u5df4\u6797",
                    "value": 562399
                },
                {
                    "name": "\u4e2d\u56fd",
                    "value": 529071
                },
                {
                    "name": "\u59d4\u5185\u745e\u62c9",
                    "value": 521946
                },
                {
                    "name": "\u6469\u5c14\u591a\u74e6",
                    "value": 516164
                },
                {
                    "name": "\u57c3\u53ca",
                    "value": 511977
                },
                {
                    "name": "\u5229\u6bd4\u4e9a",
                    "value": 501834
                },
                {
                    "name": "\u6ce2\u591a\u9ece\u5404",
                    "value": 489219
                },
                {
                    "name": "\u57c3\u585e\u4fc4\u6bd4\u4e9a",
                    "value": 470232
                },
                {
                    "name": "\u585e\u6d66\u8def\u65af",
                    "value": 464366
                },
                {
                    "name": "\u4e9a\u7f8e\u5c3c\u4e9a",
                    "value": 422729
                },
                {
                    "name": "\u6d2a\u90fd\u62c9\u65af",
                    "value": 421268
                },
                {
                    "name": "\u963f\u66fc",
                    "value": 388795
                },
                {
                    "name": "\u6ce2\u9ed1",
                    "value": 376437
                },
                {
                    "name": "\u5361\u5854\u5c14",
                    "value": 363345
                },
                {
                    "name": "\u7559\u5c3c\u65fa",
                    "value": 360445
                },
                {
                    "name": "\u80af\u5c3c\u4e9a",
                    "value": 323605
                },
                {
                    "name": "\u8d5e\u6bd4\u4e9a\u5171\u548c\u56fd",
                    "value": 318404
                },
                {
                    "name": "\u5317\u9a6c\u5176\u987f",
                    "value": 308390
                },
                {
                    "name": "\u535a\u8328\u74e6\u7eb3",
                    "value": 305859
                },
                {
                    "name": "\u963f\u5c14\u5df4\u5c3c\u4e9a",
                    "value": 274429
                },
                {
                    "name": "\u963f\u5c14\u53ca\u5229\u4e9a",
                    "value": 265738
                },
                {
                    "name": "\u5c3c\u65e5\u5229\u4e9a",
                    "value": 255663
                },
                {
                    "name": "\u6d25\u5df4\u5e03\u97e6",
                    "value": 247237
                },
                {
                    "name": "\u4e4c\u5179\u522b\u514b\u65af\u5766",
                    "value": 238214
                },
                {
                    "name": "\u9ed1\u5c71",
                    "value": 234051
                },
                {
                    "name": "\u5362\u68ee\u5821",
                    "value": 229311
                },
                {
                    "name": "\u83ab\u6851\u6bd4\u514b",
                    "value": 225316
                },
                {
                    "name": "\u6bdb\u91cc\u6c42\u65af",
                    "value": 218229
                },
                {
                    "name": "\u5409\u5c14\u5409\u65af\u65af\u5766",
                    "value": 200980
                },
                {
                    "name": "\u8001\u631d",
                    "value": 199434
                },
                {
                    "name": "\u51b0\u5c9b",
                    "value": 183974
                },
                {
                    "name": "\u963f\u5bcc\u6c57",
                    "value": 178373
                },
                {
                    "name": "\u9a6c\u5c14\u4ee3\u592b",
                    "value": 178320
                },
                {
                    "name": "\u4e4c\u5e72\u8fbe",
                    "value": 164042
                },
                {
                    "name": "\u8428\u5c14\u74e6\u591a",
                    "value": 162089
                },
                {
                    "name": "\u52a0\u7eb3",
                    "value": 161101
                },
                {
                    "name": "\u7eb3\u7c73\u6bd4\u4e9a",
                    "value": 158001
                },
                {
                    "name": "\u9a6c\u63d0\u5c3c\u514b",
                    "value": 148332
                },
                {
                    "name": "\u74dc\u5fb7\u7f57\u666e\u5c9b",
                    "value": 142573
                },
                {
                    "name": "\u7279\u7acb\u5c3c\u8fbe\u548c\u591a\u5df4\u54e5",
                    "value": 141853
                },
                {
                    "name": "\u6587\u83b1",
                    "value": 139637
                },
                {
                    "name": "\u67ec\u57d4\u5be8",
                    "value": 136032
                },
                {
                    "name": "\u5362\u65fa\u8fbe",
                    "value": 129760
                },
                {
                    "name": "\u7259\u4e70\u52a0",
                    "value": 129168
                },
                {
                    "name": "\u5580\u9ea6\u9686",
                    "value": 119780
                },
                {
                    "name": "\u5b89\u54e5\u62c9",
                    "value": 99194
                },
                {
                    "name": "\u9a6c\u8033\u4ed6",
                    "value": 88244
                },
                {
                    "name": "\u521a\u679c\uff08\u91d1\uff09",
                    "value": 87023
                },
                {
                    "name": "\u585e\u5185\u52a0\u5c14",
                    "value": 85965
                },
                {
                    "name": "\u9a6c\u62c9\u7ef4",
                    "value": 85723
                },
                {
                    "name": "\u79d1\u7279\u8fea\u74e6",
                    "value": 85518
                },
                {
                    "name": "\u6cd5\u5c5e\u572d\u4e9a\u90a3",
                    "value": 79529
                },
                {
                    "name": "\u82cf\u91cc\u5357",
                    "value": 79276
                },
                {
                    "name": "\u6cd5\u5c5e\u6ce2\u5229\u5c3c\u897f\u4e9a",
                    "value": 72596
                },
                {
                    "name": "\u65af\u5a01\u58eb\u5170",
                    "value": 70048
                },
                {
                    "name": "\u6590\u6d4e",
                    "value": 64509
                },
                {
                    "name": "\u5df4\u5df4\u591a\u65af",
                    "value": 64177
                },
                {
                    "name": "\u9a6c\u8fbe\u52a0\u65af\u52a0",
                    "value": 64089
                },
                {
                    "name": "\u572d\u4e9a\u90a3",
                    "value": 63364
                },
                {
                    "name": "\u82cf\u4e39",
                    "value": 62057
                },
                {
                    "name": "\u65b0\u5580\u91cc\u591a\u5c3c\u4e9a",
                    "value": 60521
                },
                {
                    "name": "\u6bdb\u91cc\u5854\u5c3c\u4e9a",
                    "value": 58710
                },
                {
                    "name": "\u4f2f\u5229\u5179",
                    "value": 57331
                },
                {
                    "name": "\u4f5b\u5f97\u89d2",
                    "value": 55978
                },
                {
                    "name": "\u53d9\u5229\u4e9a",
                    "value": 55765
                },
                {
                    "name": "\u4e0d\u4e39",
                    "value": 50433
                },
                {
                    "name": "\u6cfd\u897f\u5c9b",
                    "value": 48529
                },
                {
                    "name": "\u52a0\u84ec",
                    "value": 47591
                },
                {
                    "name": "\u5df4\u5e03\u4e9a\u65b0\u51e0\u5185\u4e9a",
                    "value": 43660
                },
                {
                    "name": "\u585e\u820c\u5c14",
                    "value": 41660
                },
                {
                    "name": "\u5e93\u62c9\u7d22\u5c9b",
                    "value": 41306
                },
                {
                    "name": "\u5b89\u9053\u5c14",
                    "value": 40709
                },
                {
                    "name": "\u5173\u5c9b",
                    "value": 40053
                },
                {
                    "name": "\u5e03\u9686\u8fea\u5171\u548c\u56fd",
                    "value": 38722
                },
                {
                    "name": "\u591a\u54e5",
                    "value": 36962
                },
                {
                    "name": "\u9a6c\u7ea6\u7279",
                    "value": 36958
                },
                {
                    "name": "\u51e0\u5185\u4e9a",
                    "value": 36502
                },
                {
                    "name": "\u6cd5\u7f57\u7fa4\u5c9b",
                    "value": 34658
                },
                {
                    "name": "\u963f\u9c81\u5df4",
                    "value": 34345
                },
                {
                    "name": "\u5766\u6851\u5c3c\u4e9a",
                    "value": 33851
                },
                {
                    "name": "\u5df4\u54c8\u9a6c",
                    "value": 33388
                },
                {
                    "name": "\u83b1\u7d22\u6258",
                    "value": 32968
                },
                {
                    "name": "\u9a6c\u91cc",
                    "value": 30633
                },
                {
                    "name": "\u6d77\u5730",
                    "value": 30594
                },
                {
                    "name": "\u9a6c\u6069\u5c9b",
                    "value": 30169
                },
                {
                    "name": "\u8d1d\u5b81",
                    "value": 26952
                },
                {
                    "name": "\u7d22\u9a6c\u91cc",
                    "value": 26471
                },
                {
                    "name": "\u521a\u679c\uff08\u5e03\uff09",
                    "value": 24079
                },
                {
                    "name": "\u6839\u897f\u5c9b",
                    "value": 23684
                },
                {
                    "name": "\u5723\u5362\u897f\u4e9a",
                    "value": 23050
                },
                {
                    "name": "\u4e1c\u5e1d\u6c76",
                    "value": 22851
                },
                {
                    "name": "\u5f00\u66fc\u7fa4\u5c9b",
                    "value": 21406
                },
                {
                    "name": "\u5e03\u57fa\u7eb3\u6cd5\u7d22",
                    "value": 20865
                },
                {
                    "name": "\u5c3c\u52a0\u62c9\u74dc",
                    "value": 18491
                },
                {
                    "name": "\u5854\u5409\u514b\u65af\u5766",
                    "value": 17786
                },
                {
                    "name": "\u76f4\u5e03\u7f57\u9640",
                    "value": 17494
                },
                {
                    "name": "\u5357\u82cf\u4e39",
                    "value": 17369
                },
                {
                    "name": "\u5217\u652f\u6566\u58eb\u767b",
                    "value": 16946
                },
                {
                    "name": "\u8d64\u9053\u51e0\u5185\u4e9a",
                    "value": 16001
                },
                {
                    "name": "\u5723\u9a6c\u529b\u8bfa",
                    "value": 15874
                },
                {
                    "name": "\u7f8e\u5c5e\u7ef4\u5c14\u4eac\u7fa4\u5c9b",
                    "value": 15836
                },
                {
                    "name": "\u5409\u5e03\u63d0",
                    "value": 15598
                },
                {
                    "name": "\u4e2d\u975e\u5171\u548c\u56fd",
                    "value": 14649
                },
                {
                    "name": "\u683c\u6797\u90a3\u8fbe",
                    "value": 14165
                },
                {
                    "name": "\u767e\u6155\u5927",
                    "value": 12901
                },
                {
                    "name": "\u5188\u6bd4\u4e9a",
                    "value": 11994
                },
                {
                    "name": "\u591a\u7c73\u5c3c\u514b",
                    "value": 11988
                },
                {
                    "name": "\u683c\u9675\u5170",
                    "value": 11971
                },
                {
                    "name": "\u4e5f\u95e8\u5171\u548c\u56fd",
                    "value": 11815
                },
                {
                    "name": "\u6469\u7eb3\u54e5",
                    "value": 11341
                },
                {
                    "name": "\u5317\u9a6c\u91cc\u4e9a\u7eb3\u7fa4\u5c9b\u8054\u90a6",
                    "value": 11203
                },
                {
                    "name": "\u5723\u9a6c\u4e01\u5c9b",
                    "value": 10129
                },
                {
                    "name": "\u8377\u5c5e\u5723\u9a6c\u4e01",
                    "value": 9873
                },
                {
                    "name": "\u5384\u7acb\u7279\u91cc\u4e9a",
                    "value": 9733
                },
                {
                    "name": "\u5723\u6587\u68ee\u7279\u548c\u683c\u6797\u7eb3\u4e01\u65af",
                    "value": 9447
                },
                {
                    "name": "\u5c3c\u65e5\u5c14",
                    "value": 8866
                },
                {
                    "name": "\u51e0\u5185\u4e9a\u6bd4\u7ecd",
                    "value": 8176
                },
                {
                    "name": "\u79d1\u6469\u7f57",
                    "value": 8100
                },
                {
                    "name": "\u585e\u62c9\u5229\u6602",
                    "value": 7681
                },
                {
                    "name": "\u5b89\u63d0\u74dc\u548c\u5df4\u5e03\u8fbe",
                    "value": 7535
                },
                {
                    "name": "\u5229\u6bd4\u91cc\u4e9a",
                    "value": 7402
                },
                {
                    "name": "\u4e4d\u5f97",
                    "value": 7378
                },
                {
                    "name": "\u82f1\u5c5e\u7ef4\u5c14\u4eac\u7fa4\u5c9b",
                    "value": 6162
                },
                {
                    "name": "\u5723\u591a\u7f8e\u548c\u666e\u6797\u897f\u6bd4",
                    "value": 5948
                },
                {
                    "name": "\u7279\u514b\u65af\u548c\u51ef\u79d1\u65af\u7fa4\u5c9b",
                    "value": 5936
                },
                {
                    "name": "\u5723\u5176\u8328\u548c\u5c3c\u7ef4\u65af",
                    "value": 5555
                },
                {
                    "name": "\u5723\u5df4\u6cf0\u52d2\u7c73\u5c9b",
                    "value": 4233
                },
                {
                    "name": "\u5b89\u572d\u62c9",
                    "value": 2731
                },
                {
                    "name": "\u5723\u76ae\u57c3\u5c14\u548c\u5bc6\u514b\u9686\u7fa4\u5c9b",
                    "value": 2537
                },
                {
                    "name": "\u94bb\u77f3\u516c\u4e3b\u53f7\u90ae\u8f6e",
                    "value": 741
                },
                {
                    "name": "\u8377\u5170\u52a0\u52d2\u6bd4\u5730\u533a",
                    "value": 177
                },
                {
                    "name": "\u8499\u7279\u585e\u62c9\u7279",
                    "value": 176
                },
                {
                    "name": "\u798f\u514b\u5170\u7fa4\u5c9b",
                    "value": 131
                },
                {
                    "name": "\u68b5\u8482\u5188",
                    "value": 29
                }
            ],
            "roam": true,
            "zoom": 1,
            "nameMap": {
                "Singapore Rep.": "\u65b0\u52a0\u5761",
                "Dominican Rep.": "\u591a\u7c73\u5c3c\u52a0",
                "Palestine": "\u5df4\u52d2\u65af\u5766",
                "Bahamas": "\u5df4\u54c8\u9a6c",
                "Timor-Leste": "\u4e1c\u5e1d\u6c76",
                "Afghanistan": "\u963f\u5bcc\u6c57",
                "Guinea-Bissau": "\u51e0\u5185\u4e9a\u6bd4\u7ecd",
                "C\u00f4te d'Ivoire": "\u79d1\u7279\u8fea\u74e6",
                "Siachen Glacier": "\u9521\u4e9a\u7434\u51b0\u5ddd",
                "Br. Indian Ocean Ter.": "\u82f1\u5c5e\u5370\u5ea6\u6d0b\u9886\u571f",
                "Angola": "\u5b89\u54e5\u62c9",
                "Albania": "\u963f\u5c14\u5df4\u5c3c\u4e9a",
                "United Arab Emirates": "\u963f\u8054\u914b",
                "Argentina": "\u963f\u6839\u5ef7",
                "Armenia": "\u4e9a\u7f8e\u5c3c\u4e9a",
                "French Southern and Antarctic Lands": "\u6cd5\u5c5e\u5357\u534a\u7403\u548c\u5357\u6781\u9886\u5730",
                "Australia": "\u6fb3\u5927\u5229\u4e9a",
                "Austria": "\u5965\u5730\u5229",
                "Azerbaijan": "\u963f\u585e\u62dc\u7586",
                "Burundi": "\u5e03\u9686\u8fea",
                "Belgium": "\u6bd4\u5229\u65f6",
                "Benin": "\u8d1d\u5b81",
                "Burkina Faso": "\u5e03\u57fa\u7eb3\u6cd5\u7d22",
                "Bangladesh": "\u5b5f\u52a0\u62c9\u56fd",
                "Bulgaria": "\u4fdd\u52a0\u5229\u4e9a",
                "The Bahamas": "\u5df4\u54c8\u9a6c",
                "Bosnia and Herz.": "\u6ce2\u65af\u5c3c\u4e9a\u548c\u9ed1\u585e\u54e5\u7ef4\u90a3",
                "Belarus": "\u767d\u4fc4\u7f57\u65af",
                "Belize": "\u4f2f\u5229\u5179",
                "Bermuda": "\u767e\u6155\u5927",
                "Bolivia": "\u73bb\u5229\u7ef4\u4e9a",
                "Brazil": "\u5df4\u897f",
                "Brunei": "\u6587\u83b1",
                "Bhutan": "\u4e0d\u4e39",
                "Botswana": "\u535a\u8328\u74e6\u7eb3",
                "Central African Rep.": "\u4e2d\u975e",
                "Canada": "\u52a0\u62ff\u5927",
                "Switzerland": "\u745e\u58eb",
                "Chile": "\u667a\u5229",
                "China": "\u4e2d\u56fd",
                "Ivory Coast": "\u8c61\u7259\u6d77\u5cb8",
                "Cameroon": "\u5580\u9ea6\u9686",
                "Dem. Rep. Congo": "\u521a\u679c\u6c11\u4e3b\u5171\u548c\u56fd",
                "Congo": "\u521a\u679c",
                "Colombia": "\u54e5\u4f26\u6bd4\u4e9a",
                "Costa Rica": "\u54e5\u65af\u8fbe\u9ece\u52a0",
                "Cuba": "\u53e4\u5df4",
                "N. Cyprus": "\u5317\u585e\u6d66\u8def\u65af",
                "Cyprus": "\u585e\u6d66\u8def\u65af",
                "Czech Rep.": "\u6377\u514b",
                "Germany": "\u5fb7\u56fd",
                "Djibouti": "\u5409\u5e03\u63d0",
                "Denmark": "\u4e39\u9ea6",
                "Algeria": "\u963f\u5c14\u53ca\u5229\u4e9a",
                "Ecuador": "\u5384\u74dc\u591a\u5c14",
                "Egypt": "\u57c3\u53ca",
                "Eritrea": "\u5384\u7acb\u7279\u91cc\u4e9a",
                "Spain": "\u897f\u73ed\u7259",
                "Estonia": "\u7231\u6c99\u5c3c\u4e9a",
                "Ethiopia": "\u57c3\u585e\u4fc4\u6bd4\u4e9a",
                "Finland": "\u82ac\u5170",
                "Fiji": "\u6590",
                "Falkland Islands": "\u798f\u514b\u5170\u7fa4\u5c9b",
                "France": "\u6cd5\u56fd",
                "Gabon": "\u52a0\u84ec",
                "United Kingdom": "\u82f1\u56fd",
                "Georgia": "\u683c\u9c81\u5409\u4e9a",
                "Ghana": "\u52a0\u7eb3",
                "Guinea": "\u51e0\u5185\u4e9a",
                "Gambia": "\u5188\u6bd4\u4e9a",
                "Guinea Bissau": "\u51e0\u5185\u4e9a\u6bd4\u7ecd",
                "Eq. Guinea": "\u8d64\u9053\u51e0\u5185\u4e9a",
                "Greece": "\u5e0c\u814a",
                "Greenland": "\u683c\u9675\u5170",
                "Guatemala": "\u5371\u5730\u9a6c\u62c9",
                "French Guiana": "\u6cd5\u5c5e\u572d\u4e9a\u90a3",
                "Guyana": "\u572d\u4e9a\u90a3",
                "Honduras": "\u6d2a\u90fd\u62c9\u65af",
                "Croatia": "\u514b\u7f57\u5730\u4e9a",
                "Haiti": "\u6d77\u5730",
                "Hungary": "\u5308\u7259\u5229",
                "Indonesia": "\u5370\u5ea6\u5c3c\u897f\u4e9a",
                "India": "\u5370\u5ea6",
                "Ireland": "\u7231\u5c14\u5170",
                "Iran": "\u4f0a\u6717",
                "Iraq": "\u4f0a\u62c9\u514b",
                "Iceland": "\u51b0\u5c9b",
                "Israel": "\u4ee5\u8272\u5217",
                "Italy": "\u610f\u5927\u5229",
                "Jamaica": "\u7259\u4e70\u52a0",
                "Jordan": "\u7ea6\u65e6",
                "Japan": "\u65e5\u672c",
                "Kazakhstan": "\u54c8\u8428\u514b\u65af\u5766",
                "Kenya": "\u80af\u5c3c\u4e9a",
                "Kyrgyzstan": "\u5409\u5c14\u5409\u65af\u65af\u5766",
                "Cambodia": "\u67ec\u57d4\u5be8",
                "Korea": "\u97e9\u56fd",
                "Kosovo": "\u79d1\u7d22\u6c83",
                "Kuwait": "\u79d1\u5a01\u7279",
                "Lao PDR": "\u8001\u631d",
                "Lebanon": "\u9ece\u5df4\u5ae9",
                "Liberia": "\u5229\u6bd4\u91cc\u4e9a",
                "Libya": "\u5229\u6bd4\u4e9a",
                "Sri Lanka": "\u65af\u91cc\u5170\u5361",
                "Lesotho": "\u83b1\u7d22\u6258",
                "Lithuania": "\u7acb\u9676\u5b9b",
                "Luxembourg": "\u5362\u68ee\u5821",
                "Latvia": "\u62c9\u8131\u7ef4\u4e9a",
                "Morocco": "\u6469\u6d1b\u54e5",
                "Moldova": "\u6469\u5c14\u591a\u74e6",
                "Madagascar": "\u9a6c\u8fbe\u52a0\u65af\u52a0",
                "Mexico": "\u58a8\u897f\u54e5",
                "Macedonia": "\u9a6c\u5176\u987f",
                "Mali": "\u9a6c\u91cc",
                "Myanmar": "\u7f05\u7538",
                "Montenegro": "\u9ed1\u5c71",
                "Mongolia": "\u8499\u53e4",
                "Mozambique": "\u83ab\u6851\u6bd4\u514b",
                "Mauritania": "\u6bdb\u91cc\u5854\u5c3c\u4e9a",
                "Malawi": "\u9a6c\u62c9\u7ef4",
                "Malaysia": "\u9a6c\u6765\u897f\u4e9a",
                "Namibia": "\u7eb3\u7c73\u6bd4\u4e9a",
                "New Caledonia": "\u65b0\u5580\u91cc\u591a\u5c3c\u4e9a",
                "Niger": "\u5c3c\u65e5\u5c14",
                "Nigeria": "\u5c3c\u65e5\u5229\u4e9a",
                "Nicaragua": "\u5c3c\u52a0\u62c9\u74dc",
                "Netherlands": "\u8377\u5170",
                "Norway": "\u632a\u5a01",
                "Nepal": "\u5c3c\u6cca\u5c14",
                "New Zealand": "\u65b0\u897f\u5170",
                "Oman": "\u963f\u66fc",
                "Pakistan": "\u5df4\u57fa\u65af\u5766",
                "Panama": "\u5df4\u62ff\u9a6c",
                "Peru": "\u79d8\u9c81",
                "Philippines": "\u83f2\u5f8b\u5bbe",
                "Papua New Guinea": "\u5df4\u5e03\u4e9a\u65b0\u51e0\u5185\u4e9a",
                "Poland": "\u6ce2\u5170",
                "Puerto Rico": "\u6ce2\u591a\u9ece\u5404",
                "Dem. Rep. Korea": "\u671d\u9c9c",
                "Portugal": "\u8461\u8404\u7259",
                "Paraguay": "\u5df4\u62c9\u572d",
                "Qatar": "\u5361\u5854\u5c14",
                "Romania": "\u7f57\u9a6c\u5c3c\u4e9a",
                "Russia": "\u4fc4\u7f57\u65af",
                "Rwanda": "\u5362\u65fa\u8fbe",
                "W. Sahara": "\u897f\u6492\u54c8\u62c9",
                "Saudi Arabia": "\u6c99\u7279\u963f\u62c9\u4f2f",
                "Sudan": "\u82cf\u4e39",
                "S. Sudan": "\u5357\u82cf\u4e39",
                "Senegal": "\u585e\u5185\u52a0\u5c14",
                "Solomon Is.": "\u6240\u7f57\u95e8\u7fa4\u5c9b",
                "Sierra Leone": "\u585e\u62c9\u5229\u6602",
                "El Salvador": "\u8428\u5c14\u74e6\u591a",
                "Somaliland": "\u7d22\u9a6c\u91cc\u5170",
                "Somalia": "\u7d22\u9a6c\u91cc",
                "Serbia": "\u585e\u5c14\u7ef4\u4e9a",
                "Suriname": "\u82cf\u91cc\u5357",
                "Slovakia": "\u65af\u6d1b\u4f10\u514b",
                "Slovenia": "\u65af\u6d1b\u6587\u5c3c\u4e9a",
                "Sweden": "\u745e\u5178",
                "Swaziland": "\u65af\u5a01\u58eb\u5170",
                "Syria": "\u53d9\u5229\u4e9a",
                "Chad": "\u4e4d\u5f97",
                "Togo": "\u591a\u54e5",
                "Thailand": "\u6cf0\u56fd",
                "Tajikistan": "\u5854\u5409\u514b\u65af\u5766",
                "Turkmenistan": "\u571f\u5e93\u66fc\u65af\u5766",
                "East Timor": "\u4e1c\u5e1d\u6c76",
                "Trinidad and Tobago": "\u7279\u91cc\u5c3c\u8fbe\u548c\u591a\u5df4\u54e5",
                "Tunisia": "\u7a81\u5c3c\u65af",
                "Turkey": "\u571f\u8033\u5176",
                "Tanzania": "\u5766\u6851\u5c3c\u4e9a",
                "Uganda": "\u4e4c\u5e72\u8fbe",
                "Ukraine": "\u4e4c\u514b\u5170",
                "Uruguay": "\u4e4c\u62c9\u572d",
                "United States": "\u7f8e\u56fd",
                "Uzbekistan": "\u4e4c\u5179\u522b\u514b\u65af\u5766",
                "Venezuela": "\u59d4\u5185\u745e\u62c9",
                "Vietnam": "\u8d8a\u5357",
                "Vanuatu": "\u74e6\u52aa\u963f\u56fe",
                "West Bank": "\u897f\u5cb8",
                "Yemen": "\u4e5f\u95e8",
                "South Africa": "\u5357\u975e",
                "Zambia": "\u8d5e\u6bd4\u4e9a",
                "Zimbabwe": "\u6d25\u5df4\u5e03\u97e6",
                "Comoros": "\u79d1\u6469\u7f57"
            },
            "showLegendSymbol": false,
            "itemStyle": {
                "color": "rgb(49,60,72)"
            },
            "emphasis": {}
        }
    ],
    "legend": [
        {
            "data": [
                "\u786e\u8bca\u4eba\u6570"
            ],
            "selected": {
                "\u786e\u8bca\u4eba\u6570": true
            },
            "show": true,
            "padding": 5,
            "itemGap": 10,
            "itemWidth": 25,
            "itemHeight": 14
        }
    ],
    "tooltip": {
        "show": true,
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "borderWidth": 0
    },
    "title": [
        {
            "padding": 5,
            "itemGap": 10
        }
    ],
    "visualMap": {
        "show": true,
        "type": "continuous",
        "min": 0,
        "max": 1000000,
        "inRange": {
            "color": [
                "#50a3ba",
                "#eac763",
                "#d94e5d"
            ]
        },
        "calculable": true,
        "inverse": false,
        "splitNumber": 5,
        "orient": "vertical",
        "showLabel": true,
        "itemWidth": 20,
        "itemHeight": 140,
        "borderWidth": 0
    }
};
                chart_eda3dcc3d0564ebe825459d694d37909.setOption(option_eda3dcc3d0564ebe825459d694d37909);
        });
    </script>





```python
country = data_area_times.sort_values('province_confirmedCount', ascending=False).drop_duplicates(
    subset='countryName', keep='first').head(6)['countryName']
country = list(country)  # 对于同一天采集的多个数据，只保留第一次出现的数据也就是最后一次更新的数据
country
```




    ['美国', '巴西', '印度', '俄罗斯', '秘鲁', '智利']




```python
data_America = data_area_times[data_area_times['countryName'] == '美国']
data_Brazil = data_area_times[data_area_times['countryName'] == '巴西']
data_India = data_area_times[data_area_times['countryName'] == '印度']
data_Russia = data_area_times[data_area_times['countryName'] == '俄罗斯']
data_Peru = data_area_times[data_area_times['countryName'] == '秘鲁']
data_Chile = data_area_times[data_area_times['countryName'] == '智利']

timeindex = data_area_times.index
timeindex = timeindex.floor('D')  # 对于日期索引，只保留具体到哪一天
data_area_times.index = timeindex

timeseries = pd.DataFrame(data_America.index)
timeseries.index = data_America.index
data_America = pd.concat([timeseries, data_America], axis=1)
data_America.drop_duplicates(
    subset='updateTime', keep='first', inplace=True)  # 对美国数据进行处理，获得美国确诊人数的时间序列
data_America.drop('updateTime', axis=1, inplace=True)

timeseries = pd.DataFrame(data_Brazil.index)
timeseries.index = data_Brazil.index
data_Brazil = pd.concat([timeseries, data_Brazil], axis=1)
# 对巴西数据进行处理，获得巴西确诊人数的时间序列
data_Brazil.drop_duplicates(subset='updateTime', keep='first', inplace=True)
data_Brazil.drop('updateTime', axis=1, inplace=True)

timeseries = pd.DataFrame(data_India.index)
timeseries.index = data_India.index
data_India = pd.concat([timeseries, data_India], axis=1)
# 对印度数据进行处理，获得印度确诊人数的时间序列
data_India.drop_duplicates(subset='updateTime', keep='first', inplace=True)
data_India.drop('updateTime', axis=1, inplace=True)

timeseries = pd.DataFrame(data_Russia.index)
timeseries.index = data_Russia.index
data_Russia = pd.concat([timeseries, data_Russia], axis=1)
# 对俄罗斯数据进行处理，获得俄罗斯确诊人数的时间序列
data_Russia.drop_duplicates(subset='updateTime', keep='first', inplace=True)
data_Russia.drop('updateTime', axis=1, inplace=True)

timeseries = pd.DataFrame(data_Peru.index)
timeseries.index = data_Peru.index
data_Peru = pd.concat([timeseries, data_Peru], axis=1)
# 对秘鲁数据进行处理，获得秘鲁确诊人数的时间序列
data_Peru.drop_duplicates(subset='updateTime', keep='first', inplace=True)
data_Peru.drop('updateTime', axis=1, inplace=True)

timeseries = pd.DataFrame(data_Chile.index)
timeseries.index = data_Chile.index
data_Chile = pd.concat([timeseries, data_Chile], axis=1)
# 对智利数据进行处理，获得智利确诊人数的时间序列
data_Chile.drop_duplicates(subset='updateTime', keep='first', inplace=True)
data_Chile.drop('updateTime', axis=1, inplace=True)

plt.title("世界疫情排行分布", fontproperties=myfont)
plt.plot(data_America['province_confirmedCount'])
plt.plot(data_Brazil['province_confirmedCount'])
plt.plot(data_India['province_confirmedCount'])
plt.plot(data_Russia['province_confirmedCount'])
plt.plot(data_Peru['province_confirmedCount'])
plt.plot(data_Chile['province_confirmedCount'])
plt.legend(country, prop=myfont)
```




    <matplotlib.legend.Legend at 0x1c56300bca0>




    
![png](output_10_1.png)
    



```python
pip install wordcloud
```

    Requirement already satisfied: wordcloud in c:\anaconda\lib\site-packages (1.8.1)
    Requirement already satisfied: numpy>=1.6.1 in c:\anaconda\lib\site-packages (from wordcloud) (1.20.3)
    Requirement already satisfied: pillow in c:\anaconda\lib\site-packages (from wordcloud) (8.4.0)
    Requirement already satisfied: matplotlib in c:\anaconda\lib\site-packages (from wordcloud) (3.4.3)
    Requirement already satisfied: cycler>=0.10 in c:\anaconda\lib\site-packages (from matplotlib->wordcloud) (0.10.0)
    Requirement already satisfied: kiwisolver>=1.0.1 in c:\anaconda\lib\site-packages (from matplotlib->wordcloud) (1.3.1)
    Requirement already satisfied: python-dateutil>=2.7 in c:\anaconda\lib\site-packages (from matplotlib->wordcloud) (2.8.2)
    Requirement already satisfied: pyparsing>=2.2.1 in c:\anaconda\lib\site-packages (from matplotlib->wordcloud) (3.0.4)
    Requirement already satisfied: six in c:\anaconda\lib\site-packages (from cycler>=0.10->matplotlib->wordcloud) (1.16.0)
    Note: you may need to restart the kernel to use updated packages.
    


```python
pip install jieba
```

    Requirement already satisfied: jieba in c:\anaconda\lib\site-packages (0.42.1)Note: you may need to restart the kernel to use updated packages.
    
    


```python
import jieba
import re
from wordcloud import WordCloud

def word_cut(x): return jieba.lcut(x)  # 进行结巴分词

news = []
reg = "[^\u4e00-\u9fa5]"
for i in data_news['title']:
    if re.sub(reg, '', i) != '':  # 去掉英文数字和标点等无关字符，仅保留中文词组
        news.append(re.sub(reg, '', i))  # 用news列表汇总处理后的新闻标题

words = []
counts = {}
for i in news:
    words.append(word_cut(i))  # 对所有新闻进行分词
for word in words:
    for a_word in word:
        if len(a_word) == 1:
            continue
        else:
            counts[a_word] = counts.get(a_word, 0)+1  # 用字典存储对应分词的词频
words_sort = list(counts.items())
words_sort.sort(key=lambda x: x[1], reverse=True)

newcloud = WordCloud(font_path=r"C:\Users\崔梦茹\Documents\作业\数据技术分析技术作业\NotoSansCJK.otf",
                     background_color="white", width=600, height=300, max_words=50)  # 生成词云
newcloud.generate_from_frequencies(counts)
image = newcloud.to_image()  # 转换成图片
image
```

    Building prefix dict from the default dictionary ...
    Loading model from cache C:\Users\崔梦茹\AppData\Local\Temp\jieba.cache
    Loading model cost 0.981 seconds.
    Prefix dict has been built successfully.
    




    
![png](output_13_1.png)
    




```python
pip install -i https://pypi.douban.com/simple gensim
```

    Looking in indexes: https://pypi.douban.com/simple
    Requirement already satisfied: gensim in c:\anaconda\lib\site-packages (4.2.0)
    Requirement already satisfied: smart-open>=1.8.1 in c:\anaconda\lib\site-packages (from gensim) (6.0.0)
    Requirement already satisfied: Cython==0.29.28 in c:\anaconda\lib\site-packages (from gensim) (0.29.28)
    Requirement already satisfied: numpy>=1.17.0 in c:\anaconda\lib\site-packages (from gensim) (1.20.3)
    Requirement already satisfied: scipy>=0.18.1 in c:\anaconda\lib\site-packages (from gensim) (1.7.1)
    Note: you may need to restart the kernel to use updated packages.
    


```python
from gensim.models import Word2Vec
from sklearn.cluster import KMeans
import warnings
warnings.filterwarnings('ignore')

words = []

for i in news:
    words.append(word_cut(i))
model = Word2Vec(words, sg=0, vector_size=300, window=5, min_count=5)  
keys = model.wv.key_to_index.keys()  
wordvector = []
for key in keys:
    wordvector.append(model.wv[key])  

distortions = []
for i in range(1, 40):
    word_kmeans = KMeans(n_clusters=i,
                         init='k-means++',
                         n_init=10,
                         max_iter=300,
                         random_state=0)  
    word_kmeans.fit(wordvector)
    distortions.append(word_kmeans.inertia_)  
plt.plot(range(1, 40), distortions, marker='o')  
plt.xlabel('Number of clusters')
plt.ylabel('Distortion')
```




    Text(0, 0.5, 'Distortion')




    
![png](output_15_1.png)
    



```python
word_kmeans = KMeans(n_clusters=10)  # 聚成10类
word_kmeans.fit(wordvector)

labels = word_kmeans.labels_

for num in range(0, 10):
    text = []
    for i in range(len(keys)):
        if labels[i] == num:
            text.append(list(keys)[i])  # 分别获得10类的聚类结果
    print(text)
```

    ['湖南', '贵州', '河北', '山西', '青海', '重庆市', '天津市', '黑龙江省', '墨西哥', '吉林省', '比利时', '印尼', '沙特', '云南省', '澳门', '摩洛哥', '省区市', '瑞士', '贵州省', '安徽省', '以外', '河南省', '发布会', '卫健委日', '超例', '辽宁省', '刚果', '哈萨克斯坦', '英国首相', '内蒙古自治区', '匈牙利', '全区', '黎巴嫩', '俄', '绥芬河', '集体', '实验室', '家', '金银', '首个', '趋缓', '研发', '员工', '多名', '业务', '经验', '病人', '吉林市', '受新冠', '升级', '救治', '今天', '月底', '调查', '快速', '放松', '封闭', '但', '小汤山', '外', '门诊', '世界卫生组织', '状态', '主席', '需要', '治愈率', '火神', '两例', '失业', '破', '山', '降', '乘客', '出行', '行动', '预约', '群体', '轨迹', '做', '重要', '重点', '逼近', '零', '逾', '联合国', '启程', '军队', '有序', '一名', '失业率', '监狱', '采样', '生活', '万份', '生产', '第二阶段', '诊疗', '公共', '市民', '复苏', '部长', '用', '省', '撤离', '停止', '总', '一季度', '临床', '恶化', '民航局', '规定', '大部分', '至人', '下降', '包机', '总干事', '病毒感染', '阶段', '采取', '购买', '药物', '返京', '密接', '中国政府', '欧盟', '试剂', '今起', '前往', '都', '流行', '提高', '方式', '航线', '现', '奥组委', '交易', '万人次', '安排', '尚未', '力度', '市', '厄瓜多尔', '一周', '收治', '个人', '食品', '市长', '采购', '最小', '床位', '抗议', '暂', '哈尔滨', '央视', '期', '外籍', '推动', '今年', '无法', '旅游业', '野生动物', '补助', '落实', '两天', '名新冠', '神山', '隐瞒', '警告', '裁员', '再度', '连降', '世界', '任务', '排除', '院士', '诊断', '项目', '外长', '缓解', '返回', '举措', '高', '收到', '主要', '传染病', '运营', '因新冠', '针对', '海鲜', '日前', '处于', '加大', '副', '不是', '吨', '区域', '召开', '迪拜', '大区', '万个', '基本', '合肥', '撤侨', '当地', '中心', '增幅', '试剂盒', '援', '鲍里斯', '禁令', '每天', '亿只', '临床试验', '呼吸机', '提升', '秘书', '餐厅', '举办', '封城', '人民', '价格', '分享', '使馆', '自', '同时', '鄂', '城市', '二级', '行业', '高三', '以下', '等国', '扩散', '引', '两', '流感', '监护', '团队', '援鄂', '创新', '社交', '可以', '加剧', '坚决', '省份', '万名', '航空', '半数', '事态', '告急', '其他', '总领馆', '资金', '降至例', '使用', '严防', '证据', '爱心', '系', '籍', '不明', '急', '运输', '万次', '佛罗里达州', '吗', '认为', '三级', '保持', '启用', '此前', '乘', '复学', '移动', '过去', '代表', '免疫', '即将', '入院', '建设', '说', '恢复正常', '首相', '防护', '多项', '捐款', '氯喹', '羟', '财政', '任何', '准备', '万多', '保护', '执行', '领导', '首尔', '大臣', '全力', '现在', '机制', '若', '正常', '等级', '夜店', '沪', '人士', '网络', '也', '首日', '宣言', '急需', '迎接', '让', '工资', '英雄', '逝者', '开', '解禁', '禁足', '追加', '毕业生', '接待', '经', '转机', '体温', '欧元', '联合', '变化', '幼儿园', '酒吧', '复阳', '紧张', '州长', '封闭式', '投资', '波', '包括', '主流', '环境', '公务员', '出租车', '发']
    ['最新', '巴西', '年月日', '日时', '泰国', '新加坡', '卫健委', '性', '截至', '均', '北京市', '专家组', '从', '达', '清零', '于', '医护人员', '最大', '好消息', '持续', '年', '非洲', '取消', '实施', '全部', '目前', '前', '今日', '受', '者', '首都', '国', '计划', '总理', '东京', '万人', '特朗普', '要求', '一级', '公主', '居家', '近', '活动', '小时', '上升', '举行', '最', '提供', '岁', '关闭', '卫生', '重启', '新闻', '企业', '因', '委员会', '完成', '合作', '观察', '阳性', '开始', '公民', '援助', '全面', '发地', '内', '佩戴', '暴发', '解除', '相关', '支持', '最高', '美', '捐赠', '及', '封锁', '牺牲', '抵达', '市场', '继续', '钟南山', '首批', '研究', '时间', '再次', '发生', '应', '同胞', '来', '应急', '大', '医生', '戴', '解封', '支援', '调整', '要', '逝世', '五一', '儿童', '管理', '发热', '呼吁', '服务', '复课', '呈', '逐步', '成', '推迟', '数据', '海外', '延期', '到', '日起', '社会', '没有', '假期', '开展', '面临', '大使馆', '奥运会', '社区', '加强', '扩大', '显示', '实行', '约', '并', '回国', '亿', '烈士', '提醒', '出台', '以上', '高校', '约翰逊', '事件', '学校', '队员', '景区']
    ['月', '冠状病毒', '美国', '北京', '情况', '感染', '患者', '湖北', '人', '全国', '为', '出现', '至', '人数', '名', '起', '连续', '宣布', '对', '万', '或', '个', '被', '发布', '天', '公布', '首次', '工作', '所有', '影响', '新', '又', '开放', '专家', '启动', '一', '经济', '有', '发现', '聚集', '进行', '应对', '是', '等', '国际', '多', '风险', '地区', '后', '中', '医疗队', '响应', '驻', '再', '称', '了', '总统', '可能', '限制', '进入', '复工', '部分', '可', '增加', '向', '丨', '号', '欧洲', '暂停', '健康', '病毒检测', '民众', '令', '开学', '重症', '疫苗', '传播', '政府', '航班', '紧急', '国内', '仍', '抗体', '返校', '入境', '悼念', '旅客']
    ['例', '确诊', '新增', '病例', '肺炎', '累计']
    ['荷兰', '阿根廷', '福建省', '四川省', '巴基斯坦', '葡萄牙', '江西省', '山西省', '波兰', '智利', '新西兰', '希腊', '肯尼亚', '青海省', '哥伦比亚', '河北省', '斯洛伐克', '塞尔维亚', '巴林', '乌克兰', '共计', '浙江省', '以色列', '伊拉克', '孟加拉国', '湖南省', '贫民窟', '塞内加尔', '白俄罗斯', '详情', '赞比亚', '捷克', '例均', '破万', '西藏自治区', '格鲁吉亚', '科威特', '毛里求斯', '境内', '越南', '奥地利', '缅甸', '斯里兰卡', '乌兹别克斯坦', '卢森堡', '总数', '至时', '江苏省', '纽约市', '卫健委月', '阿塞拜疆', '台湾', '突尼斯', '压力', '金', '考虑', '证明', '情况通报', '阿曼', '婴儿', '辽宁大连', '上调', '白宫', '利比亚', '雷', '罚款', '保加利亚', '文莱', '卡塔尔', '喀麦隆', '默哀', '阿尔巴尼亚', '派', '很', '教育部', '沈阳', '千例', '岗位', '布', '牡丹江', '危重', '给', '所', '考试', '统计', '得到', '会议', '生命', '埃塞俄比亚', '超人', '化', '亚美尼亚', '通告', '有关', '倍', '好', '严峻', '住院', '一个月', '工作人员', '下跌', '展开', '春节假期', '筛查', '发言人', '迪士尼', '复航', '流动', '返程', '家中', '海滩', '多地', '量', '也门', '吴尊友', '上海市', '指导', '舒兰市', '停运', '停课', '进出', '作用', '水平', '检疫', '津巴布韦', '型', '全员', '三个', '数量', '死于', '记者', '滞留', '立陶宛', '安道尔', '行程', '加纳', '洛杉矶', '多家', '减少', '之下', '男子', '重开', '黄石', '危机', '北美', '一例', '接近', '深圳', '普京', '例例', '第二批', '预测', '乌拉圭', '吉布提', '史', '圭亚那', '病情', '尼日利亚', '轻症', '错峰', '近万人', '叙利亚', '各', '销售', '胜利', '警惕', '发改委', '多国', '柳叶刀', '武汉协和医院', '阿尔及利亚', '营业', '回', '严禁', '供应链', '实现', '上班', '南京', '出征', '凯旋', '订正', '布基纳法索', '资助', '第一批', '蒙古国', '至例', '增例', '坦桑尼亚', '老挝', '塞浦路斯', '有名', '襄阳', '构成', '堂食', '同一', '陆续', '日本政府', '加快', '明确', '补贴', '办理', '而', '过万', '医务', '引发', '苏丹', '网友', '仪式', '两万', '不足', '国际航班', '传染', '亚洲', '接收', '比赛', '吉尔吉斯斯坦', '挑战', '数超', '序列', '名单', '近万', '外卖', '比', '座', '正在', '案例', '多数', '纳入', '明显', '圈', '经济衰退', '规模', '疾控', '削减', '具备', '级', '主任', '出席', '发出', '冠', '有效', '两个', '酒店', '地方', '关键', '心理', '大厅', '疾病', '须', '帮助', '指南', '这些', '老人', '如何', '办事处', '视频', '省市', '民航', '不断', '工人', '系统', '参与', '强调', '分批', '赤道几内亚', '供应', '近例', '大会', '次', '啦', '航空公司', '马里', '就诊', '份', '尚', '条', '建', '蛋白质', '全', '回升', '团结', '全体', '参加', '中考', '变', '看', '其', '防止', '处以', '多州', '进京']
    ['疫情', '的', '将', '中国', '武汉', '病毒', '检测', '国家', '和', '在', '防控', '抗疫', '医院', '口罩', '措施', '隔离', '核酸', '已', '人员', '医疗', '恢复', '不', '物资', '延长', '与', '防疫', '期间']
    ['万例', '全球', '世卫', '组织', '超过', '超']
    ['新冠', '例新冠', '日', '出院', '无', '输入', '境外', '治愈', '通报', '报告', '本地']
    ['新型', '死亡', '上海', '首例', '天津', '日本', '增至', '英国', '意大利', '韩国', '达例', '感染者', '西班牙', '升至', '印度', '广东', '德国', '辽宁', '时', '黑龙江', '山东', '重庆', '广西', '无症状', '香港', '俄罗斯', '四川', '陕西', '法国', '超万', '伊朗', '江苏', '福建', '云南', '疑似病例', '安徽', '浙江', '甘肃', '单日', '内蒙古', '吉林', '新疆', '海南', '加拿大', '天无', '昨日', '有例', '本土', '其中']
    ['河南', '江西', '宁夏', '澳大利亚', '菲律宾', '土耳其', '山东省', '马来西亚', '广东省', '湖北省', '突破', '直播', '阴性', '第例', '来自', '紧急状态', '西藏', '秘鲁', '年月日时', '例为', '阿联酋', '机构', '纽约州', '医学观察', '严重', '达到', '管控', '钻石', '强制', '疑似', '抗击', '康复', '确认', '含', '南非', '外交部', '重新', '兵团', '共有', '现有', '埃及', '结束', '以来', '已经', '赴', '集中', '会', '已有', '接受', '高风险', '一个', '治疗', '能力', '驰援', '进一步', '复产', '全省', '卫生部长', '至少', '反弹', '方舱', '邮轮', '数', '安全', '医务人员', '建议', '一天', '疾控中心', '结果', '共', '增长', '控制', '信息', '机场', '允许', '去世', '亿元', '卫生部', '广州', '禁止', '曾', '加州', '密切接触', '回应', '蔓延', '放宽', '医用', '高考', '需', '地', '养老院', '推出', '留学生', '关联', '痊愈', '护士', '武汉市', '口岸', '免费', '正式', '医护', '官员', '旅游', '学生', '正', '国务院', '发放', '工作者', '预计', '小区', '问题', '各地', '各国', '感谢', '冲击', '范围', '人群', '两周', '场所', '最后', '上', '一线', '潭', '加速', '下周', '临时', '运抵', '旅行', '边境', '关于', '我', '通过', '居民', '一律', '低', '每日', '以', '积极', '公共卫生', '铁路', '中小学', '除', '莫斯科', '张文宏', '症状', '媒体', '公司', '非', '客运', '消费', '我国', '不会', '排查', '导致', '接触', '纽约', '蔬菜', '占', '州', '未来', '中央', '常态', '第二', '决定', '必须', '突发', '发展', '成为', '黄冈', '下', '大规模', '高峰', '是否', '月份', '中方', '注意', '家庭', '深切', '回家', '下半旗', '宵禁', '表示', '游客', '通知', '未', '还', '级别', '医疗机构', '助力', '批准', '战疫', '确定', '样本', '就', '救助', '外出', '快递', '就业', '不得', '由', '年级', '政策', '形势', '大幅', '部门', '做好', '测试', '关注', '更', '共同', '师生', '申请', '保障', '同比', '严格', '只', '志哀', '线上', '联防', '联控', '一年', '避免', '官方', '考生', '下调', '距离', '表明']
    


```python
sum_GDP = ['国内生产总值', '第一产业增加值', '第二产业增加值', '第三产业增加值']
industry_GDP = ['农林牧渔业增加值', '工业增加值', '制造业增加值', '建筑业增加值']
industry2_GDP = ['批发和零售业增加值', '交通运输、仓储和邮政业增加值', '住宿和餐饮业增加值', '金融业增加值']
industry3_GDP = ['房地产业增加值', '信息传输、软件和信息技术服务业增加值',
                 '租赁和商务服务业增加值', '其他行业增加值']  # 对不同行业分四类来展现

fig = plt.figure()
fig, axes = plt.subplots(2, 2, figsize=(21, 15))  # 分别用四个子图来展现数据变化情况

axes[0][0].plot(data_economy[sum_GDP])
axes[0][0].legend(sum_GDP, prop=myfont)
axes[0][1].plot(data_economy[industry_GDP])
axes[0][1].legend(industry_GDP, prop=myfont)
axes[1][0].plot(data_economy[industry2_GDP])
axes[1][0].legend(industry2_GDP, prop=myfont)
axes[1][1].plot(data_economy[industry3_GDP])
axes[1][1].legend(industry3_GDP, prop=myfont)

plt.title('分行业GDP变化图', fontproperties=myfont)
```




    Text(0.5, 1.0, '分行业GDP变化图')




    <Figure size 432x288 with 0 Axes>



    
![png](output_17_2.png)
    



```python
from statsmodels.graphics.tsaplots import plot_acf
from pandas.plotting import autocorrelation_plot
from statsmodels.sandbox.stats.diagnostic import acorr_ljungbox


GDP_type = ['国内生产总值', '第一产业增加值', '第二产业增加值', '第三产业增加值', 
            '农林牧渔业增加值', '工业增加值', '制造业增加值', '建筑业增加值', '批发和零售业增加值',
            '交通运输、仓储和邮政业增加值', '住宿和餐饮业增加值', '金融业增加值', 
            '房地产业增加值', '信息传输、软件和信息技术服务业增加值', '租赁和商务服务业增加值', '其他行业增加值']

for i in GDP_type:
    each_data = data_economy[i][:-2]
    plt.figure(figsize=(30, 6))
    ax1 = plt.subplot(1, 3, 1)
    ax2 = plt.subplot(1, 3, 2)
    ax3 = plt.subplot(1, 3, 3)
    LB2, P2 = acorr_ljungbox(each_data)  # 进行纯随机性检验
    plot_acf(each_data, ax=ax1)
    autocorrelation_plot(each_data, ax=ax2)  # 进行平稳性检验
    ax3.plot(P2)
```


    
![png](output_18_0.png)
    



    
![png](output_18_1.png)
    



    
![png](output_18_2.png)
    



    
![png](output_18_3.png)
    



    
![png](output_18_4.png)
    



    
![png](output_18_5.png)
    



    
![png](output_18_6.png)
    



    
![png](output_18_7.png)
    



    
![png](output_18_8.png)
    



    
![png](output_18_9.png)
    



    
![png](output_18_10.png)
    



    
![png](output_18_11.png)
    



    
![png](output_18_12.png)
    



    
![png](output_18_13.png)
    



    
![png](output_18_14.png)
    



    
![png](output_18_15.png)
    



```python
from statsmodels.tsa.arima_model import ARMA
from statsmodels.tsa.stattools import arma_order_select_ic

warnings.filterwarnings('ignore')
data_arma = pd.DataFrame(data_economy['国内生产总值'][:-2])  # 选取疫情期前的16个季度进行建模
a, b = arma_order_select_ic(data_arma, ic='hqic')['hqic_min_order']
arma = ARMA(data_arma, order=(a, b)).fit()  # 使用ARMA建模
rate1 = list(data_economy['国内生产总值'][-2] /
             arma.forecast(steps=1)[0])  # 获得疫情期当季度的预测值
rate1  # 实际值与预测值的比率
```




    [0.8273103019180329]




```python
from pyecharts import options as opts
from pyecharts.charts import Liquid

c = (
    Liquid()
    .add("实际值/预测值", rate1, is_outline_show=False)
    .set_global_opts(title_opts=opts.TitleOpts(title="第一季度国民生产总值实际值与预测值比例", 
                                               pos_left="center"))
)
c.render_notebook()
```





<script>
    require.config({
        paths: {
            'echarts':'https://assets.pyecharts.org/assets/echarts.min', 'echarts-liquidfill':'https://assets.pyecharts.org/assets/echarts-liquidfill.min'
        }
    });
</script>

        <div id="1cf54d67d72c4f628a4c11b4df0bcf6a" style="width:900px; height:500px;"></div>

<script>
        require(['echarts', 'echarts-liquidfill'], function(echarts) {
                var chart_1cf54d67d72c4f628a4c11b4df0bcf6a = echarts.init(
                    document.getElementById('1cf54d67d72c4f628a4c11b4df0bcf6a'), 'white', {renderer: 'canvas'});
                var option_1cf54d67d72c4f628a4c11b4df0bcf6a = {
    "animation": true,
    "animationThreshold": 2000,
    "animationDuration": 1000,
    "animationEasing": "cubicOut",
    "animationDelay": 0,
    "animationDurationUpdate": 300,
    "animationEasingUpdate": "cubicOut",
    "animationDelayUpdate": 0,
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597"
    ],
    "series": [
        {
            "type": "liquidFill",
            "name": "\u5b9e\u9645\u503c/\u9884\u6d4b\u503c",
            "data": [
                0.8273103019180329
            ],
            "waveAnimation": true,
            "animationDuration": 2000,
            "animationDurationUpdate": 1000,
            "color": [
                "#294D99",
                "#156ACF",
                "#1598ED",
                "#45BDFF"
            ],
            "shape": "circle",
            "outline": {
                "show": false
            },
            "label": {
                "show": true,
                "position": "inside",
                "margin": 8,
                "fontSize": 50
            }
        }
    ],
    "legend": [
        {
            "data": [],
            "selected": {},
            "show": true,
            "padding": 5,
            "itemGap": 10,
            "itemWidth": 25,
            "itemHeight": 14
        }
    ],
    "tooltip": {
        "show": true,
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "borderWidth": 0
    },
    "title": [
        {
            "text": "\u7b2c\u4e00\u5b63\u5ea6\u56fd\u6c11\u751f\u4ea7\u603b\u503c\u5b9e\u9645\u503c\u4e0e\u9884\u6d4b\u503c\u6bd4\u4f8b",
            "left": "center",
            "padding": 5,
            "itemGap": 10
        }
    ]
};
                chart_1cf54d67d72c4f628a4c11b4df0bcf6a.setOption(option_1cf54d67d72c4f628a4c11b4df0bcf6a);
        });
    </script>





```python
warnings.filterwarnings('ignore')
data_arma = pd.DataFrame(data_economy['工业增加值'][:-2])
a, b = arma_order_select_ic(data_arma, ic='hqic')['hqic_min_order']
arma = ARMA(data_arma, order=(a, b)).fit()
rate2 = list(data_economy['工业增加值'][-2]/arma.forecast(steps=1)[0])
c = (
    Liquid()
    .add("实际值/预测值", rate2, is_outline_show=False)
    .set_global_opts(title_opts=opts.TitleOpts(title="工业增加值比例", pos_left="center"))
)
c.render_notebook()
```





<script>
    require.config({
        paths: {
            'echarts':'https://assets.pyecharts.org/assets/echarts.min', 'echarts-liquidfill':'https://assets.pyecharts.org/assets/echarts-liquidfill.min'
        }
    });
</script>

        <div id="c768fc2efc654db098e6ce40585d93bd" style="width:900px; height:500px;"></div>

<script>
        require(['echarts', 'echarts-liquidfill'], function(echarts) {
                var chart_c768fc2efc654db098e6ce40585d93bd = echarts.init(
                    document.getElementById('c768fc2efc654db098e6ce40585d93bd'), 'white', {renderer: 'canvas'});
                var option_c768fc2efc654db098e6ce40585d93bd = {
    "animation": true,
    "animationThreshold": 2000,
    "animationDuration": 1000,
    "animationEasing": "cubicOut",
    "animationDelay": 0,
    "animationDurationUpdate": 300,
    "animationEasingUpdate": "cubicOut",
    "animationDelayUpdate": 0,
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597"
    ],
    "series": [
        {
            "type": "liquidFill",
            "name": "\u5b9e\u9645\u503c/\u9884\u6d4b\u503c",
            "data": [
                0.8402465197466461
            ],
            "waveAnimation": true,
            "animationDuration": 2000,
            "animationDurationUpdate": 1000,
            "color": [
                "#294D99",
                "#156ACF",
                "#1598ED",
                "#45BDFF"
            ],
            "shape": "circle",
            "outline": {
                "show": false
            },
            "label": {
                "show": true,
                "position": "inside",
                "margin": 8,
                "fontSize": 50
            }
        }
    ],
    "legend": [
        {
            "data": [],
            "selected": {},
            "show": true,
            "padding": 5,
            "itemGap": 10,
            "itemWidth": 25,
            "itemHeight": 14
        }
    ],
    "tooltip": {
        "show": true,
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "borderWidth": 0
    },
    "title": [
        {
            "text": "\u5de5\u4e1a\u589e\u52a0\u503c\u6bd4\u4f8b",
            "left": "center",
            "padding": 5,
            "itemGap": 10
        }
    ]
};
                chart_c768fc2efc654db098e6ce40585d93bd.setOption(option_c768fc2efc654db098e6ce40585d93bd);
        });
    </script>





```python
warnings.filterwarnings('ignore')
data_arma = pd.DataFrame(data_economy['制造业增加值'][:-2])
a, b = arma_order_select_ic(data_arma, ic='hqic')['hqic_min_order']
arma = ARMA(data_arma, order=(a, b)).fit()
rate3 = list(data_economy['制造业增加值'][-2]/arma.forecast(steps=1)[0])
c = (
    Liquid()
    .add("实际值/预测值", rate3, is_outline_show=False)
    .set_global_opts(title_opts=opts.TitleOpts(title="制造业增加值", pos_left="center"))
)
c.render_notebook()
```





<script>
    require.config({
        paths: {
            'echarts':'https://assets.pyecharts.org/assets/echarts.min', 'echarts-liquidfill':'https://assets.pyecharts.org/assets/echarts-liquidfill.min'
        }
    });
</script>

        <div id="dea65bf882c344058dda0459f57bdcb0" style="width:900px; height:500px;"></div>

<script>
        require(['echarts', 'echarts-liquidfill'], function(echarts) {
                var chart_dea65bf882c344058dda0459f57bdcb0 = echarts.init(
                    document.getElementById('dea65bf882c344058dda0459f57bdcb0'), 'white', {renderer: 'canvas'});
                var option_dea65bf882c344058dda0459f57bdcb0 = {
    "animation": true,
    "animationThreshold": 2000,
    "animationDuration": 1000,
    "animationEasing": "cubicOut",
    "animationDelay": 0,
    "animationDurationUpdate": 300,
    "animationEasingUpdate": "cubicOut",
    "animationDelayUpdate": 0,
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597"
    ],
    "series": [
        {
            "type": "liquidFill",
            "name": "\u5b9e\u9645\u503c/\u9884\u6d4b\u503c",
            "data": [
                0.788112043290188
            ],
            "waveAnimation": true,
            "animationDuration": 2000,
            "animationDurationUpdate": 1000,
            "color": [
                "#294D99",
                "#156ACF",
                "#1598ED",
                "#45BDFF"
            ],
            "shape": "circle",
            "outline": {
                "show": false
            },
            "label": {
                "show": true,
                "position": "inside",
                "margin": 8,
                "fontSize": 50
            }
        }
    ],
    "legend": [
        {
            "data": [],
            "selected": {},
            "show": true,
            "padding": 5,
            "itemGap": 10,
            "itemWidth": 25,
            "itemHeight": 14
        }
    ],
    "tooltip": {
        "show": true,
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "borderWidth": 0
    },
    "title": [
        {
            "text": "\u5236\u9020\u4e1a\u589e\u52a0\u503c",
            "left": "center",
            "padding": 5,
            "itemGap": 10
        }
    ]
};
                chart_dea65bf882c344058dda0459f57bdcb0.setOption(option_dea65bf882c344058dda0459f57bdcb0);
        });
    </script>





```python
data_arma = pd.DataFrame(data_economy['批发和零售业增加值'][:-2])
a, b = arma_order_select_ic(data_arma, ic='hqic')['hqic_min_order']
arma = ARMA(data_arma, order=(a, b)).fit()
rate4 = list(data_economy['批发和零售业增加值'][-2]/arma.forecast(steps=1)[0])
c = (
    Liquid()
    .add("实际值/预测值", rate4, is_outline_show=False)
    .set_global_opts(title_opts=opts.TitleOpts(title="批发和零售业增加值", pos_left="center"))
)
c.render_notebook()
```





<script>
    require.config({
        paths: {
            'echarts':'https://assets.pyecharts.org/assets/echarts.min', 'echarts-liquidfill':'https://assets.pyecharts.org/assets/echarts-liquidfill.min'
        }
    });
</script>

        <div id="9c2b779b9274494cbd1a1ee571c25b70" style="width:900px; height:500px;"></div>

<script>
        require(['echarts', 'echarts-liquidfill'], function(echarts) {
                var chart_9c2b779b9274494cbd1a1ee571c25b70 = echarts.init(
                    document.getElementById('9c2b779b9274494cbd1a1ee571c25b70'), 'white', {renderer: 'canvas'});
                var option_9c2b779b9274494cbd1a1ee571c25b70 = {
    "animation": true,
    "animationThreshold": 2000,
    "animationDuration": 1000,
    "animationEasing": "cubicOut",
    "animationDelay": 0,
    "animationDurationUpdate": 300,
    "animationEasingUpdate": "cubicOut",
    "animationDelayUpdate": 0,
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597"
    ],
    "series": [
        {
            "type": "liquidFill",
            "name": "\u5b9e\u9645\u503c/\u9884\u6d4b\u503c",
            "data": [
                0.7499102998955828
            ],
            "waveAnimation": true,
            "animationDuration": 2000,
            "animationDurationUpdate": 1000,
            "color": [
                "#294D99",
                "#156ACF",
                "#1598ED",
                "#45BDFF"
            ],
            "shape": "circle",
            "outline": {
                "show": false
            },
            "label": {
                "show": true,
                "position": "inside",
                "margin": 8,
                "fontSize": 50
            }
        }
    ],
    "legend": [
        {
            "data": [],
            "selected": {},
            "show": true,
            "padding": 5,
            "itemGap": 10,
            "itemWidth": 25,
            "itemHeight": 14
        }
    ],
    "tooltip": {
        "show": true,
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "borderWidth": 0
    },
    "title": [
        {
            "text": "\u6279\u53d1\u548c\u96f6\u552e\u4e1a\u589e\u52a0\u503c",
            "left": "center",
            "padding": 5,
            "itemGap": 10
        }
    ]
};
                chart_9c2b779b9274494cbd1a1ee571c25b70.setOption(option_9c2b779b9274494cbd1a1ee571c25b70);
        });
    </script>





```python
data_arma = pd.DataFrame(data_economy['金融业增加值'][:-2])
a, b = arma_order_select_ic(data_arma, ic='hqic')['hqic_min_order']
arma = ARMA(data_arma, order=(a, b)).fit()
rate = list(data_economy['金融业增加值'][-2]/arma.forecast(steps=1)[0])
c = (
    Liquid()
    .add("实际值/预测值", rate, is_outline_show=False)
    .set_global_opts(title_opts=opts.TitleOpts(title="金融业增加值", pos_left="center"))
)
c.render_notebook()
```





<script>
    require.config({
        paths: {
            'echarts':'https://assets.pyecharts.org/assets/echarts.min', 'echarts-liquidfill':'https://assets.pyecharts.org/assets/echarts-liquidfill.min'
        }
    });
</script>

        <div id="f7ba6151f5c1439790af3e7796bf96fe" style="width:900px; height:500px;"></div>

<script>
        require(['echarts', 'echarts-liquidfill'], function(echarts) {
                var chart_f7ba6151f5c1439790af3e7796bf96fe = echarts.init(
                    document.getElementById('f7ba6151f5c1439790af3e7796bf96fe'), 'white', {renderer: 'canvas'});
                var option_f7ba6151f5c1439790af3e7796bf96fe = {
    "animation": true,
    "animationThreshold": 2000,
    "animationDuration": 1000,
    "animationEasing": "cubicOut",
    "animationDelay": 0,
    "animationDurationUpdate": 300,
    "animationEasingUpdate": "cubicOut",
    "animationDelayUpdate": 0,
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597"
    ],
    "series": [
        {
            "type": "liquidFill",
            "name": "\u5b9e\u9645\u503c/\u9884\u6d4b\u503c",
            "data": [
                1.1258103869037306
            ],
            "waveAnimation": true,
            "animationDuration": 2000,
            "animationDurationUpdate": 1000,
            "color": [
                "#294D99",
                "#156ACF",
                "#1598ED",
                "#45BDFF"
            ],
            "shape": "circle",
            "outline": {
                "show": false
            },
            "label": {
                "show": true,
                "position": "inside",
                "margin": 8,
                "fontSize": 50
            }
        }
    ],
    "legend": [
        {
            "data": [],
            "selected": {},
            "show": true,
            "padding": 5,
            "itemGap": 10,
            "itemWidth": 25,
            "itemHeight": 14
        }
    ],
    "tooltip": {
        "show": true,
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "borderWidth": 0
    },
    "title": [
        {
            "text": "\u91d1\u878d\u4e1a\u589e\u52a0\u503c",
            "left": "center",
            "padding": 5,
            "itemGap": 10
        }
    ]
};
                chart_f7ba6151f5c1439790af3e7796bf96fe.setOption(option_f7ba6151f5c1439790af3e7796bf96fe);
        });
    </script>





```python
data_arma = pd.DataFrame(data_economy['信息传输、软件和信息技术服务业增加值'][:-2])
a, b = arma_order_select_ic(data_arma, ic='hqic')['hqic_min_order']
arma = ARMA(data_arma, order=(a, b)).fit()
rate = list(data_economy['信息传输、软件和信息技术服务业增加值'][-2]/arma.forecast(steps=1)[0])
c = (
    Liquid()
    .add("实际值/预测值", rate, is_outline_show=False)
    .set_global_opts(title_opts=opts.TitleOpts(title="信息传输、软件和信息技术服务业增加值", 
                                               pos_left="center"))
)
c.render_notebook()
```





<script>
    require.config({
        paths: {
            'echarts':'https://assets.pyecharts.org/assets/echarts.min', 'echarts-liquidfill':'https://assets.pyecharts.org/assets/echarts-liquidfill.min'
        }
    });
</script>

        <div id="6cc05d468501437ea6c04ae7f7f4f39b" style="width:900px; height:500px;"></div>

<script>
        require(['echarts', 'echarts-liquidfill'], function(echarts) {
                var chart_6cc05d468501437ea6c04ae7f7f4f39b = echarts.init(
                    document.getElementById('6cc05d468501437ea6c04ae7f7f4f39b'), 'white', {renderer: 'canvas'});
                var option_6cc05d468501437ea6c04ae7f7f4f39b = {
    "animation": true,
    "animationThreshold": 2000,
    "animationDuration": 1000,
    "animationEasing": "cubicOut",
    "animationDelay": 0,
    "animationDurationUpdate": 300,
    "animationEasingUpdate": "cubicOut",
    "animationDelayUpdate": 0,
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597"
    ],
    "series": [
        {
            "type": "liquidFill",
            "name": "\u5b9e\u9645\u503c/\u9884\u6d4b\u503c",
            "data": [
                1.0939103463927353
            ],
            "waveAnimation": true,
            "animationDuration": 2000,
            "animationDurationUpdate": 1000,
            "color": [
                "#294D99",
                "#156ACF",
                "#1598ED",
                "#45BDFF"
            ],
            "shape": "circle",
            "outline": {
                "show": false
            },
            "label": {
                "show": true,
                "position": "inside",
                "margin": 8,
                "fontSize": 50
            }
        }
    ],
    "legend": [
        {
            "data": [],
            "selected": {},
            "show": true,
            "padding": 5,
            "itemGap": 10,
            "itemWidth": 25,
            "itemHeight": 14
        }
    ],
    "tooltip": {
        "show": true,
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "borderWidth": 0
    },
    "title": [
        {
            "text": "\u4fe1\u606f\u4f20\u8f93\u3001\u8f6f\u4ef6\u548c\u4fe1\u606f\u6280\u672f\u670d\u52a1\u4e1a\u589e\u52a0\u503c",
            "left": "center",
            "padding": 5,
            "itemGap": 10
        }
    ]
};
                chart_6cc05d468501437ea6c04ae7f7f4f39b.setOption(option_6cc05d468501437ea6c04ae7f7f4f39b);
        });
    </script>





```python

```
