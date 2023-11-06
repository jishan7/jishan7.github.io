# 即插即用demo系列——结巴分词并计算权重

需要自己提前下载安装好python扩展包——jieba分词

```python
# coding=gbk
## 使用停词表和自定义新词表，以并行分词的形式，从文章中分词并最终输出TF/IDF权重排行的demo

import time
import sys
import jieba
import jieba.analyse
jieba.load_userdict("userdict.txt")
jieba.enable_parallel(4)  # 开始多进程并行处理


def getWords(content):
    # 去除停用词
    stopkey = [line.strip().decode('utf-8').encode('gbk') for line in open('stopkey.txt').readlines()]
    stopkey = {}.fromkeys(stopkey)
    segs = jieba.cut(content, cut_all=False)
    final = ''
    for seg in segs:
        seg = seg.strip().encode('gbk')

        if seg not in stopkey and len(seg) > 0:
            final += seg
            final += ' '

    return final

t1 = time.time()

content = open("news.txt", 'r').read()

final = getWords(content)
print final

tags2 = jieba.analyse.extract_tags(final, topK=10)

print '输出：'
for word in tags2:
    print word.encode('gbk')

t2 = time.time()
tm_cost = t2-t1

print 'time', tm_cost
print 'speed', len(content)/tm_cost, " bytes/second"

jieba.disable_parallel()
```


新闻 news.txt的内容：
```
台风红色预警:莫兰蒂明登陆闽粤,狂风暴雨来袭,福建广东多地中小学停课多趟高铁航班取消,中秋赏月或受影响。（新华社）

中国天气网讯 中央气象台14日06时发布台风红色预警：

今年第14号台风“莫兰蒂”（超强台风级）的中心今天（14日）早晨5点钟位于我国福建省漳浦县东南大约510公里的台湾以南海面上，就是北纬21.1度、东经121.4度，中心附近最大风力有17级以上（65米/秒），中心最低气压为905百帕，七级风圈半径280～350公里，十级风圈半径120～180公里，十二级风圈半径80～100公里。

台风红色预警：“莫兰蒂”15日登陆闽粤 狂风暴雨来袭

预计，“莫兰蒂”将以每小时20公里左右的速度向西北方向移动，14日白天将掠过台湾东南部，趋向闽粤沿海，之后强度逐渐减弱，并将于15日凌晨到上午在福建晋江到广东惠来一带沿海登陆（42～50m/s，14～15级，强台风级），随后转向偏北方向移动，于16日凌晨到上午在江西境内减弱为热带低压。

大风预报：14日08时至15日08时，南海东北部、巴士海峡、台湾以东洋面、台湾海峡、台湾沿海、福建沿海、浙江南部沿海、东海西部和南部将有7～10级大风，部分海域和地区风力有11～12级，“莫兰蒂”中心经过的附近海域和地区风力可达13～17级以上。

台风红色预警：“莫兰蒂”15日登陆闽粤 狂风暴雨来袭

降水预报：14日08时至15日08时，浙江东部和南部、福建东部和中南部、广东东部、台湾大部等地的部分地区有大雨或暴雨，其中，福建东北部和东南部、广东东部、台湾中南部等地的局地有大暴雨或特大暴雨（250～450毫米），上述局地并伴有短时强降水，小时雨强可达30～60毫米。

台风红色预警：“莫兰蒂”15日登陆闽粤 狂风暴雨来袭

防御指南：
政府及相关部门按照职责做好防台风抢险应急工作；
相关水域水上作业和过往船舶应当回港避风，加固港口设施，防止船舶走锚、搁浅和碰撞；
停止室内外大型集会和高空等户外危险作业。
加固或者拆除易被风吹动的搭建物,人员切勿随意外出，应尽可能待在防风安全的地方，确保老人小孩留在家中最安全的地方，危房人员及时转移。当台风中心经过时风力会减小或者静止一段时间，切记强风将会突然吹袭，应当继续留在安全处避风，危房人员及时转移。
相关地区应当注意防范强降水可能引发的山洪、地质灾害。
```

用户词典 userdict.txt的内容：
```
云计算 5
李小福 2 nr
创新办 3 i
easy_install 3 eng
韩玉赏鉴 3 nz
```

停词表 stopkey.txt
请自行搜索下载
