
| 特性 | HTTPS 模式 | SSH 模式 (你现在的状态) |
| :--- | :--- | :--- |
| 仓库地址 | `https://github.com/用户名/仓库.git` | `git@github.com:用户名/仓库.git` |
| 认证方式 | 账号 + 个人令牌 (Token) | SSH 密钥对 (公钥上传，私钥留存) |
| 主要痛点 | 每次推送可能需要输入 Token，或者需要配置复杂的凭据管理器 | 配置一次，终身免密 (除非密钥过期) |
| 适用场景 | 临时克隆、公共网络环境 | 日常开发、多设备同步、Obsidian 自动化 |<websource>source_group_web_1</websource>

查看一下本地仓库的远程是什么形式:
+ git remote -v

同时，也可以测试下ssh是否配置成功：
+ ssh -T git@github.com
+ ssh -T git@gitee.com

---
+ vscode也比较神奇/方便：
如果在vsc内登录了 github/gitee账号， 新打开终端或是git插件，它会自动帮我们添加好身份认证；也就是可以用git插件或是vscode内置的terminal，直接git push 而不需要 个人令牌/ssh免密。
可以看下：
> 悬浮在terminal图标上可以看到一些提示：
>The following extensions have contributed to this terminal's environment:
   Git: Enables the following features: git auth provider