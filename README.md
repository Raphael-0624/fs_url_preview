# Feishu URL Preview Bot

飞书 URL 预览机器人，为飞书个性签名/消息提供动态内容展示。基于 MarsCode 平台开发，支持一键部署到 Vercel。

📖 **配套教程**: [用豆包 MarsCode 让飞书个性签名「动」起来](https://www.feishu.cn/community/article/event)

## ✨ 功能特性

| 功能 | 路径 | 说明 |
|------|------|------|
| ⏰ 当前时间 | `/time` | 显示实时日期时间 |
| 🔮 每日塔罗 | `/tarot` | 根据用户 ID 生成每日塔罗牌 |
| 💬 一言 | `/hitokoto` | 展示随机优美句子 |
| 🌤️ 天气预报 | `/weather?city=xxx` | 查询指定城市天气 |

## 🚀 快速开始

### 1. 克隆仓库

```bash
git clone https://github.com/Rockelbel/fs_url_preview.git
cd fs_url_preview
```

### 2. 安装依赖

```bash
pip install -r requirements.txt
```

### 3. 配置环境变量

复制 `.env.example` 为 `.env` 并填写：

```bash
cp .env.example .env
```

编辑 `.env`：

```env
FEISHU_APP_ID="cli_xxxxxxxxxxxx"
FEISHU_APP_SECRET="xxxxxxxxxxxxxxxx"
AMAP_WEATHER_API_KEY="xxxxxxxxxxxxxxxx"
```

> 获取方式：
> - `FEISHU_APP_ID` / `FEISHU_APP_SECRET`: [飞书开放平台](https://open.feishu.cn/)
> - `AMAP_WEATHER_API_KEY`: [高德开放平台](https://lbs.amap.com/)

### 4. 本地运行

```bash
python -m flask --app api.index run --debug
```

服务运行在 `http://localhost:5000`，API 地址为 `/api/handler`

### 5. Vercel 部署

```bash
# 安装 Vercel CLI
npm i -g vercel

# 部署
vercel
```

## 📁 项目结构

```
fs_url_preview/
├── api/
│   └── index.py           # Flask 主应用
├── img-maker/
│   ├── upload.py          # 飞书图片上传工具
│   └── convert.py         # 图片透明度处理
├── .env.example           # 环境变量示例
├── .gitignore
├── requirements.txt       # Python 依赖
├── vercel.json            # Vercel 配置
└── README.md
```

## 🔌 API 接口

### URL Preview 回调

```
POST /api/handler
```

**飞书回调验证**

```json
{
  "type": "url_verification",
  "challenge": "xxx"
}
```

**URL Preview 请求**

```json
{
  "header": {
    "event_type": "url.preview.get"
  },
  "event": {
    "operator": {
      "open_id": "user_xxx"
    },
    "context": {
      "url": "https://your-domain.com/time"
    }
  }
}
```

**响应示例**

```json
{
  "inline": {
    "i18n_title": {
      "zh_cn": "当前时间是：2024-01-15 10:30:00"
    },
    "image_key": "img_v3_02gp_bc939d82-ad8d-4dd0-856d-c26e2d161b9g"
  }
}
```

### 支持的 URL 路径

| 路径 | 示例 | 输出 |
|------|------|------|
| `/time` | `your-domain.com/time` | 当前日期时间 |
| `/tarot` | `your-domain.com/tarot` | 今日塔罗牌 |
| `/hitokoto` | `your-domain.com/hitokoto` | 随机一言 |
| `/weather?city=xxx` | `your-domain.com/weather?city=110000` | 北京天气 |

> 天气 city 参数使用高德行政区划代码，如 110000(北京)、310000(上海)

## 🎓 教学步骤

本项目按教学步骤打上了 Git 标签：

```bash
# 查看所有标签
git tag

# 切换到指定步骤
git checkout 1a    # 第一节初始代码
git checkout 1b    # 第一节完成代码
git checkout 2a    # 第二节初始代码
# ...
```

## 🛠️ 技术栈

- **Flask** - Python Web 框架
- **Flask** - 飞书机器人服务
- **Vercel Serverless** - 无服务器部署
- **高德天气 API** - 天气数据
- **一言 API** - 随机句子

## 📝 自定义扩展

### 添加新功能

在 `api/index.py` 中添加新的路径处理：

```python
elif path == '/mycustom':
    inline_title = "自定义内容"
```

### 上传自定义图片

使用 `img-maker/upload.py` 上传图片到飞书：

```bash
python img-maker/upload.py
```

## ⚠️ 注意事项

1. **安全**: 生产环境请妥善保管 `APP_SECRET`
2. **缓存**: 天气数据有 5 秒缓存，避免频繁调用
3. **图片**: 飞书预览需要上传图片到开放平台获取 `image_key`

## 🤝 贡献

欢迎提交 Issue 和 PR！

## 📄 许可证

MIT License

---

**Happy Coding with MarsCode!** 🎉
