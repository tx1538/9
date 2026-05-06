# 酒局派对小游戏 H5

一个手机端优先的喝酒/聚会小游戏合集，支持房间码、二维码加入、房主抽题、投票、倒计时、计分、普通/刺激尺度切换，以及 PHP 后台题库维护。

## 目录

- `index.html`：手机端 H5 入口
- `assets/`：前端样式和交互脚本
- `api/`：PHP API
- `admin/`：题库后台页面
- `database/schema.sql`：MySQL 表结构
- `database/seed.sql`：82 个玩法和 80+ 张初始题卡

## 宝塔部署

1. 在宝塔创建 PHP 站点，绑定你的域名并开启 HTTPS。
2. 在宝塔 MySQL 新建数据库，例如 `drink_party_h5`。
3. 先导入 `database/schema.sql`，再导入 `database/seed.sql`。
   - 已经部署过旧版的站点，额外导入一次 `database/v2_update.sql`。
4. 修改 `api/config/config.php`：
   - `database`、`username`、`password` 改成宝塔数据库信息。
   - `base_url` 改成你的域名，例如 `https://example.com`。
   - `admin_token` 改成一串只有你知道的后台密钥。
5. 将整个项目上传到网站根目录。
6. 访问域名，创建房间并用另一台手机扫码加入测试。

## 后台

访问 `/admin/`，输入 `api/config/config.php` 里的 `admin_token`。

后台支持：

- 新增题卡
- 停用/启用题卡
- 删除题卡
- CSV 批量导入
- 查看最近房间

CSV 格式：

```csv
title,body,challenge,category_key,intensity,prompt_type,game_id
示例题卡,说出一个今天的开心瞬间,说不出来就抽下一张,classic,normal,card,1
```

## API

- `POST /api/rooms/create.php` 创建房间，可传 `gameId` 直接选好玩法
- `POST /api/rooms/join.php` 加入房间
- `GET /api/rooms/state.php` 轮询房间状态
- `POST /api/rooms/action.php` 抽题、投票、计分、切换尺度
- `GET /api/games/list.php` 获取玩法库
- `GET /api/prompts/draw.php` 随机抽题卡
- `POST /api/admin/prompt.php` 后台维护题卡

## 注意

- 房间号为 5 位纯数字；该项目默认使用 2 秒轮询，适合线下聚会房间。
- 本地电脑未安装 PHP CLI 时，无法在本地执行 PHP 语法校验；可在宝塔服务器终端运行 `php -l api/rooms/create.php` 等命令检查。
- 饮酒玩法请仅用于成年人聚会；所有惩罚都应允许跳过或用无酒精饮料替代。
