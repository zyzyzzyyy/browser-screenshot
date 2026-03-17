# Browser Screenshot Skill 📸

## Description
捕获浏览器当前页面。**截图会自动发送给用户**，无需额外调用发送功能。

## Activation
当用户提到以下关键词时触发：
- "浏览器截图"
- "网页截图"
- "截取当前页面"
- "capture browser"
- "screenshot the page"
- "截图看看"
- "移动端截图"
- "手机尺寸截图"

## ⚠️ 重要：截图会自动发送

browser screenshot 工具执行后，图片会**自动发送给用户**（通过 MEDIA: 路径机制）。

**调用此 skill 时**：
- ✅ 只调用 browser screenshot，不需要再调用 message send
- ❌ 不要调用 feishu-send-image，否则会发送两次！

## Workflow

### 步骤 1: 打开目标网页（如需要）
```json
{
  "action": "navigate",
  "target": "host",
  "url": "<目标网址>"
}
```

### 步骤 2: 验证页面加载（如需要）
```json
{
  "action": "tabs",
  "target": "host"
}
```

### 步骤 3: 执行截图

#### 标准截图
```json
{
  "action": "screenshot",
  "target": "host",
  "type": "png"
}
```

#### 完整长页面
```json
{
  "action": "screenshot",
  "target": "host",
  "type": "png",
  "fullPage": true
}
```

#### 移动端尺寸截图
```json
{
  "action": "screenshot",
  "target": "host",
  "type": "png",
  "width": 375,
  "height": 667
}
```

**常见移动端尺寸**:
- iPhone SE/13 Mini: 375x667
- iPhone 14 Pro: 393x852
- iPhone 14 Pro Max: 430x932
- iPad: 768x1024
- Android 常见：360x640, 412x915

### 步骤 4: 结束
截图会自动发送，**不需要**再调用 message send 或 feishu-send-image！

## Common Patterns

### 完整页面截图
```json
// 1. 打开网页
{"action": "navigate", "target": "host", "url": "https://example.com"}

// 2. 等待加载
{"action": "act", "request": {"kind": "wait", "timeMs": 2000}}

// 3. 截图（会自动发送）
{"action": "screenshot", "target": "host", "type": "png", "fullPage": true}

// 结束！不需要再发送
```

### 移动端适配截图
```json
// 1. 打开网页
{"action": "navigate", "target": "host", "url": "http://localhost:5173/"}

// 2. 截图（会自动发送）
{"action": "screenshot", "target": "host", "type": "png", "width": 375, "height": 667}

// 结束！
```

## Error Handling

### 错误 1: 空白页截图
**现象**: 截图是全白的  
**原因**: 浏览器当前是 `about:blank`  
**解决**: 先用 `navigate` 打开目标网页

### 错误 2: 截图前未验证
**现象**: 截到了错误的页面  
**解决**: 先用 `browser tabs` 查看当前标签页

## Tips
- ✅ 截图前确认不是 `about:blank`
- ✅ 截图会自动发送，**不需要**调用 message send
- ✅ 移动端截图用 `width` 和 `height` 参数
- ✅ 长页面用 `fullPage: true`
- ❌ 不要调用 feishu-send-image 发送截图
- ❌ 不要调用 message send 发送截图

## Version
- 2.0.0 - 简化流程，截图自动发送，无需额外发送步骤
