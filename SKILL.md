# 📸 浏览器截图技能

> **适用范围**：OpenClaw 平台 + 浏览器工具  
> **核心能力**：捕获浏览器当前页面并发送给用户  
> **成功关键**：确保浏览器已加载目标网页（不是空白页）

---

## 📌 触发场景

当用户表达以下意图时触发此技能：

- "浏览器截图"、"网页截图"、"截图看看"
- "screenshot"、"capture browser"
- 需要移动端适配测试
- 需要完整页面捕获
- 交互后的页面截图

**触发词示例**：
- "浏览器截图"
- "网页截图"
- "screenshot"
- "capture browser"
- "移动端截图"
- "全屏截图"

---

## 📥 步骤 1：打开目标网页

### 基础格式

```json
{
  "action": "navigate",
  "target": "host",
  "url": "https://www.baidu.com"
}
```

### 参数说明

| 参数 | 必填 | 说明 | 示例 |
|------|------|------|------|
| `action` | ✅ | 固定为 `"navigate"` | `"navigate"` |
| `target` | ✅ | 浏览器目标 | `"host"` 或 `"sandbox"` |
| `url` | ✅ | 要打开的网址 | `"https://www.baidu.com"` |

### 验证方法

查看当前标签页：
```json
{
  "action": "tabs",
  "target": "host"
}
```

如果看到 `about:blank`，说明是空白页，需要重新导航。

---

## 📤 步骤 2：截图

### 标准截图

```json
{
  "action": "screenshot",
  "target": "host",
  "type": "png"
}
```

### 完整页面截图

```json
{
  "action": "screenshot",
  "target": "host",
  "type": "png",
  "fullPage": true
}
```

### 移动端尺寸截图（重要！）

**⚠️ 注意**：移动端截图需要**先调整浏览器视口**，不能直接在 `screenshot` 中设置 `width/height`！

**错误做法**（不会改变视口）：
```json
{
  "action": "screenshot",
  "target": "host",
  "type": "png",
  "width": 375,
  "height": 667
}
```

**正确流程**：

```json
// 步骤 1: 打开网页
{
  "action": "navigate",
  "target": "host",
  "url": "https://example.com"
}

// 步骤 2: 调整视口为移动端尺寸（关键！）
{
  "action": "act",
  "target": "host",
  "request": {
    "kind": "resize",
    "width": 375,
    "height": 667
  }
}

// 步骤 3: 等待页面重新渲染
{
  "action": "act",
  "request": {
    "kind": "wait",
    "timeMs": 500
  }
}

// 步骤 4: 截图（此时已经是移动端视口）
{
  "action": "screenshot",
  "target": "host",
  "type": "png"
}
```

**常见移动端尺寸**：

| 设备 | 宽度 | 高度 |
|------|------|------|
| iPhone SE/13 Mini | 375 | 667 |
| iPhone 14 Pro | 393 | 852 |
| iPhone 14 Pro Max | 430 | 932 |
| iPad | 768 | 1024 |
| Android 常见 | 360 | 640 |

### 参数说明

| 参数 | 必填 | 说明 | 示例 |
|------|------|------|------|
| `action` | ✅ | 固定为 `"screenshot"` | `"screenshot"` |
| `target` | ✅ | 浏览器目标（与 navigate 一致） | `"host"` |
| `type` | ✅ | 图片格式 | `"png"` 或 `"jpeg"` |
| `fullPage` | ❌ | 是否截取整个页面 | `true` 或 `false` |

### 返回结果

成功后返回：
```
MEDIA:~/.openclaw/media/browser/xxx.png
```

这是截图文件的本地路径。

---

## 📤 步骤 3：发送截图

```json
{
  "action": "send",
  "media": "~/.openclaw/media/browser/xxx.png",
  "mimeType": "image/png"
}
```

### 参数说明

| 参数 | 必填 | 说明 | 示例 |
|------|------|------|------|
| `action` | ✅ | 固定为 `"send"` | `"send"` |
| `media` | ✅ | 截图的**本地绝对路径** | `"~/.openclaw/media/browser/xxx.png"` |
| `mimeType` | ✅ | 图片类型 | `"image/png"` 或 `"image/jpeg"` |

---

## ✅ 完整示例

### 示例 1：基础截图流程

**场景**：给用户看百度首页

**步骤**：
```json
// 1. 打开网页
{
  "action": "navigate",
  "target": "host",
  "url": "https://www.baidu.com"
}

// 2. 截图
{
  "action": "screenshot",
  "target": "host",
  "type": "png"
}

// 3. 发送
{
  "action": "send",
  "media": "~/.openclaw/media/browser/xxx.png",
  "mimeType": "image/png"
}
```

