
 touch mihomo_install.sh
 写入如下内容，然后sudo执行即可


```bash
# 定义 UI 存放路径变量

mihomo_dir="/etc/mihomo"

UI_DIR="/etc/mihomo/ui"

  

# 判断是否是sudo执行

if [ "$EUID" -ne 0 ]; then

    echo "请使用 sudo 执行此脚本以获得必要的权限。"

    exit 1

fi

mkdir -p "$mihomo_dir" "$UI_DIR"  && touch "$mihomo_dir/config.yaml" && cd "$mihomo_dir" && chmod 600 config.yaml

# install mihomo

wget  -O mihomo.deb https://github.com/MetaCubeX/mihomo/releases/download/v1.19.24/mihomo-linux-amd64-v3-v1.19.24.deb

dpkg -i mihomo.deb && rm mihomo.deb

   
  

# download metacubexd ui

# git clone https://github.com/metacubex/metacubexd.git -b gh-pages "$UI_DIR/metacubexd"

wget -O "$UI_DIR/metacubexd.tgz" https://github.com/MetaCubeX/metacubexd/releases/latest/download/compressed-dist.tgz

mkdir -p "$UI_DIR/metacubexd"

tar -xzf "$UI_DIR/metacubexd.tgz" -C "$UI_DIR/metacubexd/" && rm "$UI_DIR/metacubexd.tgz"

  
  

# download zashboard ui

# wget -O "$UI_DIR/zashboard.zip" https://github.com/Zephyruso/zashboard/releases/latest/download/dist.zip

# unzip "$UI_DIR/zashboard.zip" -d "$UI_DIR/zashboard/" && rm "$UI_DIR/zashboard.zip"

  

# download Yacd-meta ui

# wget -O "$UI_DIR/Yacd-meta.zip" https://github.com/MetaCubeX/yacd/archive/gh-pages.zip

# unzip "$UI_DIR/Yacd-meta.zip" -d "$UI_DIR/Yacd-meta/" && rm "$UI_DIR/Yacd-meta.zip"


cat > /etc/mihomo/config.yaml << 'EOF'

# ClashMeta 高级优化配置

# 最后更新：2024-10-28

# 作者：🥞煎饼果子卷鲨鱼辣椒🌶️

  

#------------------------基础配置------------------------#

mixed-port: 7890            # 混合端口：HTTP(S)和SOCKS5共用端口

geodata-mode: true          # GEO模式：true使用geoip.dat数据库,false使用mmdb数据库

tcp-concurrent: true        # TCP并发：允许并发连接TCP,提高并发性能

unified-delay: true         # 统一延迟：统一显示节点延迟

allow-lan: true            # 局域网连接：允许其他设备经过本机代理

bind-address: "*"          # 监听地址：*表示绑定所有IP地址

find-process-mode: strict  # 进程匹配模式：strict严格,off关闭,always总是

ipv6: false               # IPv6开关：是否启用IPv6支持

  

# 运行模式(任选其一):

# rule: 规则模式 - 根据规则匹配来选择代理

# global: 全局模式 - 全部流量走代理

# direct: 直连模式 - 全部流量不走代理

mode: rule

  

# 日志等级(按详细程度排序):

# debug: 调试

# info: 信息

# warning: 警告

# error: 错误

# silent: 静默

log-level: info

  

# 外部控制设置 ##  metacubexd    Yacd-meta  zashboard 都可以，推荐 metacubexd

external-controller: 127.0.0.1:9090        # 外部控制器监听地址

external-ui: "/etc/mihomo/ui/metacubexd"   # 外部控制器UI目录

secret: "!mihomo@.dashboard.local"       # 外部控制器密码

  
  

#------------------------性能调优------------------------#

tcp-concurrent-users: 64      # TCP并发连接数,根据服务器性能调整,建议值:16-128

keep-alive-interval: 15       # 保活心跳间隔(秒),建议值:15-30

inbound-tfo: true            # 入站TCP Fast Open

outbound-tfo: true           # 出站TCP Fast Open

  

# Windows示例

#interface-name: WLAN   # Windows中的无线网卡名称

# 或

#interface-name: 以太网  # Windows中的有线网卡名称

# macOS示例

#interface-name: en0    # macOS中通常是Wi-Fi

# 或

#interface-name: en1    # macOS中通常是有线网卡

# Linux示例

#interface-name: eth0   # Linux中常见的有线网卡名

# 或

#interface-name: wlan0  # Linux中常见的无线网卡名

  

# 连接池配置

connection-pool-size: 256     # 连接池大小,建议值:128-512

idle-timeout: 60             # 空闲超时时间(秒)

  

#------------------------TLS 配置------------------------#

tls:

  enable: true               # 启用TLS支持

  skip-cert-verify: false    # 是否跳过证书验证

  alpn:                      # 应用层协议协商

    - h2                     # HTTP/2

    - http/1.1              # HTTP/1.1

  min-version: "1.2"        # 最低TLS版本

  max-version: "1.3"        # 最高TLS版本

  cipher-suites:            # 加密套件优先级

    - TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256

    - TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256

    - TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305

    - TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305

  

#------------------------TUN 配置------------------------#

tun:

  enable: false              # 启用TUN模式? 默认配置为false,需要管理员权限才能启用

  stack: system

  auto-route: true

  auto-detect-interface: true

  dns-hijack:

    - any:53

    - tcp://any:53

  

#------------------------DNS 配置------------------------#

dns:

  enable: true              # 启用DNS服务器

  prefer-h3: true          # 优先使用HTTP/3查询

  ipv6: false              # DNS解析IPv6

  listen: 0.0.0.0:53       # DNS监听地址

  enhanced-mode: redir-host   # DNS模式: fake-ip或redir-host

  use-hosts: true          # 使用hosts文件

  

  # 默认DNS服务器(用于解析其他DNS服务器的域名)

  default-nameserver:

    - 223.5.5.5            # 阿里DNS

    - 119.29.29.29         # 腾讯DNS

  

  # DNS服务器分流策略

  nameserver-policy:

    'www.google.com': 'https://dns.google/dns-query'      # Google域名使用Google DNS

    'www.facebook.com': 'https://dns.google/dns-query'    # Facebook域名使用Google DNS

    '.cn': 'https://doh.pub/dns-query'                    # 中国域名使用国内DNS

  

  # Fake-IP配置

  fake-ip-range: 198.18.0.1/16    # Fake-IP地址段

  fake-ip-filter:                 # Fake-IP过滤清单

    - "*.lan"                     # 本地域名

    - "localhost.ptlogin2.qq.com" # QQ登录

  

  # 主要DNS服务器

  nameserver:

    # 国内DNS服务器

    - https://doh.pub/dns-query#h3=true                # DNSPod DOH

    - https://dns.alidns.com/dns-query#h3=true         # 阿里 DOH

    - tls://223.5.5.5:853                              # 阿里 DOT

  

    # 国外DNS服务器

    - https://dns.google/dns-query#h3=true             # Google DOH

    - https://cloudflare-dns.com/dns-query#h3=true     # Cloudflare DOH

    - quic://dns.adguard.com:784                       # AdGuard DOQ

  

  # 备用DNS服务器(用于解析国外域名)

  fallback:

    - https://dns.google/dns-query#h3=true

    - https://1.1.1.1/dns-query#h3=true

    - tls://8.8.8.8:853

  
  
  

# 代理提供商配置

proxy-providers:

  订阅1:

    type: http

    url: ""

    interval: 21600

    path: ./proxy_providers/sub1.yaml

    health-check:

      enable: true

      url: http://www.google.com/generate_204

      interval: 1800

  订阅2:

    type: http

    url: ""

    interval: 21600

    path: ./proxy_providers/sub2.yaml

    health-check:

      enable: true

      url: http://www.google.com/generate_204

      interval: 1800

  
  

#代理分组

#  include-all-providers: true 自动引入【proxy-providers】所有代理集合，顺序将按照名称排序

proxy-groups:

  #------------------------基础分组------------------------#

  - name: 🚀 节点选择

    type: select

    proxies:

      - ♻️ 自动选择

      - 🔯 故障转移

      - 🔮 负载均衡

      - 🇭🇰 香港节点

      - 🇲🇴 澳门节点

      - 🇨🇳 台湾节点

      - 🇯🇵 日本节点

      - 🇰🇷 韩国节点

      - 🇺🇲 美国节点

      - 🇬🇧 英国节点

      - 🇩🇪 德国节点

      - 🇫🇷 法国节点

      - 🇮🇳 印度节点

      - 🇸🇬 狮城节点

      - 🇮🇩 印尼节点

      - 🇻🇳 越南节点

      - 🇹🇭 泰国节点

      - 🇦🇺 澳洲节点

      - 🇧🇷 巴西节点

      - 🌍 其他节点

      - DIRECT

  

  - name: ♻️ 自动选择

    type: url-test

    include-all-providers: true

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100            # 调整延迟容差为100ms

  

  - name: 🔯 故障转移

    type: fallback

    include-all-providers: true

    url: http://www.gstatic.com/generate_204

    interval: 300

  

  - name: 🔮 负载均衡

    type: load-balance

    strategy: consistent-hashing

    include-all-providers: true

    url: http://www.gstatic.com/generate_204

    interval: 300

  

  #------------------------地区分组------------------------#

  - name: 🇭🇰 香港节点

    type: url-test

    include-all-providers: true

    filter: "(?i)港|hk|hongkong|hong kong"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇲🇴 澳门节点

    type: url-test

    include-all-providers: true

    filter: "(?i)澳门|门|mo|macao"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇨🇳 台湾节点

    type: url-test

    include-all-providers: true

    filter: "(?i)台|tw|taiwan|taipei"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇯🇵 日本节点

    type: url-test

    include-all-providers: true

    filter: "(?i)日本|jp|japan|tokyo|osaka"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇰🇷 韩国节点

    type: url-test

    include-all-providers: true

    filter: "(?i)韩|kr|korea|seoul"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇺🇲 美国节点

    type: url-test

    include-all-providers: true

    filter: "(?i)美|us|united states|america|los angeles|san jose|silicon valley"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇬🇧 英国节点

    type: url-test

    include-all-providers: true

    filter: "(?i)英|uk|united kingdom|london"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇩🇪 德国节点

    type: url-test

    include-all-providers: true

    filter: "(?i)德|de|germany|frankfurt"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇫🇷 法国节点

    type: url-test

    include-all-providers: true

    filter: "(?i)法|fr|france|paris"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇮🇳 印度节点

    type: url-test

    include-all-providers: true

    filter: "(?i)印度|in|india|mumbai"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇸🇬 狮城节点

    type: url-test

    include-all-providers: true

    filter: "(?i)新|sg|singapore"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇮🇩 印尼节点

    type: url-test

    include-all-providers: true

    filter: "(?i)印尼|印度尼西亚|id|indonesia|jakarta"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇻🇳 越南节点

    type: url-test

    include-all-providers: true

    filter: "(?i)越南|vn|vietnam"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇹🇭 泰国节点

    type: url-test

    include-all-providers: true

    filter: "(?i)泰国|th|thailand|bangkok"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇦🇺 澳洲节点

    type: url-test

    include-all-providers: true

    filter: "(?i)澳大利亚|au|australia|sydney"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🇧🇷 巴西节点

    type: url-test

    include-all-providers: true

    filter: "(?i)巴西|br|brazil"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  - name: 🌍 其他节点

    type: url-test

    include-all-providers: true

    filter: "(?i)^(?!.*(香港|台湾|日本|韩国|新加坡|美国|英国|德国|法国|印度|泰国|越南|印尼|澳大利亚|巴西|港|台|日|韩|新|美|英|德|法|印|泰|越|尼|澳|巴|hk|tw|jp|kr|sg|us|uk|de|fr|in|th|vn|id|au|br)).*"

    url: http://www.gstatic.com/generate_204

    interval: 300

    tolerance: 100

  

  #------------------------场景分组------------------------#

  

  - name: 🎬 国外媒体

    type: select

    proxies:

      - 🚀 节点选择

      - 🇭🇰 香港节点

      - 🇨🇳 台湾节点

      - 🇯🇵 日本节点

      - 🇺🇲 美国节点

      - 🇸🇬 狮城节点

  

  - name: 🎮 游戏平台

    type: select

    proxies:

      - 🚀 节点选择

      - 🔯 故障转移

      - 🇭🇰 香港节点

      - 🇯🇵 日本节点

      - 🇺🇲 美国节点

      - 🇸🇬 狮城节点

      - DIRECT

  

  - name: 📱 即时通讯

    type: select

    proxies:

      - 🚀 节点选择

      - 🔯 故障转移

      - 🇭🇰 香港节点

      - 🇯🇵 日本节点

      - 🇺🇲 美国节点

      - 🇸🇬 狮城节点

  

  - name: 🤖 AI平台

    type: select

    proxies:

      - 🇯🇵 日本节点

      - 🇺🇲 美国节点

      - 🇸🇬 狮城节点

      - 🇰🇷 韩国节点

      - 🚀 节点选择

      - 🔯 故障转移

  

  - name: 🔧 GitHub

    type: select

    proxies:

      - 🚀 节点选择

      - 🔯 故障转移

      - 🇭🇰 香港节点

      - 🇨🇳 台湾节点

      - 🇯🇵 日本节点

      - 🇺🇲 美国节点

      - 🇸🇬 狮城节点

      - DIRECT

  
  

  - name: Ⓜ️ 微软服务

    type: select

    proxies:

      - 🚀 节点选择

      - 🇭🇰 香港节点

      - 🇨🇳 台湾节点

      - 🇯🇵 日本节点

      - 🇺🇲 美国节点

      - 🇸🇬 狮城节点

      - DIRECT

  

  - name: 🍎 苹果服务

    type: select

    proxies:

      - 🚀 节点选择

      - 🇭🇰 香港节点

      - 🇨🇳 台湾节点

      - 🇯🇵 日本节点

      - 🇺🇲 美国节点

      - 🇸🇬 狮城节点

      - DIRECT

  

  #------------------------特殊分组------------------------#

  - name: 🎯 全球直连

    type: select

    proxies:

      - DIRECT

      - 🚀 节点选择

  

  - name: 🛑 广告拦截

    type: select

    proxies:

      - REJECT

      - DIRECT

  

  - name: 🍃 应用净化

    type: select

    proxies:

      - REJECT

      - DIRECT

  

  - name: 🆎 AdBlock

    type: select

    proxies:

      - REJECT

      - DIRECT

  

  - name: 🛡️ 隐私防护

    type: select

    proxies:

      - REJECT

      - DIRECT

  

  - name: 🐟 漏网之鱼

    type: select

    proxies:

      - 🚀 节点选择

      - 🎯 全球直连

      - ♻️ 自动选择

      - 🔯 故障转移

  

# 规则提供商配置 - 优化版

rule-providers:

  # 广告规则

  reject:

    type: http

    behavior: domain

    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/reject.txt"

    path: ./ruleset/reject.yaml

    interval: 86400

  

  # 隐私规则

  privacy:

    type: http

    behavior: domain

    url: "https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script/rule/Clash/Privacy/Privacy.yaml"

    path: ./ruleset/privacy.yaml

    interval: 86400

  

  # 广告扩展规则

  reject-extra:

    type: http

    behavior: domain

    url: "https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script/rule/Clash/AdvertisingLite/AdvertisingLite.yaml"

    path: ./ruleset/reject-extra.yaml

    interval: 86400

  

  # AI平台规则

  ai-platforms:

    type: http

    behavior: classical

    url: "https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script/rule/Clash/OpenAI/OpenAI.yaml"

    path: ./ruleset/ai-platforms.yaml

    interval: 86400

  

  # 流媒体规则

  streaming:

    type: http

    behavior: classical

    url: "https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script/rule/Clash/GlobalMedia/GlobalMedia.yaml"

    path: ./ruleset/streaming.yaml

    interval: 86400

  

  # 社交通讯规则

  social:

    type: http

    behavior: classical

    url: "https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script/rule/Clash/Telegram/Telegram.yaml"

    path: ./ruleset/social.yaml

    interval: 86400

  

  # 微软服务规则

  microsoft:

    type: http

    behavior: classical

    url: "https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script/rule/Clash/Microsoft/Microsoft.yaml"

    path: ./ruleset/microsoft.yaml

    interval: 86400

  

  # 苹果服务规则

  apple:

    type: http

    behavior: classical

    url: "https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script/rule/Clash/Apple/Apple.yaml"

    path: ./ruleset/apple.yaml

    interval: 86400

  

  # 游戏平台规则

  games:

    type: http

    behavior: classical

    url: "https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script/rule/Clash/Game/Game.yaml"

    path: ./ruleset/games.yaml

    interval: 86400

  

  # 开发平台规则

  dev-platforms:

    type: http

    behavior: classical

    url: "https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script/rule/Clash/GitHub/GitHub.yaml"

    path: ./ruleset/dev-platforms.yaml

    interval: 86400

  

# 规则配置 - 优化版

rules:

  # 优先处理

  - RULE-SET,reject,🛑 广告拦截,no-resolve

  - RULE-SET,privacy,🛡️ 隐私防护,no-resolve

  - RULE-SET,reject-extra,🆎 AdBlock,no-resolve

  

  # 本地局域网

  - DOMAIN-SUFFIX,local,DIRECT

  - DOMAIN-SUFFIX,localhost,DIRECT

  - IP-CIDR,127.0.0.0/8,DIRECT

  - IP-CIDR,172.16.0.0/12,DIRECT

  - IP-CIDR,192.168.0.0/16,DIRECT

  - IP-CIDR,10.0.0.0/8,DIRECT

  - IP-CIDR,17.0.0.0/8,DIRECT

  - IP-CIDR,100.64.0.0/10,DIRECT

  - IP-CIDR,224.0.0.0/4,DIRECT

  - IP-CIDR6,fe80::/10,DIRECT

  

  # 应用分流

  - RULE-SET,ai-platforms,🤖 AI平台,no-resolve

  - RULE-SET,streaming,🎬 国外媒体,no-resolve

  - RULE-SET,social,📱 即时通讯,no-resolve

  - RULE-SET,microsoft,Ⓜ️ 微软服务,no-resolve

  - RULE-SET,apple,🍎 苹果服务,no-resolve

  - RULE-SET,games,🎮 游戏平台,no-resolve

  - RULE-SET,dev-platforms,🔧 GitHub,no-resolve

  

  # 自定义规则

  - PROCESS-NAME,clash,DIRECT

  - PROCESS-NAME,v2ray,DIRECT

  - PROCESS-NAME,xray,DIRECT

  - PROCESS-NAME,naive,DIRECT

  - PROCESS-NAME,trojan,DIRECT

  - PROCESS-NAME,trojan-go,DIRECT

  - PROCESS-NAME,ss-local,DIRECT

  - PROCESS-NAME,privoxy,DIRECT

  - PROCESS-NAME,leaf,DIRECT

  - PROCESS-NAME,Thunder,DIRECT

  - PROCESS-NAME,DownloadService,DIRECT

  - PROCESS-NAME,qBittorrent,DIRECT

  - PROCESS-NAME,Transmission,DIRECT

  - PROCESS-NAME,fdm,DIRECT

  - PROCESS-NAME,aria2c,DIRECT

  - PROCESS-NAME,Folx,DIRECT

  - PROCESS-NAME,NetTransport,DIRECT

  - PROCESS-NAME,uTorrent,DIRECT

  - PROCESS-NAME,WebTorrent,DIRECT

  

  # 地域规则

  - GEOIP,LAN,DIRECT,no-resolve

  - GEOIP,CN,DIRECT,no-resolve

  

  # 兜底规则

  - MATCH,🚀 节点选择

EOF

  

echo "------------------------\n\

Clash mihomo 已安装到 $mihomo_dir\n\

启动命令: sudo systemctl start mihomo\n\
状态查看: sudo systemctl status mihomo\n\
开机自启: sudo systemctl enable mihomo\n\
日志查看: sudo journalctl -u mihomo -f\n\

配置文件路径: $mihomo_dir/config.yaml\n\
UI路径: $UI_DIR/metacubexd\n\

推荐使用 metacubexd 作为UI,还可以使用zashboard 或Yacd-meta\n\

使用 sudo systemctl start mihomo启动服务后, 访问 http://localhost:9090/ui 即可打开控制面板，默认密码为 !mihomo@.dashboard.local \n\

------------------------\n\
"
```