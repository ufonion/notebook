# Clash

```yaml:docker-compose.yml
version: '3'
services:
  clash:
    image: dreamacro/clash
    container_name: clash
    volumes:
      - ./config.yaml:/root/.config/clash/config.yaml
      - ./ui:/root/.config/clash/ui # dashboard volume
    ports:
      - "127.0.0.1:7890:7890"
      - "127.0.0.1:7891:7891"
      - "127.0.0.1:9090:9090" # external controller (Restful API)
    restart: unless-stopped
    network_mode: "bridge" # or "host" on Linux
```

```yaml:config.yaml
# Port of HTTP(S) proxy server on the local end
port: 7890

# Port of SOCKS5 proxy server on the local end
socks-port: 7891

# Transparent proxy server port for Linux and macOS (Redirect TCP and TProxy UDP)
# redir-port: 7892

# Transparent proxy server port for Linux (TProxy TCP and TProxy UDP)
# tproxy-port: 7893

# HTTP(S) and SOCKS5 server on the same port
# mixed-port: 7890

# authentication of local SOCKS5/HTTP(S) server
# authentication:
#  - "user1:pass1"
#  - "user2:pass2"

# Set to true to allow connections to the local-end server from
# other LAN IP addresses
allow-lan: true

# This is only applicable when `allow-lan` is `true`
# '*': bind all IP addresses
# 192.168.122.11: bind a single IPv4 address
# "[aaaa::a8aa:ff:fe09:57d8]": bind a single IPv6 address
bind-address: '*'

# Clash router working mode
# rule: rule-based packet routing
# global: all packets will be forwarded to a single endpoint
# direct: directly forward the packets to the Internet
mode: rule

# Clash by default prints logs to STDOUT
# info / warning / error / debug / silent
log-level: info
# When set to false, resolver won't translate hostnames to IPv6 addresses
ipv6: false


# RESTful web API listening address
external-controller: 0.0.0.0:9090

# A relative path to the configuration directory or an absolute path to a
# directory in which you put some static web resource. Clash core will then
# serve it at `http://{{external-controller}}/ui`.
external-ui: ui

# Secret for the RESTful API (optional)
# Authenticate by spedifying HTTP header `Authorization: Bearer ${secret}`
# ALWAYS set a secret if RESTful API is listening on 0.0.0.0
secret: "SECRET"

# Outbound interface name
# interface-name: en0

# Static hosts for DNS server and connection establishment (like /etc/hosts)
#
# Wildcard hostnames are supported (e.g. *.clash.dev, *.foo.*.example.com)
# Non-wildcard domain names have a higher priority than wildcard domain names
# e.g. foo.example.com > *.example.com > .example.com
# P.S. +.foo.com equals to .foo.com and foo.com
# hosts:
  # '*.clash.dev': 127.0.0.1
  # '.dev': 127.0.0.1
  # 'alpha.clash.dev': '::1'

# clash for windows only
# cfw-bypass:
#   - localhost
#   - 127.*
#   - 10.*
#   - 172.16.*
#   - 172.17.*
#   - 172.18.*
#   - 172.19.*
#   - 172.20.*
#   - 172.21.*
#   - 172.22.*
#   - 172.23.*
#   - 172.24.*
#   - 172.25.*
#   - 172.26.*
#   - 172.27.*
#   - 172.28.*
#   - 172.29.*
#   - 172.30.*
#   - 172.31.*
#   - 192.168.*
#   - <local>
# cfw-latency-timeout: 3000
# cfw-latency-url: http://www.gstatic.com/generate_204
# cfw-conn-break-strategy:
#   proxy: none
#   profile: true
#   mode: false
# cfw-child-process: []
# cfw-proxies-order: default

proxies:
  - name: aws-us.mayongcong.top
    type: vmess
    server: aws-us.mayongcong.top
    port: "443"
    uuid: UUID
    alterId: "10"
    cipher: auto
    tls: true
    network: ws
    ws-path: /path/
proxy-groups:
  - name: ActiveProxy
    proxies:
      - aws-us.mayongcong.top
    type: load-balance
    url: http://www.gstatic.com/generate_204
    interval: 600
  - name: UrlTest
    proxies:
      - aws-us.mayongcong.top
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 600
  - name: Select
    proxies:
      - aws-us.mayongcong.top
    type: select