### 示例 2：移动端截图流程（推荐）

```json
// 1. 打开网页
{
  "action": "navigate",
  "target": "host",
  "url": "http://localhost:5173/"
}

// 2. 调整视口为移动端
{
  "action": "act",
  "target": "host",
  "request": {
    "kind": "resize",
    "width": 375,
    "height": 667
  }
}

// 3. 等待页面重新渲染
{
  "action": "act",
  "request": {
    "kind": "wait",
    "timeMs": 500
  }
}

// 4. 截图
{
  "action": "screenshot",
  "target": "host",
  "type": "png"
}

// 5. 发送
{
  "action": "send",
  "media": "~/.openclaw/media/browser/xxx.png",
  "mimeType": "image/png"
}
```

### 示例 3：带交互的完整流程

```json
// 1. 打开网页
{
  "action": "navigate",
  "target": "host",
  "url": "http://localhost:5173/"
}

// 2. 输入问题并点击
{
  "action": "act",
  "target": "host",
  "request": {
    "kind": "evaluate",
    "fn": "() => { document.getElementById('questionInput').value = '我会看到猴子吗？'; document.getElementById('answerBtn').click(); return 'done'; }"
  }
}

// 3. 等待答案出现
{
  "action": "act",
  "target": "host",
  "request": {
    "kind": "wait",
    "timeMs": 2000
  }
}

// 4. 移动端截图
{
  "action": "act",
  "target": "host",
  "request": {
    "kind": "resize",
    "width": 375,
    "height": 667
  }
}

{
  "action": "act",
  "target": "host",
  "request": {
    "kind": "wait",
    "timeMs": 500
  }
}

{
  "action": "screenshot",
  "target": "host",
  "type": "png"
}

// 5. 发送
{
  "action": "send",
  "media": "~/.openclaw/media/browser/xxx.png",
  "mimeType": "image/png"
}
```

---

## ❌ 常见错误与解决方案

| 错误 | 原因 | 解决方案 |
|------|------|---------|
| 空白页截图 | 浏览器当前是 `about:blank` | 先用 `navigate` 打开目标网页 |
| 截图前未验证 | 不知道当前是什么页面 | 先用 `browser tabs` 查看当前标签页 |
| MIME 类型不匹配 | PNG 文件用 `image/jpeg` | PNG 用 `image/png`，JPEG 用 `image/jpeg` |
| 路径错误 | 使用相对路径 | 使用绝对路径 `~/.openclaw/media/browser/xxx.png` |
| **移动端截图无效** | 直接在 `screenshot` 中设置 `width/height` | **先用 `act` + `resize` 调整视口，再截图** |

---

## 🔍 问题排查

### 截图是空白的

1. 用 `browser tabs` 查看当前页面
2. 如果是 `about:blank`，先导航到目标网页
3. 等待页面加载完成再截图

### 文件找不到

1. 检查路径是否正确
2. 用 `ls -lh ~/.openclaw/media/browser/` 查看最新截图
3. 截图文件名是 UUID 格式

### 文件大小异常

- **正常网页截图**：几十 KB 到几百 KB
- **空白页截图**：几 KB（非常小）
- 如果文件太小，很可能是空白页

---

## 🎯 成功关键

**流程图**：
```
打开网页 (navigate) 
  → 验证非空白页 (tabs) 
  → 调整视口（移动端需 resize）
  → 截图 (screenshot) 
  → 发送 (message)
```

**关键点**：
- ✅ 先导航到目标网页
- ✅ 验证不是空白页
- ✅ **移动端截图：先 resize 再截图**
- ✅ 使用正确的 MIME 类型
- ✅ 使用绝对路径

**核心公式**：
1. `navigate(url)`
2. `act(kind: "resize", width, height)` ← 移动端必备
3. `act(kind: "wait", timeMs: 500)` ← 等待渲染
4. `screenshot()` → 返回路径
5. `send(路径，MIME 类型)`

---

## 📁 文件路径

- **截图保存位置**：`~/.openclaw/media/browser/<UUID>.png`
- **文件格式**：PNG 或 JPEG
- **命名规则**：UUID 格式（如 `xxx.png`）

---

## 📚 依赖

- OpenClaw Browser 工具
- OpenClaw Message 工具
- 配置好的浏览器环境

---

*版本：1.1.0*  
*最后更新：2026-03-12*  
*更新内容：添加正确的移动端截图流程（先 resize 再 screenshot）*
