# Hostloc Auto Get Points
使用 GitHub Actions 自动获取 Hostloc 论坛积分1

## 更新说明

本仓库主要功能基本不会再改变，但是也会偶尔增加一些小更新，后续会在这里做更新通知。

**注意：** 对 Git 和 GitHub 不熟悉的人建议通过**删除原仓库，重新 Fork** 的方式更新，不要乱点 pull request ，我已经关掉过 n 个莫名奇妙的 pull request 了。

#### 2021年3月13日

论坛近期配置了 www 域强制跳转到顶级域，导致无法正常获取积分，[该 pull request](https://github.com/inkuang/hostloc-auto-get-points/pull/32) 已对此进行了修复。  
各位可以同步更新代码，或者手动将 `hostloc_get_points.py` 文件中所有的 `www.hostloc.com` 修改为 `hostloc.com` 即可解决问题。

#### 2020年12月1日

新增自动通过防 CC 机制验证的功能，解决了 Hostloc 开启防御时脚本无法正常使用的问题。  
如有最近一段时间出现各种报错或者无法正常获取积分的请尝试更新，相关问题讨论见 [#issues22](https://github.com/inkuang/hostloc-auto-get-points/issues/22)、[#issues24](https://github.com/inkuang/hostloc-auto-get-points/issues/24)

#### 2020年7月13日

新增显示当前所使用 IP 地址、显示登录后和获取积分完成后帐户总积分数的功能。

#### 2020年7月10日

由于近期 Hostloc 提高了封禁 IP 的严格程度，即使本脚本设置的每 4 秒请求一次仍出现了部分用户无法正常使用的情况，目前已修改为 5 秒请求一次，并在 HTTP 状态码不为 200 时抛出异常（主要解决 IP 被封禁时也显示登录失败的问题）。  
另外，建议各位修改一下文件  `.github/workflows/action.yml` 中的 `cron: '0 17 * * *'` 部分，设置一个自己的运行时间，不要全部挤在一起运行。

## 使用说明

Fork 本仓库，然后点击你的仓库右上角的 Settings，找到 Secrets 这一项，添加两个秘密环境变量。

![添加秘密环境变量](./images/set-secrets-env.png)

其中 `HOSTLOC_USERNAME` 存放你在 Hostloc 的帐户名，`HOSTLOC_PASSWORD` 存放你的帐户密码。支持同时添加多个帐户，数据之间用半角逗号 `,` 隔开即可，帐户名和帐户密码需一一对应。

设置好环境变量后点击你的仓库上方的 Actions 选项，会打开一个如下的页面，点击 `I understand...` 按钮确认在 Fork 的仓库上启用 GitHub Actions 。

![在 Fork 的仓库上启用 GitHub Actions](./images/understand-workflows.png)

此时页面上会显示当前仓库所有的 Workflows，点击左侧的 `Hostloc Auto Get Points`，然后点击页面上黄色提醒框 `This scheduled workflow is disabled...` 处的 `Enable workflow` 按钮确认在 Fork 的仓库上启用 GitHub Actions 定时任务。  
最后在你这个 Fork 的仓库内随便改点什么（比如给 README 文件删掉或者增加几个字符）提交一下手动触发一次 GitHub Actions 就可以了。

仓库内包含的 GitHub Actions 配置文件会在每天国际标准时间 17 点（北京时间凌晨 1 点）自动执行获取积分的脚本文件，你也可以通过 `Push` 操作手动触发执行（测试发现定时任务的执行可能有 5 到 10 分钟的延迟，属正常现象，耐心等待即可）。

**注意：** 为了实现某个链接/帐户访问出错时不中断程序继续尝试下一个，GitHub Actions 的状态将永远是“通过”（显示绿色的✔），请自行检查 GitHub Actions 日志 `Get points` 项的输出确定程序执行情况。
