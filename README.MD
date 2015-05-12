# ocserv-cn-no-route

## 產生過程

1、生成最新国内IP列表chnroute.txt

```
curl 'http://ftp.apnic.net/apnic/stats/apnic/delegated-apnic-latest' | grep ipv4 | grep CN | awk -F\| '{ printf("%s/%d\n", $4, 32-log($5)/log(2)) }' > chnroute.txt
```

2、合并chnroute.txt(334490624 IPs)，生成chnroute_merged.txt(334490624 IPs)

3、将所有/12及以上改成/11，并合并，生成11_143.txt(713031680 IPs)

4、生成不含保留IP的cn-no-route.txt(712965888 IPs)

剔除11_143.txt中保留IP：192.168.0.0/16、203.0.113.0/24

5、添加保留IP并合并，生成cn-no-route2.txt(1305674752 IPs)


## 注意事項

ocserv需要修改src/vpn.h来支持超过96行(ocserv默认值)但不超过200行(Cisco AnyConnect最大值)的路由表:

```
#define MAX_CONFIG_ENTRIES 96 
```

96改为200以上

## Q&A

为什么选择将所有/12及以上改成/11，而不是将所有/13及以上改成/12或将所有/11及以上改成/10？

* 若將所有/13及以上改成/12，則不含保留IP的路由表是261條(580911104 IPs)，超過了客戶端限制的最高200條。
* 若將所有/12及以上改成/11，則不含保留IP的路由表是143條(713031680 IPs)。
* 若將所有/11及以上改成/10，則不含保留IP的路由表是69條(847249408 IPs)，IP範圍進一步擴大，誤差進一步擴大。

## [保留IP](http://en.wikipedia.org/wiki/Reserved_IP_addresses) (592708864 IPs)
* 0.0.0.0/8 
* 10.0.0.0/8 
* 100.64.0.0/10 
* 127.0.0.0/8 
* 169.254.0.0/16 
* 172.16.0.0/12 
* 192.0.0.0/24 
* 192.0.2.0/24 
* 192.88.99.0/24 
* 192.168.0.0/16 
* 198.18.0.0/15 
* 198.51.100.0/24 
* 203.0.113.0/24 
* 224.0.0.0/4 
* 240.0.0.0/4 
* 255.255.255.255/32

