# Browser Screenshot Skill

📸 捕获浏览器当前页面并发送给用户的 OpenClaw 技能。

## 功能特性

- ✅ 标准网页截图
- ✅ 完整长页面截图（`fullPage: true`）
- ✅ 移动端尺寸适配截图
- ✅ 多种图片格式支持（PNG/JPEG/WEBP）

## 前置条件

使用前请确保：

1. **OpenClaw 已安装**：版本 >= 2026.3.0
2. **浏览器工具可用**：已配置 `browser` 工具
3. **消息渠道已连接**：已配置 Feishu/Telegram 等渠道

## 安装方法

```bash
npx skills add https://github.com/zyzyzzyyy/browser-screenshot
```

## 快速开始

### 1. 基础截图

在对话中直接说：
```
帮我截个百度的图
```

### 2. 移动端测试

```
用 iPhone 尺寸截个图，网址是 https://example.com
```

### 3. 完整长页面

```
截取整个长页面，网址是 https://example.com
```

## 使用示例

### 示例 1：网页截图

```
用户：帮我截个百度的图
执行：browser navigate -> browser screenshot -> message send
```

### 示例 2：移动端测试

```
用户：看看这个页面在手机上的效果
执行：browser screenshot -> width: 375, height: 667
```

### 示例 3：完整长页面

```
用户：截取整个长页面
执行：browser screenshot -> fullPage: true
```

## 触发词

- "浏览器截图"
- "网页截图"
- "截取当前页面"
- "capture browser"
- "screenshot the page"
- "移动端截图"

## 支持尺寸

| 设备 | 尺寸 |
|------|------|
| iPhone SE/13 Mini | 375x667 |
| iPhone 14 Pro | 393x852 |
| iPad | 768x1024 |

## 常见问题

### Q: 截图是空白页怎么办？

**A**: 确保先导航到目标网页，不是 `about:blank`。使用 `browser tabs` 查看当前页面。

### Q: 移动端截图不生效？

**A**: 需要先用 `resize` 调整视口，再截图。直接使用 `width/height` 参数不会改变视口。

### Q: 图片发送失败？

**A**: 检查文件路径是否正确，MIME 类型是否匹配（PNG 用 `image/png`）。

## 依赖

- OpenClaw Browser 工具
- OpenClaw Message 工具

## 版本

1.0.0 - 初始版本

## 许可证

MIT
