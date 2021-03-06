---
layout: post
title:  "微信小程序之根据经纬度反查地址"
categories: 微信小程序
tags: 小程序
author: ywyan
---

* content
{:toc}



*最近做微信小程序项目中遇到根据后台接口获取城市某个区域内的信息，后台接口要求传入城市的区域名称，例如上海市杨浦区，小程序官方地址提供的API只能获取到用户当前的经纬度，如何通过经纬度查询到用户的当前位置成了一个问题。所以通过研究和查询资料解决了这个问题，现共享给大家。*

我是通过腾讯地图逆地址解析，在通过经纬度获取详细的位置信息数据。

根据腾讯地图API，以图文的方式说明如何获取详细的位置信息数据。具体参考腾讯地图Webservice API的介绍。地址：[腾讯位置服务](http://link.zhihu.com/?target=http%3A//lbs.qq.com/webservice_v1/guide-geocoder.html)

#### step1:申请腾讯地图密钥（key），申请地址：[申请密钥](http://link.zhihu.com/?target=http%3A//lbs.qq.com/console/key.html)

填写完成后即可获取到对应的key值。

 ![申请腾讯地图密钥.png](http://upload-images.jianshu.io/upload_images/4041074-9bac7902d91a41fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### step2:通过小程序官方API获取用户当前位置经纬度。然后根据腾讯Webservice API逆地址解析相关介绍，传入获取到的经纬度，即可获取。

示例代码：

```
//获取当前位置经纬度
    wx.getLocation({
      type: 'wgs84',
      success: function (res) {
        //console.log("获取当前经纬度：" + JSON.stringify(res));
        //发送请求通过经纬度反查地址信息  
        var getAddressUrl = "https://apis.map.qq.com/ws/geocoder/v1/?location=" + res.latitude + "," + res.longitude + "&key=你的key值&get_poi=1";
        common.Request(getAddressUrl, "get", "", function (ops) {
          //console.log(JSON.stringify(ops)); 
        })
      }
    }) 
```
![获取当前经纬度.jpg](http://upload-images.jianshu.io/upload_images/4041074-5b9aedfd9fb43821.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

获取的位置示例，根据项目提取需要的数据。
```
{
    "status": 0,
    "message": "query ok",
    "request_id": "7e11ac8e-f763-11e7-b568-6c92bf3a15eb",
    "result": {
        "location": {
            "lat": 39.984154,
            "lng": 116.30749
        },
        "address": "北京市海淀区北四环西路66号",
        "formatted_addresses": {
            "recommend": "海淀区中国技术交易大厦(左岸工社东)",
            "rough": "海淀区中国技术交易大厦(左岸工社东)"
        },
        "address_component": {
            "nation": "中国",
            "province": "北京市",
            "city": "北京市",
            "district": "海淀区",
            "street": "北四环西路",
            "street_number": "北四环西路66号"
        },
        "ad_info": {
            "nation_code": "156",
            "adcode": "110108",
            "city_code": "156110000",
            "name": "中国,北京市,北京市,海淀区",
            "location": {
                "lat": 39.984154,
                "lng": 116.307487
            },
            "nation": "中国",
            "province": "北京市",
            "city": "北京市",
            "district": "海淀区"
        },
        "address_reference": {
            "business_area": {
                "title": "中关村",
                "location": {
                    "lat": 39.984089,
                    "lng": 116.307564
                },
                "_distance": 0,
                "_dir_desc": "内"
            },
            "famous_area": {
                "title": "中关村",
                "location": {
                    "lat": 39.984089,
                    "lng": 116.307564
                },
                "_distance": 0,
                "_dir_desc": "内"
            },
            "crossroad": {
                "title": "彩和坊路/北四环西路辅路(路口)",
                "location": {
                    "lat": 39.985001,
                    "lng": 116.308113
                },
                "_distance": 102.8,
                "_dir_desc": "西南"
            },
            "town": {
                "title": "海淀街道",
                "location": {
                    "lat": 39.984154,
                    "lng": 116.307487
                },
                "_distance": 0,
                "_dir_desc": "内"
            },
            "street_number": {
                "title": "北四环西路66号",
                "location": {
                    "lat": 39.984119,
                    "lng": 116.307503
                },
                "_distance": 6.2,
                "_dir_desc": ""
            },
            "street": {
                "title": "彩和坊路",
                "location": {
                    "lat": 39.984169,
                    "lng": 116.308098
                },
                "_distance": 46.6,
                "_dir_desc": "西"
            },
            "landmark_l1": {
                "title": "左岸工社",
                "location": {
                    "lat": 39.984112,
                    "lng": 116.30439
                },
                "_distance": 176,
                "_dir_desc": "东"
            },
            "landmark_l2": {
                "title": "中国技术交易大厦A座",
                "location": {
                    "lat": 39.984329,
                    "lng": 116.307419
                },
                "_distance": 20.4,
                "_dir_desc": ""
            }
        },
        "poi_count": 10,
        "pois": [
            {
                "id": "2845372667492951071",
                "title": "中国技术交易大厦A座",
                "address": "北京市海淀区北四环西路66号",
                "category": "房产小区:商务楼宇",
                "location": {
                    "lat": 39.984329,
                    "lng": 116.307419
                },
                "ad_info": {
                    "adcode": "110108",
                    "province": "北京市",
                    "city": "北京市",
                    "district": "海淀区"
                },
                "_distance": 20.4,
                "_dir_desc": ""
            },
            {
                "id": "11939717548889564206",
                "title": "中国技术交易大厦-西门",
                "address": "北京市海淀区北四环西路66号附近",
                "category": "室内及附属设施:通行设施类:门/出入口",
                "location": {
                    "lat": 39.98415,
                    "lng": 116.307281
                },
                "ad_info": {
                    "adcode": "110108",
                    "province": "北京市",
                    "city": "北京市",
                    "district": "海淀区"
                },
                "_distance": 17.6,
                "_dir_desc": ""
            },
            {
                "id": "13097787876388519900",
                "title": "中国技术交易大厦-东门",
                "address": "北京市海淀区彩和坊路与海淀北一街交叉口西北50米",
                "category": "室内及附属设施:通行设施类:门/出入口",
                "location": {
                    "lat": 39.984131,
                    "lng": 116.307716
                },
                "ad_info": {
                    "adcode": "110108",
                    "province": "北京市",
                    "city": "北京市",
                    "district": "海淀区"
                },
                "_distance": 19.7,
                "_dir_desc": ""
            },
            {
                "id": "12925244666643621769",
                "title": "中国技术交易大厦B座",
                "address": "北京市海淀区北四环西路66",
                "category": "房产小区:商务楼宇",
                "location": {
                    "lat": 39.984112,
                    "lng": 116.307587
                },
                "ad_info": {
                    "adcode": "110108",
                    "province": "北京市",
                    "city": "北京市",
                    "district": "海淀区"
                },
                "_distance": 9.7,
                "_dir_desc": ""
            },
            {
                "id": "3629720141162880123",
                "title": "中国技术交易大厦",
                "address": "北京市海淀区北四环西路66号",
                "category": "房产小区:商务楼宇",
                "location": {
                    "lat": 39.984089,
                    "lng": 116.307564
                },
                "ad_info": {
                    "adcode": "110108",
                    "province": "北京市",
                    "city": "北京市",
                    "district": "海淀区"
                },
                "_distance": 0,
                "_dir_desc": "内"
            },
            {
                "id": "9969038414753335812",
                "title": "腾讯科技(北京)有限公司(中国技术交易大厦)",
                "address": "北京市海淀区北四环西路66号中国技术交易大厦",
                "category": "公司企业:公司企业",
                "location": {
                    "lat": 39.984131,
                    "lng": 116.307503
                },
                "ad_info": {
                    "adcode": "110108",
                    "province": "北京市",
                    "city": "北京市",
                    "district": "海淀区"
                },
                "_distance": 2.9,
                "_dir_desc": ""
            },
            {
                "id": "12689244359326172642",
                "title": "车库咖啡",
                "address": "北京市海淀区中关村创业大街6号楼2层",
                "category": "娱乐休闲:咖啡厅",
                "location": {
                    "lat": 39.983898,
                    "lng": 116.306908
                },
                "ad_info": {
                    "adcode": "110108",
                    "province": "北京市",
                    "city": "北京市",
                    "district": "海淀区"
                },
                "_distance": 57.1,
                "_dir_desc": "东北"
            },
            {
                "id": "3187032738687555052",
                "title": "中关村创业大街",
                "address": "北京市海淀区海淀西大街",
                "category": "购物:商业步行街",
                "location": {
                    "lat": 39.984741,
                    "lng": 116.306519
                },
                "ad_info": {
                    "adcode": "110108",
                    "province": "北京市",
                    "city": "北京市",
                    "district": "海淀区"
                },
                "_distance": 43.9,
                "_dir_desc": "东北"
            },
            {
                "id": "7246616758286733108",
                "title": "基督教堂(彩和坊路)",
                "address": "北京市海淀区彩和坊路9号",
                "category": "旅游景点:教堂",
                "location": {
                    "lat": 39.983269,
                    "lng": 116.307648
                },
                "ad_info": {
                    "adcode": "110108",
                    "province": "北京市",
                    "city": "北京市",
                    "district": "海淀区"
                },
                "_distance": 69.5,
                "_dir_desc": "北"
            },
            {
                "id": "14510474916445262010",
                "title": "言几又(中关村店)",
                "address": "北京市海淀区海淀西大街48号",
                "category": "购物:图书音像",
                "location": {
                    "lat": 39.98373,
                    "lng": 116.30661
                },
                "ad_info": {
                    "adcode": "110108",
                    "province": "北京市",
                    "city": "北京市",
                    "district": "海淀区"
                },
                "_distance": 88.4,
                "_dir_desc": "东北"
            }
        ]
    }
}
```
在小程序里使用注意的地方：
- 需要在微信小程序后台配置合法域名；
- 测试时也可以在开发工具中选择不校验安全域名。

其它文章请访问：

*   [微信小程序之自定义组件](https://www.jianshu.com/p/5e7da13f1a3a)
*   [微信小程序之根据经纬度反查地址](https://www.jianshu.com/p/967159573f7f) 
*   [微信小程序之转发功能（附效果图和源码）](https://www.jianshu.com/p/c7b1925cd3c1)
*   [微信小程序之轮播图的实现（附效果图和源码）](https://www.jianshu.com/p/6bd93b2210f0)
*   [微信小程序之tab的实现（附源代码和效果图）](https://www.jianshu.com/p/416631c91520)
*   [微信小程序之多列表的显示和隐藏功能（附源码）](https://www.jianshu.com/p/d96a4ccbcce4)