rules:
  # 国内网站
  - DOMAIN-SUFFIX,cn,DIRECT
  - DOMAIN-KEYWORD,-cn,DIRECT
  - DOMAIN-SUFFIX,126.com,DIRECT
  - DOMAIN-SUFFIX,126.net,DIRECT
  - DOMAIN-SUFFIX,127.net,DIRECT
  - DOMAIN-SUFFIX,163.com,DIRECT
  - DOMAIN-SUFFIX,360buyimg.com,DIRECT
  - DOMAIN-SUFFIX,36kr.com,DIRECT
  - DOMAIN-SUFFIX,acfun.tv,DIRECT
  - DOMAIN-SUFFIX,air-matters.com,DIRECT
  - DOMAIN-SUFFIX,aixifan.com,DIRECT
  - DOMAIN-SUFFIX,akamaized.net,DIRECT
  - DOMAIN-KEYWORD,alicdn,DIRECT
  - DOMAIN-KEYWORD,alipay,DIRECT
  - DOMAIN-KEYWORD,taobao,DIRECT
  - DOMAIN-SUFFIX,amap.com,DIRECT
  - DOMAIN-SUFFIX,autonavi.com,DIRECT
  - DOMAIN-KEYWORD,baidu,DIRECT
  - DOMAIN-SUFFIX,bdimg.com,DIRECT
  - DOMAIN-SUFFIX,bdstatic.com,DIRECT
  - DOMAIN-SUFFIX,bilibili.com,DIRECT
  - DOMAIN-SUFFIX,bilivideo.com,DIRECT
  - DOMAIN-SUFFIX,caiyunapp.com,DIRECT
  - DOMAIN-SUFFIX,clouddn.com,DIRECT
  - DOMAIN-SUFFIX,cnbeta.com,DIRECT
  - DOMAIN-SUFFIX,cnbetacdn.com,DIRECT
  - DOMAIN-SUFFIX,cootekservice.com,DIRECT
  - DOMAIN-SUFFIX,csdn.net,DIRECT
  - DOMAIN-SUFFIX,ctrip.com,DIRECT
  - DOMAIN-SUFFIX,dgtle.com,DIRECT
  - DOMAIN-SUFFIX,dianping.com,DIRECT
  - DOMAIN-SUFFIX,douban.com,DIRECT
  - DOMAIN-SUFFIX,doubanio.com,DIRECT
  - DOMAIN-SUFFIX,duokan.com,DIRECT
  - DOMAIN-SUFFIX,easou.com,DIRECT
  - DOMAIN-SUFFIX,ele.me,DIRECT
  - DOMAIN-SUFFIX,feng.com,DIRECT
  - DOMAIN-SUFFIX,fir.im,DIRECT
  - DOMAIN-SUFFIX,frdic.com,DIRECT
  - DOMAIN-SUFFIX,g-cores.com,DIRECT
  - DOMAIN-SUFFIX,godic.net,DIRECT
  - DOMAIN-SUFFIX,gtimg.com,DIRECT
  - DOMAIN,cdn.hockeyapp.net,DIRECT
  - DOMAIN-SUFFIX,hongxiu.com,DIRECT
  - DOMAIN-SUFFIX,hxcdn.net,DIRECT
  - DOMAIN-SUFFIX,iciba.com,DIRECT
  - DOMAIN-SUFFIX,ifeng.com,DIRECT
  - DOMAIN-SUFFIX,ifengimg.com,DIRECT
  - DOMAIN-SUFFIX,ipip.net,DIRECT
  - DOMAIN-SUFFIX,iqiyi.com,DIRECT
  - DOMAIN-SUFFIX,jd.com,DIRECT
  - DOMAIN-SUFFIX,jianshu.com,DIRECT
  - DOMAIN-SUFFIX,knewone.com,DIRECT
  - DOMAIN-SUFFIX,le.com,DIRECT
  - DOMAIN-SUFFIX,lecloud.com,DIRECT
  - DOMAIN-SUFFIX,lemicp.com,DIRECT
  - DOMAIN-SUFFIX,licdn.com,DIRECT
  - DOMAIN-SUFFIX,linkedin.com,DIRECT
  - DOMAIN-SUFFIX,luoo.net,DIRECT
  - DOMAIN-SUFFIX,meituan.com,DIRECT
  - DOMAIN-SUFFIX,meituan.net,DIRECT
  - DOMAIN-SUFFIX,mi.com,DIRECT
  - DOMAIN-SUFFIX,miaopai.com,DIRECT
  - DOMAIN-SUFFIX,microsoft.com,ActiveProxy
  - DOMAIN-SUFFIX,microsoftonline.com,DIRECT
  - DOMAIN-SUFFIX,miui.com,DIRECT
  - DOMAIN-SUFFIX,miwifi.com,DIRECT
  - DOMAIN-SUFFIX,mob.com,DIRECT
  - DOMAIN-SUFFIX,netease.com,DIRECT
  - DOMAIN-SUFFIX,office.com,DIRECT
  - DOMAIN-SUFFIX,office365.com,DIRECT
  - DOMAIN-KEYWORD,officecdn,DIRECT
  - DOMAIN-SUFFIX,oschina.net,DIRECT
  - DOMAIN-SUFFIX,ppsimg.com,DIRECT
  - DOMAIN-SUFFIX,pstatp.com,DIRECT
  - DOMAIN-SUFFIX,qcloud.com,DIRECT
  - DOMAIN-SUFFIX,qdaily.com,DIRECT
  - DOMAIN-SUFFIX,qdmm.com,DIRECT
  - DOMAIN-SUFFIX,qhimg.com,DIRECT
  - DOMAIN-SUFFIX,qhres.com,DIRECT
  - DOMAIN-SUFFIX,qidian.com,DIRECT
  - DOMAIN-SUFFIX,qihucdn.com,DIRECT
  - DOMAIN-SUFFIX,qiniu.com,DIRECT
  - DOMAIN-SUFFIX,qiniucdn.com,DIRECT
  - DOMAIN-SUFFIX,qiyipic.com,DIRECT
  - DOMAIN-SUFFIX,qq.com,DIRECT
  - DOMAIN-SUFFIX,qqurl.com,DIRECT
  - DOMAIN-SUFFIX,rarbg.to,DIRECT
  - DOMAIN-SUFFIX,ruguoapp.com,DIRECT
  - DOMAIN-SUFFIX,segmentfault.com,DIRECT
  - DOMAIN-SUFFIX,sinaapp.com,DIRECT
  - DOMAIN-SUFFIX,smzdm.com,DIRECT
  - DOMAIN-SUFFIX,snapdrop.net,DIRECT
  - DOMAIN-SUFFIX,sogou.com,DIRECT
  - DOMAIN-SUFFIX,sogoucdn.com,DIRECT
  - DOMAIN-SUFFIX,sohu.com,DIRECT
  - DOMAIN-SUFFIX,soku.com,DIRECT
  - DOMAIN-SUFFIX,speedtest.net,DIRECT
  - DOMAIN-SUFFIX,sspai.com,DIRECT
  - DOMAIN-SUFFIX,suning.com,DIRECT
  - DOMAIN-SUFFIX,taobao.com,DIRECT
  - DOMAIN-SUFFIX,tencent.com,DIRECT
  - DOMAIN-SUFFIX,tenpay.com,DIRECT
  - DOMAIN-SUFFIX,tianyancha.com,DIRECT
  - DOMAIN-SUFFIX,tmall.com,DIRECT
  - DOMAIN-SUFFIX,tudou.com,DIRECT
  - DOMAIN-SUFFIX,umetrip.com,DIRECT
  - DOMAIN-SUFFIX,upaiyun.com,DIRECT
  - DOMAIN-SUFFIX,upyun.com,DIRECT
  - DOMAIN-SUFFIX,veryzhun.com,DIRECT
  - DOMAIN-SUFFIX,weather.com,DIRECT
  - DOMAIN-SUFFIX,weibo.com,DIRECT
  - DOMAIN-SUFFIX,xiami.com,DIRECT
  - DOMAIN-SUFFIX,xiami.net,DIRECT
  - DOMAIN-SUFFIX,xiaomicp.com,DIRECT
  - DOMAIN-SUFFIX,ximalaya.com,DIRECT
  - DOMAIN-SUFFIX,xmcdn.com,DIRECT
  - DOMAIN-SUFFIX,xunlei.com,DIRECT
  - DOMAIN-SUFFIX,yhd.com,DIRECT
  - DOMAIN-SUFFIX,yihaodianimg.com,DIRECT
  - DOMAIN-SUFFIX,yinxiang.com,DIRECT
  - DOMAIN-SUFFIX,ykimg.com,DIRECT
  - DOMAIN-SUFFIX,youdao.com,DIRECT
  - DOMAIN-SUFFIX,youku.com,DIRECT
  - DOMAIN-SUFFIX,zealer.com,DIRECT
  - DOMAIN-SUFFIX,zhihu.com,DIRECT
  - DOMAIN-SUFFIX,zhimg.com,DIRECT
  - DOMAIN-SUFFIX,zimuzu.tv,DIRECT
  - DOMAIN-SUFFIX,zoho.com,DIRECT
