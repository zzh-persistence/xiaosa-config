# xiaosa-config

"潇洒 20260722 TVBoxOSC" 本地包的线上订阅版（`tvbox/` 单配置入口）。

## 来源

- 原始路径：`E:\电子书\潇洒，20260722\TVBoxOSC\tvbox\`
- 快照时间：2026-11-25
- 原始配置备份：`backup/api.original.json`（未改动的 `api.json`）
- 内容：115 个站点（`sites`）+ 1 条空直播位 + 23 个相对路径引用

## 订阅地址

```
https://zzh-persistence.github.io/xiaosa-config/tvbox/api.json
```

## 排错记录（2026-11-25）

初次上线后 APP 显示暂无数据，定位并修复两处：

1. **spider 缺少 md5**：影视仓规范为 `URL;md5;校验值` 三段式，缺少 md5 时 JAR 不加载，而 115 个站点中 108 个依赖该 JAR（`csp_` 爬虫），表现为全空。已补上 `;md5;98acf8e1884192d73fa7a8375125841a`。
2. **空字段**：`wallpaper`/`logo`/`danmaku` 原本被置为空字符串、`lives` 为 `[{}]` 空对象，部分 APP 版本会解析异常。已移除这些空字段，让 APP 走默认值。

如修改后仍显示暂无数据，请先在影视仓里**清除旧订阅缓存**再重新添加（Pages 有 10 分钟 CDN 缓存，也可在 URL 后加 `?t=时间戳` 强制刷新）。

## 改写内容

相对影视仓本地包，线上版做了三类改写（原始文件完整备份在 `backup/`）：

1. **25 处相对路径 → Pages 绝对 URL**：`./spider.jar`、`./js/*.js`、`./json/*.json`、`./py/*.py`、`./XBPQ/*.json`、`./XYQHiker/*.json` 全部改为 `https://zzh-persistence.github.io/xiaosa-config/tvbox/...`，使远程订阅能加载到爬虫和规则文件。
2. **11 处本地 cookie 引用清空**：原为 `http://127.0.0.1:9978/file/TVBox/bili_cookie.txt`（B站站点的用户本地 cookie 文件）。线上订阅无法访问手机本地文件，故置空——需要时请在 APP 端自行配置 cookie。
3. **3 处本地代理引用清空**：`wallpaper`、`logo`、`danmaku` 原指向 `127.0.0.1:9978` 本地代理服务，线上版置空，APP 会使用默认行为。

**未改动**：115 个站点的 key/name/api/ext、直播源（原包本身为空）、所有 JS/PY/JAR 引擎文件（字节级原样）。

## 目录结构

| 路径 | 说明 |
|---|---|
| `tvbox/api.json` | 线上订阅入口（115 站点） |
| `tvbox/spider.jar` | 爬虫 JAR，2,019,604 字节，MD5 `98acf8e188...`（与源包一致） |
| `tvbox/js/` | 11 个 drpy2 JS 规则 + 2 个引擎 |
| `tvbox/py/` | 6 个 Python 爬虫 |
| `tvbox/json/` | 11 个分类/过滤规则 |
| `backup/api.original.json` | 原始配置备份 |

## 校验

```bash
curl -sSL https://zzh-persistence.github.io/xiaosa-config/tvbox/api.json | head -c 200
```

## 备注

- 本包不含直播源（原始 `lives` 为 `[{}]`），直播需另行订阅。
- 若某站点不可用，多为上游站点自身失效，与本镜像无关。
- 本仓库仅托管第三方公开接口地址与爬虫脚本，不含媒体内容；请遵守当地法律法规，仅用于你有权访问的内容。
