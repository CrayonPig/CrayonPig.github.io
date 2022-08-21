---
title: 家庭NAS——DDNS篇
date: 2020.04.17 16:53
tags: [NAS, DDNS]
---
家庭NAS最大的一个问题之一就是公网IP不固定，这个问题的解决方法很多，如：
1.路由器刷固件；
2.群晖：白群晖自带内网穿透，黑群晖就放弃这个方案吧；
3.花生壳/花生棒：支持各类操作系统平台(路由器/Windows/Linux/群晖)，缺点是需要花钱；
4.阿里云DDNS：强烈推荐；
5.腾讯云DDNS：推荐，证书验证不如阿里云方便。
6.ngrok等内网传统方案

我买了黑群晖，家用路由器是小米的，所以对于我来说通过阿里云DDNS和腾讯云DDNS是最好的方案，我的域名是在阿里云上注册的，所以，我直接采用了阿里云DDNS方案。

思路是：每隔一定时间获取当前的公网ip，把阿里云的域名解析地址更改为获取到的公网ip


#获取公网IP
最直观的获取ip的方法是
![百度](https://upload-images.jianshu.io/upload_images/9056389-dc086f0d794b25bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
后续我们获取到ip后可以跟此ip对比，验证是否正确


node 获取ip方式有好多，我尝试了public-ip、http 请求等方法

```
// npm install --save public-ip
const publicIp = require('public-ip');

publicIp.v4().then(ip => {
    console.log(ip);
    //=> '46.5.21.123'
});
```
很有意思的是我利用public-ip这个库请求到的ip地址跟我百度的ip地址并不一样，这个问题我后续探究下

所以我直接使用http请求，获取到用户公网ip，代码如下
```
    const http = require('http')
    // 获取用户公网ip
    const url = 'http://txt.go.sohu.com/ip/soip'
    http.get(url, res => {
      let data = ''
      res.on('data', chunk => data += chunk)
      res.on('end', () => {
        let m = data.match(/\d+\.\d+\.\d+\.\d+/g)
        if (m.length > 0) {
          console.log('myIP', m[0])
        }
      })
    }).on('error', e => console.log(`error messsage: ${e.message}`))
```
这样获取到的ip地址跟百度出来的ip地址是一样的

# 修改阿里云域名解析
首先登录阿里云获取accessKeyId和accessKeySecret
[获取accessKeyId和accessKeySecret](https://usercenter.console.aliyun.com/#/manage/ak)
[域名解析文档](https://help.aliyun.com/document_detail/29776.html?spm=a2c4g.11186623.6.655.3a8449fcWwuU2C)
使用阿里云npm库@alicloud/pop-core进行调用
```
// npm install @alicloud/pop-core --save
const Core = require('@alicloud/pop-core');
var client = new Core({
        accessKeyId: 'xxxxxxxx',
        accessKeySecret:  'xxxxxxxx',
        endpoint: 'https://alidns.aliyuncs.com',
        apiVersion: '2015-01-09'
      });
```
通过文档我们可以发现，如果需要修改解析地址，我们需要用到被修改解析记录的RecordId，
所以我们先获取到RecordId
```
var params = {
      // 参数可根据自己需求去增加
      "RegionId": "cn-hangzhou",
      "DomainName": "baidu.com",
      "KeyWord": "A"
    }
    
    var requestOption = {
      method: 'POST'
    };
    
    client.request('DescribeDomainRecords', params, requestOption).then((res) => {
      console.log(JSON.stringify(res));
      if(res.DomainRecords.Record) {
        // 我修改的是根域名，所以筛选了下
        let recordList = res.DomainRecords.Record.filter(item => {
          return item.Type === 'A'
        })
        let recordID = recordList[0].RecordId
        // 此处获取到的就是RecordId，可根据自己需求修改逻辑
      } else {
        console.log('DomainRecords record is null')
      }
    }, (ex) => {
      console.log(ex);
    })
```
根据获取到的RecordId将获取到的公网ip赋值
```
var params = {
      "RegionId": "cn-hangzhou",
      "RecordId": recordID,
      "RR": "@",
      "Type": "A",
      "Value": myIP
    }
    
    var requestOption = {
      method: 'POST'
    };
    
    this.client.request('UpdateDomainRecord', params, requestOption).then((result) => {
      // console.log(JSON.stringify(result));
      console.log('修改成功，公网IP为', myIP)
    }, (ex) => {
      console.log(ex);
    })
```

以上就是修改ip的全过程，后续我们再利用node-schedule做个定时任务就可以了，整体代码如下
```
const http = require('http')
const Core = require('@alicloud/pop-core');
const schedule = require('node-schedule');

class AliDDNS {
  constructor() {
    this.myIP = ''
    this.accessKeyId = ''
    this.accessKeySecret = ''
    this.domainName = ''
    this.KeyWord = '@'
  }

  init () {
    this.getIP()
    this.initCore()
    this.getDomainNameID()
  }

  getIP () {
    // 获取用户公网ip
    const url = 'http://txt.go.sohu.com/ip/soip'
    http.get(url, res => {
      let data = ''
      res.on('data', chunk => data += chunk)
      res.on('end', () => {
        let m = data.match(/\d+\.\d+\.\d+\.\d+/g)
        if (m.length > 0) {
          this.myIP = m[0]
          console.log('myIP', this.myIP)
        }
      })
    }).on('error', e => console.log(`error messsage: ${e.message}`))
  }

  initCore() {
      // 初始化 阿里云Core
      this.client = new Core({
        accessKeyId: this.accessKeyId,
        accessKeySecret: this.accessKeySecret,
        endpoint: 'https://alidns.aliyuncs.com',
        apiVersion: '2015-01-09'
      });
  }
  getDomainNameID() {
    if(!this.client) {
      console.error('client no init')
      return false
    }
    var params = {
      "RegionId": "cn-hangzhou",
      "DomainName": this.domainName,
      "KeyWord": this.KeyWord,
    }
    
    var requestOption = {
      method: 'POST'
    };
    
    this.client.request('DescribeDomainRecords', params, requestOption).then((res) => {
      console.log(JSON.stringify(res));
      if(res.DomainRecords.Record) {
        let recordList = res.DomainRecords.Record.filter(item => {
          return item.Type === 'A'
        })
        let recordID = recordList[0].RecordId
        this.changeDomainIP(recordID)
      } else {
        console.log('DomainRecords record is null')
      }
    }, (ex) => {
      console.log(ex);
    })
  }

  changeDomainIP(recordID) {
    var params = {
      "RegionId": "cn-hangzhou",
      "RecordId": recordID,
      "RR": "@",
      "Type": "A",
      "Value": this.myIP
    }
    
    var requestOption = {
      method: 'POST'
    };
    
    this.client.request('UpdateDomainRecord', params, requestOption).then((result) => {
      // console.log(JSON.stringify(result));
      console.log('修改成功，公网IP为', this.myIP)
    }, (ex) => {
      console.log(ex);
    })
  }
}

const aliDDns = new AliDDNS()

// 每五分钟更新一次ip
schedule.scheduleJob('0 5 * * * *', ()=>{
  aliDDns.init()
})

```


