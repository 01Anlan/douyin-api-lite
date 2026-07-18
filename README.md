# Douyin API Lite 使用说明

Douyin API Lite 是一个可以部署在自己服务器上的抖音接口程序。程序启动后，会提供网页接口文档、APIKey 管理、APIKey 领取、作品解析、评论获取、账号作品解析、Cookie 管理等功能。

> 使用时只需要准备程序文件 `douyin-api-lite-secure`，并在同级目录创建 `.env` 配置文件。

## 一、程序文件

需要使用的程序文件：

```text
douyin-api-lite-secure
```

这个文件就是服务程序，上传到 Linux 服务器后，配置好 `.env` 就可以运行。

## 二、适合谁使用

适合有自己服务器、想部署抖音解析接口服务的用户。

你需要准备：

- 一台 Linux 服务器
- 宝塔面板，推荐
- MySQL 数据库
- Redis
- Node.js
- 一个可以发送邮件的 SMTP 邮箱，只有使用“获取 APIKey”邮件功能时才需要
- 一个启动卡密

## 三、主要功能

- 抖音作品解析
- 抖音账号作品解析
- 抖音评论获取
- OpenAPI 在线接口文档
- APIKey 管理中心
- 用户自助领取 APIKey
- Cookie 管理
- 二维码登录
- APIKey IP 黑白名单
- APIKey 域名黑白名单

## 四、卡密获取方式

程序启动前必须配置启动卡密，也就是 `.env` 里的 `BOOTSTRAP_APIKEY`。

获取方式：

1. 打开网址：<https://km.9107788.xkm/>
2. 点击“使用 QQ 登录”
3. 登录后领取卡密
4. 把领取到的卡密填写到 `.env` 里的 `BOOTSTRAP_APIKEY`

示例：

```env
BOOTSTRAP_APIKEY=这里填写你领取到的卡密
```

## 五、宝塔面板部署教程

宝塔面板可以直接添加 Go 项目，不需要再用“纯静态站点 + 进程守护管理器”的方式。

下面以这个目录为例：

```text
/www/wwwroot/douyin-api-lite
```

### 1. 上传程序

1. 登录宝塔面板
2. 打开“文件”
3. 进入或新建目录：

```text
/www/wwwroot/douyin-api-lite
```

4. 把程序文件上传到这个目录：

```text
douyin-api-lite-secure
```

上传后完整路径类似：

```text
/www/wwwroot/douyin-api-lite/douyin-api-lite-secure
```

### 2. 创建 .env 配置文件

如果目录里有 `.env.example`，直接复制一份改名为 `.env`：

```bash
cp .env.example .env
```

如果是在宝塔文件管理里操作，也可以复制 `.env.example`，然后把复制出来的文件重命名为 `.env`。

也就是让目录保持类似这样：

```text
/www/wwwroot/douyin-api-lite/douyin-api-lite-secure
/www/wwwroot/douyin-api-lite/.env.example
/www/wwwroot/douyin-api-lite/.env
```

然后打开 `.env`，根据自己的信息修改：

```env
PORT=8080
PUBLIC_BASE_URL=https://你的域名
BOOTSTRAP_APIKEY=这里填写你领取到的卡密
MYSQL_DSN=数据库用户名:数据库密码@tcp(127.0.0.1:3306)/数据库名?charset=utf8mb4&parseTime=True&loc=Local
SMTP_HOST=smtp.example.com
SMTP_PORT=465
SMTP_USERNAME=admin@example.com
SMTP_PASSWORD=邮箱SMTP授权码
SMTP_FROM=admin@example.com
COOKIE_ADMIN_PASSWORD=设置一个Cookie管理密码
APIKEY_ADMIN_PAGE_PASSWORD=设置一个APIKey管理页面密码
HOME_PAGE_PASSWORD=留空或填写 your_home_page_password 表示不启用首页密码
OPENAPI_DOC_SYNC_ENABLED=0
```

保存后，确认 `.env` 和 `douyin-api-lite-secure` 在同一个目录。

### 3. 安装运行环境

宝塔面板里需要确认：

- MySQL 已安装，并且已经创建好数据库
- Redis 已安装并启动
- Node.js 已安装

如果宝塔软件商店里没有 Node.js，可以在宝塔终端里安装。

Ubuntu / Debian：

```bash
apt update
apt install -y nodejs npm
```

CentOS：

```bash
yum install -y nodejs npm
```

### 4. 给程序执行权限

打开宝塔终端，执行：

```bash
chmod +x /www/wwwroot/douyin-api-lite/douyin-api-lite-secure
```

### 5. 添加 Go 项目

1. 打开宝塔面板
2. 点击左侧“网站”
3. 在上方项目类型里选择“Go项目”
4. 点击“添加项目”
5. 在“添加Go项目”窗口里按下面填写

