# carp
使用tushare获取A股数据保存到mongodb并提供本地接口访问

# Usage

## 启动mongodb server
create cache directory
```sh
cd carp ; mkdir cache
mongodb -f mongodb.conf
```

## 同步数据
```python
context.sync_history_bar()
```

## 读取数据

* 获取某只股票的详细信息

```python
basic = context.get_basic()
basic.info('000002')
```

```sh
area                         深圳
bvps                      10.54
esp                       1.005
fixedAssets              685739
gpr                       31.73
holders                  208975
industry                   全国地产
liquidAssets        8.99749e+07
name                       万 科Ａ
npr                        9.47
outstanding               97.09
pb                          3.1
pe                        24.36
perundp                    5.76
profit                    34.23
reserved                 905916
reservedPerShare           0.82
rev                        0.04
timeToMarket         1991-01-29
totalAssets         1.01838e+08
totals                   110.39
undp                6.35703e+06
Name: 000002, dtype: object
```


* 获取市场基本数据
```python
basic = context.get_basic()
basic.df()
```

```sh
       area  bvps    esp  fixedAssets    gpr   holders industry  liquidAssets  \
code
000882   北京  2.84  0.011      5505.55  36.91  138416.0       百货     322886.03
600635   上海  2.47  0.121    405055.38  14.68  202454.0     供气供热     628222.56
002702   福建  1.60 -0.052     31236.58  29.26   55226.0       食品      58924.83
000909   浙江  3.40  0.087      1989.45  11.83   33316.0      综合类     379352.22
600895   上海  5.41  0.255      1479.16  46.54  122383.0     园区开发     862835.88
600213   江苏  0.71  0.112     23624.40  16.84   26421.0     汽车整车     454487.75
603778   北京  1.92  0.067       993.25  23.82   35005.0     建筑施工     160503.88
600865   浙江  4.64  0.155     14625.50  27.01   28719.0       百货      48666.62
300390   江苏  2.41  0.066     25445.63  20.95   16371.0      元器件      43705.53
002208   安徽  5.10  0.069      6756.07  26.76   34015.0     区域地产    1148314.00
...


         name    npr     ...           pe  perundp   profit   reserved  \
code                     ...
000882   华联股份   3.89     ...       240.29     0.07   -76.12  465522.50
600635   大众公用  10.69     ...        31.99     0.51   -18.54  109635.42
002702   海欣食品  -4.18     ...         0.00     0.33  -409.12   23162.76
000909   数源科技   1.81     ...        94.32     0.68    20.19   47205.76
600895   张江高科  57.45     ...        42.95     2.07     1.72  263649.78
600213   亚星客车   1.83     ...        76.65    -2.06   -43.38   34209.40
...



        reservedPerShare      rev  timeToMarket  totalAssets totals  \
code
000882              1.70    -0.25    1998-06-16   1399492.25  27.37
600635              0.37    -0.76    1993-03-04   1994932.25  29.52
002702              0.48     0.88    2012-10-11    102808.57   4.81
000909              1.51    49.28    1999-05-07    449869.56   3.12
600895              1.70   -54.38    1996-04-22   1941576.63  15.49
...
```



* 获取某只股票的日K
```python
context.get_date_bars('000002', '2015-11-11', '2018-05-11', freq = util.FREQ_DAY)
```


```sh
           amount  close   datetime   high    low   open   tor        vol
0    8.899396e+08  27.94 2018-05-11  28.19  27.73  28.10  0.33   318303.0
1    1.204133e+09  28.07 2018-05-10  28.53  27.70  28.32  0.44   429639.0
2    1.152922e+09  28.20 2018-05-09  28.50  28.00  28.50  0.42   409030.0
3    1.857899e+09  28.69 2018-05-08  28.86  27.33  27.40  0.68   655817.0
4    1.110388e+09  27.38 2018-05-07  27.51  26.72  27.01  0.42   408453.0
5    1.154815e+09  26.91 2018-05-04  27.56  26.81  27.26  0.44   425383.0
6    1.466857e+09  27.33 2018-05-03  27.66  26.56  27.65  0.56   542419.0
7    1.111293e+09  27.68 2018-05-02  28.66  27.63  28.40  0.41   397170.0
8    1.071392e+09  28.40 2018-04-27  28.75  27.42  28.66  0.39   381958.0

...

```

```python
context.get_count_bars('000002', None, limit = 5, freq = util.FREQ_DAY)
```

```
          amount  close   datetime   high    low   open      tor        vol
0   1.951842e+09  27.99 2018-06-15  28.30  27.33  27.37  0.72000  698195.00
1   9.804182e+08  27.37 2018-06-14  27.76  27.12  27.48  0.37000  357604.00
2   1.680584e+09  27.64 2018-06-13  28.25  27.31  27.41  0.62000  602843.00
3   1.376049e+09  27.71 2018-06-12  27.83  27.23  27.57  0.51000  498372.00
4   1.437706e+09  27.31 2018-06-11  27.73  26.68  26.74  0.54000  526436.00
...
```


* sample render


```python
r = render.StockBarRender.create('000002', '2017-10-10', '2018-03-20', util.FREQ_DAY)

page = render.WebPage('test')
page.add(r)
page.show()
```

![image](https://github.com/lynink/carp/blob/master/test/render.png)



# TODO

 * 添加其他周期数据