# 抗 DNS 污染 
  - DOMAIN-KEYWORD,amazon,ActiveProxy
  - DOMAIN-KEYWORD,google,ActiveProxy
  - DOMAIN-KEYWORD,gmail,ActiveProxy
  - DOMAIN-KEYWORD,youtube,ActiveProxy
  - DOMAIN-KEYWORD,facebook,ActiveProxy
  - DOMAIN-SUFFIX,fb.me,ActiveProxy
  - DOMAIN-SUFFIX,fbcdn.net,ActiveProxy
  - DOMAIN-KEYWORD,twitter,ActiveProxy
  - DOMAIN-KEYWORD,instagram,ActiveProxy
  - DOMAIN-KEYWORD,dropbox,ActiveProxy
  - DOMAIN-SUFFIX,twimg.com,ActiveProxy
  - DOMAIN-KEYWORD,blogspot,ActiveProxy
  - DOMAIN-SUFFIX,youtu.be,ActiveProxy
  - DOMAIN-KEYWORD,whatsapp,ActiveProxy
# 常见广告域名屏蔽
  - DOMAIN-KEYWORD,admarvel,REJECT
  - DOMAIN-KEYWORD,admaster,REJECT
  - DOMAIN-KEYWORD,adsage,REJECT
  - DOMAIN-KEYWORD,adsmogo,REJECT
  - DOMAIN-KEYWORD,adsrvmedia,REJECT
  - DOMAIN-KEYWORD,adwords,REJECT
  - DOMAIN-KEYWORD,adservice,REJECT
  - DOMAIN-KEYWORD,domob,REJECT
  - DOMAIN-KEYWORD,duomeng,REJECT
  - DOMAIN-KEYWORD,dwtrack,REJECT
  - DOMAIN-KEYWORD,guanggao,REJECT
  - DOMAIN-KEYWORD,lianmeng,REJECT
  - DOMAIN-SUFFIX,mmstat.com,REJECT
  - DOMAIN-KEYWORD,omgmta,REJECT
  - DOMAIN-KEYWORD,openx,REJECT
  - DOMAIN-KEYWORD,partnerad,REJECT
  - DOMAIN-KEYWORD,pingfore,REJECT
  - DOMAIN-KEYWORD,supersonicads,REJECT
  - DOMAIN-KEYWORD,uedas,REJECT
  - DOMAIN-KEYWORD,umeng,REJECT
  - DOMAIN-KEYWORD,usage,REJECT
  - DOMAIN-KEYWORD,wlmonitor,REJECT
  - DOMAIN-KEYWORD,zjtoolbar,REJECT