| 项目 | 填写内容 |
| --- | --- |
| 项目执行文件 | `/www/wwwroot/douyin-api-lite/douyin-api-lite-secure` |
| 项目名称 | `douyin-api-lite` |
| 项目端口 | `8080` |
| 放行端口 | 如果需要通过 `IP:8080` 访问就勾选；只用域名访问可以按宝塔提示选择 |
| 执行命令 | `./douyin-api-lite-secure` |
| 环境变量 | 推荐选择“从文件加载”，选择同目录的 `.env` |
| 运行用户 | 一般选择 `www`；如果遇到权限问题，再检查文件权限 |
| 开机启动 | 建议开启 |
| 项目备注 | 可以填写 `Douyin API Lite`，也可以留空 |
| 绑定域名 | 有域名就填写你的域名，例如 `api.example.com`；没有域名可以先留空 |

如果你的宝塔版本在“环境变量”里没有“从文件加载”，可以先选择“无”，但要确认 `.env` 和程序文件在同一个目录。

如果执行命令填写 `./douyin-api-lite-secure` 后无法启动，可以改成完整路径：

```bash
/www/wwwroot/douyin-api-lite/douyin-api-lite-secure
```

### 6. 启动并查看日志

添加项目后，回到“Go项目”列表，点击启动。

如果启动失败，先查看项目日志。常见原因有：

- `.env` 没有放在程序同级目录
- `BOOTSTRAP_APIKEY` 没有填写
- `PORT` 和宝塔里填写的项目端口不一致
- MySQL、Redis 或 Node.js 没有正常安装
- 程序文件没有执行权限

### 7. 访问项目

如果绑定了域名，可以访问：

```text
https://你的域名/
```

如果没有绑定域名，并且已经放行端口，可以访问：

```text
http://服务器IP:8080/
```

### 8. 开启 HTTPS

如果使用域名访问，建议在宝塔面板里给域名申请 SSL 证书，并开启 HTTPS。

操作位置一般在宝塔的站点或项目设置中，不同宝塔版本入口可能略有不同。

## 六、普通命令行部署方式

如果不用宝塔，也可以直接使用命令行部署。

创建目录：

```bash
mkdir -p /www/wwwroot/douyin-api-lite
cd /www/wwwroot/douyin-api-lite
```

上传 `douyin-api-lite-secure` 和 `.env.example` 后，复制配置文件：

```bash
cp .env.example .env
vim .env
```

给执行权限：

```bash
chmod +x ./douyin-api-lite-secure
```

启动：

```bash
./douyin-api-lite-secure
```

后台运行：

```bash
nohup ./douyin-api-lite-secure > douyin-api-lite.log 2>&1 &
```

查看日志：

```bash
tail -f douyin-api-lite.log
```

## 七、访问地址

默认端口是 `8080`。

如果在宝塔 Go 项目里没有绑定域名，并且已经放行端口，访问：

```text
http://服务器IP:8080/
```

如果在宝塔 Go 项目里绑定了域名并开启 HTTPS，访问：

```text
https://你的域名/
```

常用页面：

| 页面 | 地址 |
| --- | --- |
| OpenAPI 文档首页 | `/` |
| APIKey 管理中心 | `/apikey_admin.html?password=你的管理页密码` |
| 获取 APIKey 页面 | `/apikey_apply` |

## 八、.env 配置说明

| 配置项 | 是否必填 | 说明 |
| --- | --- | --- |
| `PORT` | 否 | 程序监听端口，默认建议使用 `8080`。 |
| `PUBLIC_BASE_URL` | 建议填写 | 你的公网访问地址，例如 `https://api.example.com`。 |
| `BOOTSTRAP_APIKEY` | 必填 | 启动卡密，到 <https://km.9107788.xkm/> 使用 QQ 登录领取。 |
| `MYSQL_DSN` | 必填 | MySQL 数据库连接信息。 |
| `SMTP_HOST` | 邮件功能必填 | SMTP 邮件服务器地址。 |
| `SMTP_PORT` | 邮件功能必填 | SMTP 端口，常见是 `465` 或 `587`。 |
| `SMTP_USERNAME` | 邮件功能必填 | SMTP 邮箱账号。 |
| `SMTP_PASSWORD` | 邮件功能必填 | SMTP 授权码或密码。 |
| `SMTP_FROM` | 邮件功能必填 | 发件人邮箱。 |
| `COOKIE_ADMIN_PASSWORD` | 建议填写 | Cookie 管理入口密码。 |
| `APIKEY_ADMIN_PAGE_PASSWORD` | 建议填写 | APIKey 管理中心页面密码。 |
| `HOME_PAGE_PASSWORD` | 建议填写 | 文档首页访问密码。留空或保持默认值则不启用。 |
| `APIKEY_APPLY_ONCE_PER_EMAIL` | 否 | 是否限制每个 QQ 邮箱只能领取一次 APIKey。默认开启。 |
| `OPENAPI_DOC_SYNC_ENABLED` | 否 | 是否让 `/openapi.json` 和 Swagger 文档 `servers` 内容按当前访问地址动态同步。默认关闭。 |
| `JOB_WORKER_COUNT` | 否 | 异步任务 worker 数量。默认 `1`。 |

