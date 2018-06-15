# ECUSTMap - 华理地图可视化

## 简介
ECUSTMap 是基于百度地图与mapv根据华理校区定制的js库。由于该库只是对百度地图与mapv简单封装，所以如果出现bug可以参照以上开源库的API文档解决。
* mapv: https://github.com/huiyan-fe/mapv
* bmap: http://lbsyun.baidu.com/index.php?title=jspopular3.0

## 使用方法
1. 申请百度地图api后,修改ak
2. 引入mapv.js,map.js文件

```
<script type="text/javascript" src="http://api.map.baidu.com/api?v=3.0&ak=你的ak"></script>
<script type="text/javascript" src="/static/dist/js/map/map.js"></script>
<script type="text/javascript" src="/static/dist/js/map/mapv.js"></script>
```
3. 在页面中定义一个div并添加id
```
<div style="height: 560px; width: 100%;" id="myECUSTMap"></div>
```
4. 新建地图
```
// myECUSTMap为div的id，XuHui为徐汇校区
var ECUSTMap = new ECUSTMap('myECUSTMap');
// 将地图移动到徐汇校区
ECUSTMap.moveToXuHui();
```

## 注意事项
* ECUSTMap是基于: bmap **v3.0** 版本 mapv **2.0** 版本。其中mapv的2.0版本为预发布版本，其API以后可能会发生改变。bmap的ak是在 **百度地图开放平台** 申请的。
* 百度地图经纬度拾取系统: http://api.map.baidu.com/lbsapi/getpoint/index.html
* 百度地图使用的坐标系标准为: BD-09，国内其他地图厂商使用的标准均为: GCJ02(火星坐标)，国外基本为WGS84。如果坐标不是从百度地图经纬度拾取系统获取的需要对其进行换算。在线转换: http://mapclub.cn/archives/2168，开源js版: http://wandergis.github.io/coordtransform/

## 源码组成与功能简介
* 三大校区坐标与缩放配置
* 三大校区定位切换
* 标记点样式配置
* 标记点控制
* 热力图控制
* 蜂窝图控制
* 地图样式配置

## API
创建地图原型
> newMap(id)
```
// id: 地图的id [string]
// 用于实例化地图

var ECUSTMap = new ECUSTMap('myECUSTMap');
```

更换地图样式
> setMapStyle(mapStyle)
```
// mapStyle: 地图样式 [json]
// 更改地图样式，具体可以参考: http://lbsyun.baidu.com/custom/list.htm，也可以自定义样式: http://developer.baidu.com/map/custom/

var mapStyle = {
    features: ["road", "building", "water", "land"],//隐藏地图上的poi
    style: "dark"  //设置地图风格为高端黑
};
ECUSTMap.setMapStyle(mapStyle);
```

移动视觉中心至不同校区
> moveToXuHui()
```
// 移动至徐汇校区

var ECUSTMap = new ECUSTMap('myECUSTMap');
ECUSTMap.moveToXuHui();
```
> moveToFengXian()
```
// 移动至奉贤校区

var ECUSTMap = new ECUSTMap('myECUSTMap');
ECUSTMap.moveToFengXian();
```
> moveToJinShan()
```
// 移动至金山校区

var ECUSTMap = new ECUSTMap('myECUSTMap');
ECUSTMap.moveToJinShan();
```

添加标记点
> addPoint(lng, lat, count, iconUrl, campus)
```
// lng: 经度 [double]
// lat: 维度 [double]
// count: 标记点的值 [int]
// iconUrl: 标记点图片样式 [string(url)]
// campus: 校区 [string]
// return: 标记点对象 [object(BMap.Marker)]
// 在地图中添加标记点。标记点需根据不同校区存放。标记点的值(count)本是用于标记热力图，但现在 **已弃用** ，可设为任意值。

var point = ECUSTMap.addPoint(lng, lat, value, "./icon_url", "XuHui");
```

删除标记点
> deleteAllPoints()
```
// 删除所有标记点，删除时将隐藏所有标记点

ECUSTMap.deleteAllPoints()
```

显示标记点
> showPoints()
```
// 显示所有标记点

ECUSTMap.showPoints()
```

隐藏标记点
> hidePoints()
```
// 隐藏所有标记点

ECUSTMap.hidePoints()
```

标记点的信息内容
> msg(title, content, img)
```
// title: 信息标题 [string]
// content: 信息主要内容 [string]
// img: 信息配图 [string(url)]
// return: html [string(HTML)]
// 标记点统一信息样式，返回对象为html内容，用户可以覆写该函数用于自定义信息样式

ECUSTMap.msg("标题", "内容", "./img_url")
```
根据标记点添加事件
> addMsg(marker, content)
```
// marker: 标记点对象 [object(BMap.Marker)]
// content: 显示的内容 [string(HTML)]

var point = ECUSTMap.addPoint(lng, lat, value, "./icon_url", "XuHui");
ECUSTMap.addMsg(reviewPoint, ECUSTMap.msg("标题", "内容", "./img_url"));
```

添加热力点
> addHeatPoint(lng, lat, count)
```
// lng: 经度 [double]
// lat: 维度 [double]
// count: 热力点的值 [int]
// 在地图中添加热力点，用于显示热力图与蜂窝图。标记点与热力点分开存放，如果需要同时显示标记点与热力点，则需要同时添加两种点

ECUSTMap.addHeatPoint(lng, lat, value);
```

删除热力点
> deleteHeatPoints()
```
// 删除所有热力点，删除时将关闭热力图与蜂窝图

ECUSTMap.deleteHeatPoints();
```

显示热力图
> showHeatMap(max)
```
// max: 热力图最大值 [iint]
// 显示热力图，max为热力图中的最大值

ECUSTMap.showHeatMap(max);
```

关闭热力图
> closeHeatMap()
```
// 关闭热力图

ECUSTMap.closeHeatMap();
```

显示蜂窝图(热力图变种)
> showHoneycombMap(max)
```
// max: 蜂窝图最大值 [iint]
// 显示蜂窝图，max为蜂窝图中的最大值

ECUSTMap.showHoneycombMap(max);
```

关闭蜂窝图
> closeHoneycombMap()
```
// 关闭蜂窝图

ECUSTMap.closeHoneycombMap();
```

## 二次开发
该js库对外暴露了原bmap对象，所以可以直接使用它，进行功能扩展。具体可以参考bmap的官方文档
```
// 使用该扩展接口直接更改地图样式

var mapStyle = {
    features: ["road", "building", "water", "land"],//隐藏地图上的poi
    style: "dark"  //设置地图风格为高端黑
};
ECUSTMap.map.setMapStyle(mapStyle);
```

## 文件说明
* map.js ECUSTMap地图主体
* mapv.js mapv v2.0.23
* 奉贤校区建筑坐标.json 奉贤校区建筑坐标(使用标准为BD-09)
* 徐汇校区建筑坐标.json 徐汇校区建筑坐标(使用标准为BD-09)