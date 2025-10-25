# Little Tree Wallpaper 核心 API 规范（公告与更新）

本文档描述客户端读取的核心资源结构与字段规范，涵盖「公告」与「更新」两部分。

---

## 目录结构

```
core/
  announcement/
    lang/
      index.json                # 语言列表（如 zh、en、ja）
      {lang}/
        index.json              # 该语言的公告元数据数组
        content/
          {announcement_id}.md  # 公告正文（Markdown）
  update/
    channel.json                # 更新渠道列表（stable/beta/…）
    {channel}/
      update.json               # 该渠道的最新版本信息
```

---

## 线上访问路径（托管后）

资源将以静态文件形式托管至域名：`https://wallpaper.api.zsxiaoshu.cn/core/`，各资源的在线访问路径为：

- 渠道列表：
  - `https://wallpaper.api.zsxiaoshu.cn/core/update/channel.json`
- 某渠道更新：
  - `https://wallpaper.api.zsxiaoshu.cn/core/update/{channel}/update.json`
- 公告语言索引：
  - `https://wallpaper.api.zsxiaoshu.cn/core/announcement/lang/index.json`
- 某语言公告元数据：
  - `https://wallpaper.api.zsxiaoshu.cn/core/announcement/lang/{lang}/index.json`
- 某公告正文：
  - `https://wallpaper.api.zsxiaoshu.cn/core/announcement/lang/{lang}/content/{id}.md`

> 客户端可直接以 `GET` 请求获取上述 JSON/Markdown。建议在客户端侧设置合理的缓存策略（E-Tag/If-None-Match 或短期 max-age），并在关键文件（如 `update.json`）上使用较短缓存时间以保证更新及时性。

---

## 公告 API

### 文件位置
- 语言索引：`core/announcement/lang/index.json`
- 某语言的公告元数据：`core/announcement/lang/{lang}/index.json`
- 某公告的正文内容：`core/announcement/lang/{lang}/content/{id}.md`

> 说明：公告元数据存放在特定语言目录下，元数据对象中不额外冗余 `lang` 字段，语言由路径 `{lang}` 隐式确定。

### 公告元数据字段

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `id` | string | ✅ | 公告唯一 ID（建议：`YYYYMMDD-NN`，如 `20251025-01`） |
| `title` | string | ✅ | 公告标题（建议 ≤ 50 字符） |
| `publish_date` | ISO 8601 | ✅ | 生效时间（例：`2025-10-25T10:00:00Z`） |
| `expiry_date` | ISO 8601 | ✅ | 过期时间（过期后客户端不再显示） |
| `priority` | string | ❌ | `low` / `normal` / `high`（高优先级可弹窗） |
| `action_url` | string | ❌ | 点击“操作按钮”跳转链接 |
| `action_text` | string | ❌ | 操作按钮文字（如“立即更新”） |
| `channel` | list | ✅ | 显示给哪些更新渠道（例如：`["stable", "beta"]`） |

### 公告示例

- 语言索引 `core/announcement/lang/index.json`
```json
[
  { "id": "zh", "name": "简体中文" },
  { "id": "en", "name": "English" }
]
```

- 公告元数据 `core/announcement/lang/zh/index.json`
```json
[
  {
    "id": "20251025-01",
    "title": "欢迎参加小树壁纸Next的测试！",
    "publish_date": "2025-10-25T14:28:43+08:00",
    "expiry_date": "2026-10-25T14:28:43+08:00",
    "channel": ["beta"]
  }
]
```

- 公告正文 `core/announcement/lang/zh/content/20251025-01.md`
```markdown
小树壁纸Next 仍在早期开发阶段，可能存在不稳定或错误。

感谢您的支持与理解！
```

### 命名与时间规范
- 公告 `id`：`YYYYMMDD-NN`（每日多条用 01、02 递增）。
- 时间格式：ISO 8601，建议使用 UTC（`Z` 结尾）或带明确时区偏移（如 `+08:00`）。
- 客户端建议逻辑：
  - 仅展示 `publish_date <= now < expiry_date` 且 `channel` 包含当前更新渠道的公告。
  - `priority=high` 可触发一次性弹窗；过期后不再显示。

---

## 更新 API

### 文件位置
- 渠道列表：`core/update/channel.json`
- 某渠道最新版本：`core/update/{channel}/update.json`

### 渠道列表结构（`channel.json`）
每一项：
```json
{
  "id": "stable",
  "name": "正式版",
  "description": "适用于大多数用户的稳定版本，经过充分测试以确保可靠性和兼容性。",
  "order": 0
}
```
- `id`：渠道唯一 ID（如 `stable`、`beta`）。
- `order`：显示顺序，越小越靠前。

### 渠道更新文件结构（`{channel}/update.json`）

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `version` | string | ✅ | 当前最新版本（SemVer） |
| `channel` | string | ✅ | `stable` / `beta` / `alpha` |
| `release_notes_url` | string | ✅ | 更新日志链接（建议 GitHub Release） |
| `release_date` | ISO 8601 | ✅ | 发布时间 |
| `platforms.{platform}.{arch}` | object | ✅ | 平台/架构构建信息 |
| `download_url` | string | ✅ | 下载地址（建议 GitHub Release） |
| `size_bytes` | integer | ✅ | 文件大小（字节） |
| `sha256` | string | ✅ | 文件 SHA256（64 位十六进制） |
| `minimum_supported_version` | string | ❌ | 低于此版本需强制升级 |
| `force_update` | boolean | ❌ | 是否强制更新（如存在严重安全问题） |
| `update_supported_version` | string | ❌ | 低于此版本无法在应用内更新，需要手动去官网 |

#### 更新示例（节选）
```json
{
  "version": "0.2.1",
  "channel": "stable",
  "release_notes_url": "https://github.com/shu-shu-1/Little-Tree-Wallpaper-Next-Flet/releases/tag/v0.2.1",
  "release_date": "2025-10-25T10:00:00Z",
  "platforms": {
    "windows": {
      "x64": {
        "download_url": "https://github.com/shu-shu-1/Little-Tree-Wallpaper-Next-Flet/releases/download/v0.2.1/LittleTreeWallpaper-Next-0.2.1-windows-x64.exe",
        "size_bytes": 24576000,
        "sha256": "a1b2c3d4e5f6...（64位十六进制）"
      },
      "arm64": {
        "download_url": "https://.../LittleTreeWallpaper-Next-0.2.1-windows-arm64.exe",
        "size_bytes": 22118400,
        "sha256": "f6e5d4c3b2a1..."
      }
    },
    "linux": {
      "x64": {
        "download_url": "https://.../LittleTreeWallpaper-Next-0.2.1-linux-x64.AppImage",
        "size_bytes": 26214400,
        "sha256": "0a1b2c3d4e5f..."
      }
    }
  },
  "minimum_supported_version": "0.1.0",
  "force_update": false
}
```


---

## 常见问题
- Q：公告里为什么不含 `lang` 字段？
  - A：公告存放在按语言分隔的目录中，语言由路径决定，避免重复冗余。
- Q：客户端如何选择展示哪些公告？
  - A：应校验时间窗口与渠道归属；过期自动隐藏；`priority=high` 可用于重要提醒。
- Q：`update_supported_version` 与 `minimum_supported_version` 有何区别？
  - A：前者低于该版本无法在应用内更新（需跳转下载）；后者低于该版本必须强制升级（可结合 `force_update=true`）。

-