# 国外网站
  - DOMAIN-SUFFIX,9to5mac.com,ActiveProxy
  - DOMAIN-SUFFIX,abpchina.org,ActiveProxy
  - DOMAIN-SUFFIX,adblockplus.org,ActiveProxy
  - DOMAIN-SUFFIX,adobe.com,ActiveProxy
  - DOMAIN-SUFFIX,alfredapp.com,ActiveProxy
  - DOMAIN-SUFFIX,amplitude.com,ActiveProxy
  - DOMAIN-SUFFIX,ampproject.org,ActiveProxy
  - DOMAIN-SUFFIX,android.com,ActiveProxy
  - DOMAIN-SUFFIX,angularjs.org,ActiveProxy
  - DOMAIN-SUFFIX,aolcdn.com,ActiveProxy
  - DOMAIN-SUFFIX,apkpure.com,ActiveProxy
  - DOMAIN-SUFFIX,appledaily.com,ActiveProxy
  - DOMAIN-SUFFIX,appshopper.com,ActiveProxy
  - DOMAIN-SUFFIX,appspot.com,ActiveProxy
  - DOMAIN-SUFFIX,arcgis.com,ActiveProxy
  - DOMAIN-SUFFIX,archive.org,ActiveProxy
  - DOMAIN-SUFFIX,armorgames.com,ActiveProxy
  - DOMAIN-SUFFIX,aspnetcdn.com,ActiveProxy
  - DOMAIN-SUFFIX,att.com,ActiveProxy
  - DOMAIN-SUFFIX,awsstatic.com,ActiveProxy
  - DOMAIN-SUFFIX,azureedge.net,ActiveProxy
  - DOMAIN-SUFFIX,azurewebsites.net,ActiveProxy
  - DOMAIN-SUFFIX,bing.com,ActiveProxy
  - DOMAIN-SUFFIX,bintray.com,ActiveProxy
  - DOMAIN-SUFFIX,bit.com,ActiveProxy
  - DOMAIN-SUFFIX,bit.ly,ActiveProxy
  - DOMAIN-SUFFIX,bitbucket.org,ActiveProxy
  - DOMAIN-SUFFIX,bjango.com,ActiveProxy
  - DOMAIN-SUFFIX,bkrtx.com,ActiveProxy
  - DOMAIN-SUFFIX,blog.com,ActiveProxy
  - DOMAIN-SUFFIX,blogcdn.com,ActiveProxy
  - DOMAIN-SUFFIX,blogger.com,ActiveProxy
  - DOMAIN-SUFFIX,blogsmithmedia.com,ActiveProxy
  - DOMAIN-SUFFIX,blogspot.com,ActiveProxy
  - DOMAIN-SUFFIX,blogspot.hk,ActiveProxy
  - DOMAIN-SUFFIX,bloomberg.com,ActiveProxy
  - DOMAIN-SUFFIX,box.com,ActiveProxy
  - DOMAIN-SUFFIX,box.net,ActiveProxy
  - DOMAIN-SUFFIX,cachefly.net,ActiveProxy
  - DOMAIN-SUFFIX,chromium.org,ActiveProxy
  - DOMAIN-SUFFIX,cl.ly,ActiveProxy
  - DOMAIN-SUFFIX,cloudflare.com,ActiveProxy
  - DOMAIN-SUFFIX,cloudfront.net,ActiveProxy
  - DOMAIN-SUFFIX,cloudmagic.com,ActiveProxy
  - DOMAIN-SUFFIX,cmail19.com,ActiveProxy
  - DOMAIN-SUFFIX,cnet.com,ActiveProxy
  - DOMAIN-SUFFIX,cocoapods.org,ActiveProxy
  - DOMAIN-SUFFIX,comodoca.com,ActiveProxy
  - DOMAIN-SUFFIX,crashlytics.com,ActiveProxy
  - DOMAIN-SUFFIX,culturedcode.com,ActiveProxy
  - DOMAIN-SUFFIX,d.pr,ActiveProxy
  - DOMAIN-SUFFIX,danilo.to,ActiveProxy
  - DOMAIN-SUFFIX,dayone.me,ActiveProxy
  - DOMAIN-SUFFIX,db.tt,ActiveProxy
  - DOMAIN-SUFFIX,deskconnect.com,ActiveProxy
  - DOMAIN-SUFFIX,disq.us,ActiveProxy
  - DOMAIN-SUFFIX,disqus.com,ActiveProxy
  - DOMAIN-SUFFIX,disquscdn.com,ActiveProxy
  - DOMAIN-SUFFIX,dnsimple.com,ActiveProxy
  - DOMAIN-SUFFIX,docker.com,ActiveProxy
  - DOMAIN-SUFFIX,dribbble.com,ActiveProxy
  - DOMAIN-SUFFIX,droplr.com,ActiveProxy
  - DOMAIN-SUFFIX,duckduckgo.com,ActiveProxy
  - DOMAIN-SUFFIX,dueapp.com,ActiveProxy
  - DOMAIN-SUFFIX,dytt8.net,ActiveProxy
  - DOMAIN-SUFFIX,edgecastcdn.net,ActiveProxy
  - DOMAIN-SUFFIX,edgekey.net,ActiveProxy
  - DOMAIN-SUFFIX,edgesuite.net,ActiveProxy
  - DOMAIN-SUFFIX,engadget.com,ActiveProxy
  - DOMAIN-SUFFIX,entrust.net,ActiveProxy
  - DOMAIN-SUFFIX,eurekavpt.com,ActiveProxy
  - DOMAIN-SUFFIX,evernote.com,ActiveProxy
  - DOMAIN-SUFFIX,fabric.io,ActiveProxy
  - DOMAIN-SUFFIX,fast.com,ActiveProxy
  - DOMAIN-SUFFIX,fastly.net,ActiveProxy
  - DOMAIN-SUFFIX,fc2.com,ActiveProxy
  - DOMAIN-SUFFIX,feedburner.com,ActiveProxy
  - DOMAIN-SUFFIX,feedly.com,ActiveProxy
  - DOMAIN-SUFFIX,feedsportal.com,ActiveProxy
  - DOMAIN-SUFFIX,fiftythree.com,ActiveProxy
  - DOMAIN-SUFFIX,firebaseio.com,ActiveProxy
  - DOMAIN-SUFFIX,flexibits.com,ActiveProxy
  - DOMAIN-SUFFIX,flickr.com,ActiveProxy
  - DOMAIN-SUFFIX,flipboard.com,ActiveProxy
  - DOMAIN-SUFFIX,g.co,ActiveProxy
  - DOMAIN-SUFFIX,gabia.net,ActiveProxy
  - DOMAIN-SUFFIX,geni.us,ActiveProxy
  - DOMAIN-SUFFIX,gfx.ms,ActiveProxy
  - DOMAIN-SUFFIX,ggpht.com,ActiveProxy
  - DOMAIN-SUFFIX,ghostnoteapp.com,ActiveProxy
  - DOMAIN-SUFFIX,git.io,ActiveProxy
  - DOMAIN-KEYWORD,github,ActiveProxy
  - DOMAIN-SUFFIX,globalsign.com,ActiveProxy
  - DOMAIN-SUFFIX,gmodules.com,ActiveProxy
  - DOMAIN-SUFFIX,godaddy.com,ActiveProxy
  - DOMAIN-SUFFIX,golang.org,ActiveProxy
  - DOMAIN-SUFFIX,gongm.in,ActiveProxy
  - DOMAIN-SUFFIX,goo.gl,ActiveProxy
  - DOMAIN-SUFFIX,goodreaders.com,ActiveProxy
  - DOMAIN-SUFFIX,goodreads.com,ActiveProxy
  - DOMAIN-SUFFIX,gravatar.com,ActiveProxy
  - DOMAIN-SUFFIX,gstatic.com,ActiveProxy
  - DOMAIN-SUFFIX,gvt0.com,ActiveProxy
  - DOMAIN-SUFFIX,hockeyapp.net,ActiveProxy
  - DOMAIN-SUFFIX,hotmail.com,ActiveProxy
  - DOMAIN-SUFFIX,icons8.com,ActiveProxy
  - DOMAIN-SUFFIX,ifixit.com,ActiveProxy
  - DOMAIN-SUFFIX,ift.tt,ActiveProxy
  - DOMAIN-SUFFIX,ifttt.com,ActiveProxy
  - DOMAIN-SUFFIX,iherb.com,ActiveProxy
  - DOMAIN-SUFFIX,imageshack.us,ActiveProxy
  - DOMAIN-SUFFIX,img.ly,ActiveProxy
  - DOMAIN-SUFFIX,imgur.com,ActiveProxy
  - DOMAIN-SUFFIX,imore.com,ActiveProxy
  - DOMAIN-SUFFIX,instapaper.com,ActiveProxy
  - DOMAIN-SUFFIX,ipn.li,ActiveProxy
  - DOMAIN-SUFFIX,is.gd,ActiveProxy
  - DOMAIN-SUFFIX,issuu.com,ActiveProxy
  - DOMAIN-SUFFIX,itgonglun.com,ActiveProxy
  - DOMAIN-SUFFIX,itun.es,ActiveProxy
  - DOMAIN-SUFFIX,ixquick.com,ActiveProxy
  - DOMAIN-SUFFIX,j.mp,ActiveProxy
  - DOMAIN-SUFFIX,js.revsci.net,ActiveProxy
  - DOMAIN-SUFFIX,jshint.com,ActiveProxy
  - DOMAIN-SUFFIX,jtvnw.net,ActiveProxy
  - DOMAIN-SUFFIX,justgetflux.com,ActiveProxy
  - DOMAIN-SUFFIX,kat.cr,ActiveProxy
  - DOMAIN-SUFFIX,klip.me,ActiveProxy
  - DOMAIN-SUFFIX,libsyn.com,ActiveProxy
  - DOMAIN-SUFFIX,linode.com,ActiveProxy
  - DOMAIN-SUFFIX,lithium.com,ActiveProxy
  - DOMAIN-SUFFIX,littlehj.com,ActiveProxy
  - DOMAIN-SUFFIX,live.com,ActiveProxy
  - DOMAIN-SUFFIX,live.net,ActiveProxy
  - DOMAIN-SUFFIX,livefilestore.com,ActiveProxy
  - DOMAIN-SUFFIX,llnwd.net,ActiveProxy
  - DOMAIN-SUFFIX,macid.co,ActiveProxy
  - DOMAIN-SUFFIX,macromedia.com,ActiveProxy
  - DOMAIN-SUFFIX,macrumors.com,ActiveProxy
  - DOMAIN-SUFFIX,mashable.com,ActiveProxy
  - DOMAIN-SUFFIX,mathjax.org,ActiveProxy
  - DOMAIN-SUFFIX,medium.com,ActiveProxy
  - DOMAIN-SUFFIX,mega.co.nz,ActiveProxy
  - DOMAIN-SUFFIX,mega.nz,ActiveProxy
  - DOMAIN-SUFFIX,megaupload.com,ActiveProxy
  - DOMAIN-SUFFIX,microsofttranslator.com,ActiveProxy
  - DOMAIN-SUFFIX,mindnode.com,ActiveProxy
  - DOMAIN-SUFFIX,mobile01.com,ActiveProxy
  - DOMAIN-SUFFIX,modmyi.com,ActiveProxy
  - DOMAIN-SUFFIX,msedge.net,ActiveProxy
  - DOMAIN-SUFFIX,myfontastic.com,ActiveProxy
  - DOMAIN-SUFFIX,name.com,ActiveProxy
  - DOMAIN-SUFFIX,nextmedia.com,ActiveProxy
  - DOMAIN-SUFFIX,nsstatic.net,ActiveProxy
  - DOMAIN-SUFFIX,nssurge.com,ActiveProxy
  - DOMAIN-SUFFIX,nyt.com,ActiveProxy
  - DOMAIN-SUFFIX,nytimes.com,ActiveProxy
  - DOMAIN-SUFFIX,omnigroup.com,ActiveProxy
  - DOMAIN-SUFFIX,onedrive.com,ActiveProxy
  - DOMAIN-SUFFIX,onenote.com,ActiveProxy
  - DOMAIN-SUFFIX,ooyala.com,ActiveProxy
  - DOMAIN-SUFFIX,openvpn.net,ActiveProxy
  - DOMAIN-SUFFIX,openwrt.org,ActiveProxy
  - DOMAIN-SUFFIX,orkut.com,ActiveProxy
  - DOMAIN-SUFFIX,osxdaily.com,ActiveProxy
  - DOMAIN-SUFFIX,outlook.com,ActiveProxy
  - DOMAIN-SUFFIX,ow.ly,ActiveProxy
  - DOMAIN-SUFFIX,paddleapi.com,ActiveProxy
  - DOMAIN-SUFFIX,parallels.com,ActiveProxy
  - DOMAIN-SUFFIX,parse.com,ActiveProxy
  - DOMAIN-SUFFIX,pdfexpert.com,ActiveProxy
  - DOMAIN-SUFFIX,periscope.tv,ActiveProxy
  - DOMAIN-SUFFIX,pinboard.in,ActiveProxy
  - DOMAIN-SUFFIX,pinterest.com,ActiveProxy
  - DOMAIN-SUFFIX,pixelmator.com,ActiveProxy
  - DOMAIN-SUFFIX,pixiv.net,ActiveProxy
  - DOMAIN-SUFFIX,playpcesor.com,ActiveProxy
  - DOMAIN-SUFFIX,playstation.com,ActiveProxy
  - DOMAIN-SUFFIX,playstation.com.hk,ActiveProxy
  - DOMAIN-SUFFIX,playstation.net,ActiveProxy
  - DOMAIN-SUFFIX,playstationnetwork.com,ActiveProxy
  - DOMAIN-SUFFIX,pushwoosh.com,ActiveProxy
  - DOMAIN-SUFFIX,rime.im,ActiveProxy
  - DOMAIN-SUFFIX,servebom.com,ActiveProxy
  - DOMAIN-SUFFIX,sfx.ms,ActiveProxy
  - DOMAIN-SUFFIX,shadowsocks.org,ActiveProxy
  - DOMAIN-SUFFIX,sharethis.com,ActiveProxy
  - DOMAIN-SUFFIX,shazam.com,ActiveProxy
  - DOMAIN-SUFFIX,skype.com,ActiveProxy
  - DOMAIN-SUFFIX,smartdnsActiveProxy.com,ActiveProxy
  - DOMAIN-SUFFIX,smartmailcloud.com,ActiveProxy
  - DOMAIN-SUFFIX,sndcdn.com,ActiveProxy
  - DOMAIN-SUFFIX,sony.com,ActiveProxy
  - DOMAIN-SUFFIX,soundcloud.com,ActiveProxy
  - DOMAIN-SUFFIX,sourceforge.net,ActiveProxy
  - DOMAIN-SUFFIX,spotify.com,ActiveProxy
  - DOMAIN-SUFFIX,squarespace.com,ActiveProxy
  - DOMAIN-SUFFIX,sstatic.net,ActiveProxy
  - DOMAIN-SUFFIX,st.luluku.pw,ActiveProxy
  - DOMAIN-SUFFIX,stackoverflow.com,ActiveProxy
  - DOMAIN-SUFFIX,startpage.com,ActiveProxy
  - DOMAIN-SUFFIX,staticflickr.com,ActiveProxy
  - DOMAIN-SUFFIX,steamcommunity.com,ActiveProxy
  - DOMAIN-SUFFIX,symauth.com,ActiveProxy
  - DOMAIN-SUFFIX,symcb.com,ActiveProxy
  - DOMAIN-SUFFIX,symcd.com,ActiveProxy
  - DOMAIN-SUFFIX,tapbots.com,ActiveProxy
  - DOMAIN-SUFFIX,tapbots.net,ActiveProxy
  - DOMAIN-SUFFIX,tdesktop.com,ActiveProxy
  - DOMAIN-SUFFIX,techcrunch.com,ActiveProxy
  - DOMAIN-SUFFIX,techsmith.com,ActiveProxy
  - DOMAIN-SUFFIX,thepiratebay.org,ActiveProxy
  - DOMAIN-SUFFIX,theverge.com,ActiveProxy
  - DOMAIN-SUFFIX,time.com,ActiveProxy
  - DOMAIN-SUFFIX,timeinc.net,ActiveProxy
  - DOMAIN-SUFFIX,tiny.cc,ActiveProxy
  - DOMAIN-SUFFIX,tinypic.com,ActiveProxy
  - DOMAIN-SUFFIX,tmblr.co,ActiveProxy
  - DOMAIN-SUFFIX,todoist.com,ActiveProxy
  - DOMAIN-SUFFIX,trello.com,ActiveProxy
  - DOMAIN-SUFFIX,trustasiassl.com,ActiveProxy
  - DOMAIN-SUFFIX,tumblr.co,ActiveProxy
  - DOMAIN-SUFFIX,tumblr.com,ActiveProxy
  - DOMAIN-SUFFIX,tweetdeck.com,ActiveProxy
  - DOMAIN-SUFFIX,tweetmarker.net,ActiveProxy
  - DOMAIN-SUFFIX,twitch.tv,ActiveProxy
  - DOMAIN-SUFFIX,txmblr.com,ActiveProxy
  - DOMAIN-SUFFIX,typekit.net,ActiveProxy
  - DOMAIN-SUFFIX,ubertags.com,ActiveProxy
  - DOMAIN-SUFFIX,ublock.org,ActiveProxy
  - DOMAIN-SUFFIX,ubnt.com,ActiveProxy
  - DOMAIN-SUFFIX,ulyssesapp.com,ActiveProxy
  - DOMAIN-SUFFIX,urchin.com,ActiveProxy
  - DOMAIN-SUFFIX,usertrust.com,ActiveProxy
  - DOMAIN-SUFFIX,v.gd,ActiveProxy
  - DOMAIN-SUFFIX,v2ex.com,ActiveProxy
  - DOMAIN-SUFFIX,vimeo.com,ActiveProxy
  - DOMAIN-SUFFIX,vimeocdn.com,ActiveProxy
  - DOMAIN-SUFFIX,vine.co,ActiveProxy
  - DOMAIN-SUFFIX,vivaldi.com,ActiveProxy
  - DOMAIN-SUFFIX,vox-cdn.com,ActiveProxy
  - DOMAIN-SUFFIX,vsco.co,ActiveProxy
  - DOMAIN-SUFFIX,vultr.com,ActiveProxy
  - DOMAIN-SUFFIX,w.org,ActiveProxy
  - DOMAIN-SUFFIX,w3schools.com,ActiveProxy
  - DOMAIN-SUFFIX,webtype.com,ActiveProxy
  - DOMAIN-SUFFIX,wikiwand.com,ActiveProxy
  - DOMAIN-SUFFIX,wikileaks.org,ActiveProxy
  - DOMAIN-SUFFIX,wikimedia.org,ActiveProxy
  - DOMAIN-SUFFIX,wikipedia.com,ActiveProxy
  - DOMAIN-SUFFIX,wikipedia.org,ActiveProxy
  - DOMAIN-SUFFIX,windows.com,ActiveProxy
  - DOMAIN-SUFFIX,windows.net,ActiveProxy
  - DOMAIN-SUFFIX,wire.com,ActiveProxy
  - DOMAIN-SUFFIX,wordpress.com,ActiveProxy
  - DOMAIN-SUFFIX,workflowy.com,ActiveProxy
  - DOMAIN-SUFFIX,wp.com,ActiveProxy
  - DOMAIN-SUFFIX,wsj.com,ActiveProxy
  - DOMAIN-SUFFIX,wsj.net,ActiveProxy
  - DOMAIN-SUFFIX,xda-developers.com,ActiveProxy
  - DOMAIN-SUFFIX,xeeno.com,ActiveProxy
  - DOMAIN-SUFFIX,xiti.com,ActiveProxy
  - DOMAIN-SUFFIX,yahoo.com,ActiveProxy
  - DOMAIN-SUFFIX,yimg.com,ActiveProxy
  - DOMAIN-SUFFIX,ying.com,ActiveProxy
  - DOMAIN-SUFFIX,yoyo.org,ActiveProxy
  - DOMAIN-SUFFIX,ytimg.com,ActiveProxy
