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


Q&A:
为什么选择将所有/12及以上改成/11，而不是将所有/13及以上改成/12或将所有/11及以上改成/10？
将所有/11及以上改成/10＝69条，IP范围扩大，误差扩大。
将所有/12及以上改成/11＝143条，200条之内的最优选择。
将所有/13及以上改成/12＝261条，超过了客户端限制的最高200条。