## 九、MySQL 配置示例

宝塔创建数据库后，会得到：

- 数据库名
- 数据库用户名
- 数据库密码

把它们填入 `MYSQL_DSN`。

示例：

```env
MYSQL_DSN=douyin_user:douyin_password@tcp(127.0.0.1:3306)/douyin_db?charset=utf8mb4&parseTime=True&loc=Local
```

说明：

- `douyin_user` 改成你的数据库用户名
- `douyin_password` 改成你的数据库密码
- `douyin_db` 改成你的数据库名

## 十、APIKey 获取说明

程序内置 APIKey 的获取方式：

1. 访问程序内置领取页：

```text
https://你的域名/apikey_apply
```

2. 在页面里填写 QQ 邮箱
3. 系统会自动分配程序内的 APIKey，并通过邮件发送
4. 获取到的 APIKey 在调用接口时传入 `apikey` 参数

如果开启了按邮箱限领，默认同一个 QQ 邮箱只能领取一次。页面本身就是“领取密钥”入口，不是错误页。

## 十一、常用接口入口

接口详情请以 OpenAPI 文档页面为准。

| 功能 | 路径 | 说明 |
| --- | --- | --- |
| OpenAPI 文档 | `/` | 在线接口文档。 |
| OpenAPI JSON | `/openapi.json` | 接口文档原始 JSON。 |
| 抖音解析 | `/api` | 抖音作品解析入口。 |
| 作品详情 | `/detail` | 获取单个作品详情。 |
| 评论接口 | `/comment` | 获取作品评论。 |
| 账号作品 | `/account_detail` | 获取账号作品列表。 |
| 账号资料 | `/account_profile` | 获取账号资料。 |
| 下载文件 | `/download` | 下载生成的文件。 |
| APIKey 管理接口 | `/apikey_admin` | 管理 APIKey 的接口。 |
| APIKey 管理页面 | `/apikey_admin.html` | APIKey 管理中心页面。 |
| APIKey 获取页面 | `/apikey_apply` | 用户自助领取 APIKey 页面。 |

## 十二、OpenAPI 文档同步开关

默认情况下，`/openapi.json` 和 Swagger 页面里的文档内容是静态模式，不会根据访问域名自动同步服务器地址。

如果你希望让文档内容自动跟随当前访问域名，请把 `.env` 里的 `OPENAPI_DOC_SYNC_ENABLED` 设置为 `1`：

```env
OPENAPI_DOC_SYNC_ENABLED=1
```

开启后，`/openapi.json` 里的 `servers` 会按当前请求的协议和域名动态输出，Swagger 页面也会同步展示对应地址。

修改这个配置后，需要重启程序才会生效。

## 十三、常见问题

### 1. 启动提示未配置 BOOTSTRAP_APIKEY

请检查 `.env` 是否和 `douyin-api-lite-secure` 在同一个目录，并确认已经填写：

```env
BOOTSTRAP_APIKEY=你的卡密
```

卡密需要到 <https://km.9107788.xkm/> 使用 QQ 登录领取。

### 2. APIKey 管理中心打不开

请确认 `.env` 中配置了：

```env
APIKEY_ADMIN_PAGE_PASSWORD=你的管理页密码
```

打开 `/apikey_admin.html` 后直接输入密码即可进入，不需要再拼接查询参数。

### 3. 获取 APIKey 邮件发送失败

请检查 SMTP 配置是否正确：

```env
SMTP_HOST=smtp.example.com
SMTP_PORT=465
SMTP_USERNAME=admin@example.com
SMTP_PASSWORD=邮箱SMTP授权码
SMTP_FROM=admin@example.com
```

同时确认服务器没有禁止连接 SMTP 端口。

### 4. 域名打不开，但 IP:8080 可以打开

一般是宝塔 Go 项目里的域名或端口配置没有设置好，请检查：

- Go 项目里是否已经填写“绑定域名”
- Go 项目端口是否和 `.env` 里的 `PORT` 一致
- 程序是否正在运行
- 服务器防火墙或宝塔安全组是否放行端口
- 域名解析是否已经指向当前服务器

## 十三、安全建议

- 不要把 `.env` 发给别人
- 不要公开数据库密码、SMTP 授权码、管理密码
- APIKey 管理中心密码建议设置复杂一点
- 建议使用 HTTPS
- 建议开启宝塔防火墙或 Nginx 限流
- 建议定期备份数据库

## 十四、许可证

本程序使用专有软件许可。允许用户在授权范围内部署和使用，但禁止反编译、破解、转售、二次分发和未授权商用。

详细条款请查看 [LICENSE](LICENSE)。

## 十五、免责声明

本程序仅供学习、研究和合法接口集成使用。请遵守目标平台服务条款、当地法律法规以及数据合规要求。因不当使用造成的任何风险和后果由使用者自行承担。

## Copyright

Copyright © 2026 Anlan Net. All Rights Reserved.
