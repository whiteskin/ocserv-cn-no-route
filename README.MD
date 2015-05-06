1、生成最新国内IP列表chnroute.txt

```
curl 'http://ftp.apnic.net/apnic/stats/apnic/delegated-apnic-latest' | grep ipv4 | grep CN | awk -F\| '{ printf("%s/%d\n", $4, 32-log($5)/log(2)) }' > chnroute.txt
```

2、合并chnroute.txt，生成chnroute_merged.txt

3、将所有/12及以上改成/11，并合并，生成11_143.txt

4、生成不含内网的cn-no-route.txt

5、添加内网IP并合并，生成cn-no-route2.txt


ocserv需要修改src/vpn.h来支持超过96行(ocserv默认值)但不超过200行(Cisco AnyConnect最大值)的路由表:

```
#define MAX_CONFIG_ENTRIES 96 
```

96改为200以上