# Telegram
  - DOMAIN-SUFFIX,telegra.ph,ActiveProxy
  - DOMAIN-SUFFIX,telegram.org,ActiveProxy
  - IP-CIDR,91.108.4.0/22,ActiveProxy,no-resolve
  - IP-CIDR,91.108.8.0/22,ActiveProxy,no-resolve
  - IP-CIDR,91.108.12.0/22,ActiveProxy,no-resolve
  - IP-CIDR,91.108.16.0/22,ActiveProxy,no-resolve
  - IP-CIDR,91.108.56.0/22,ActiveProxy,no-resolve
  - IP-CIDR,149.154.160.0/22,ActiveProxy,no-resolve
  - IP-CIDR,149.154.164.0/22,ActiveProxy,no-resolve
  - IP-CIDR,149.154.168.0/22,ActiveProxy,no-resolve
  - IP-CIDR,149.154.172.0/22,ActiveProxy,no-resolve
# LAN
  - DOMAIN-SUFFIX,local,DIRECT
  - IP-CIDR,127.0.0.0/8,DIRECT
  - IP-CIDR,172.16.0.0/12,DIRECT
  - IP-CIDR,192.168.0.0/16,DIRECT
  - IP-CIDR,10.0.0.0/8,DIRECT
  - IP-CIDR,17.0.0.0/8,DIRECT
  - IP-CIDR,100.64.0.0/10,DIRECT
# Hytera
  - DOMAIN-SUFFIX,hytera.com,DIRECT
  - DOMAIN-SUFFIX,elhg.fa.ap2.oraclecloud.com,DIRECT
# 最终规则
  - GEOIP,CN,DIRECT
  - MATCH,ActiveProxy
```