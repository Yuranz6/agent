# GitHub Pages + Render 部署指南

本指南将帮助您将前端部署到 GitHub Pages，后端部署到 Render。

## 前置要求

1. GitHub 账号和仓库
2. Render 账号（可通过 GitHub 登录）
3. API 密钥：
   - ElevenLabs API Key
   - Google AI API Key

## 步骤 1: 后端部署到 Render

### 1.1 登录 Render

1. 访问 https://render.com
2. 使用 GitHub 账号登录

### 1.2 创建 Web Service

有两种方式：

#### 方式一：使用 render.yaml（推荐）

1. 在 Render Dashboard 点击 "New" → "Apply Manifest"
2. 选择你的 GitHub 仓库
3. Render 会自动读取 `render.yaml` 配置
4. 在环境变量部分，设置以下密钥：
   - `ELEVENLABS_API_KEY`: 你的 ElevenLabs API 密钥
   - `GOOGLE_AI_API_KEY`: 你的 Google AI API 密钥
   - `ELEVENLABS_VOICE_ID`: （可选）默认语音 ID
5. 点击 "Apply" 开始部署

#### 方式二：手动创建

1. 在 Render Dashboard 点击 "New" → "Web Service"
2. 连接你的 GitHub 仓库
3. 配置如下：
   - **Name**: `clone-backend`
   - **Environment**: `Node`
   - **Region**: 选择离你最近的区域
   - **Branch**: `main` 或 `master`
   - **Root Directory**: 留空
   - **Build Command**: `npm install`
   - **Start Command**: `node server/index.js`
   - **Plan**: Free（或根据需要选择）

4. 在 Environment Variables 部分添加：
   ```
   NODE_ENV=production
   PORT=10000
   CLIENT_URL=https://yuranz6.github.io
   ELEVENLABS_API_KEY=your_elevenlabs_api_key
   GOOGLE_AI_API_KEY=your_google_ai_api_key
   ELEVENLABS_VOICE_ID=your_voice_id（可选）
   ```

5. 点击 "Create Web Service"

### 1.3 获取后端 URL

部署完成后，Render 会提供一个 URL，例如：
```
https://clone-backend.onrender.com
```

**重要**: 记下这个 URL，下一步会用到。

## 步骤 2: 前端部署到 GitHub Pages

### 2.1 配置 GitHub Secrets（可选但推荐）

1. 进入你的 GitHub 仓库
2. 点击 Settings → Secrets and variables → Actions
3. 点击 "New repository secret"
4. 添加以下 secret：
   - **Name**: `REACT_APP_API_URL`
   - **Value**: `https://your-render-backend-url.onrender.com/api`
   （将 `your-render-backend-url` 替换为你的实际 Render 后端 URL）

### 2.2 启用 GitHub Pages

1. 进入仓库 Settings → Pages
2. 在 "Source" 部分，选择 "GitHub Actions"
3. 保存设置

### 2.3 推送代码触发部署

1. 确保代码已推送到 `main` 或 `master` 分支
2. GitHub Actions 会自动运行部署工作流
3. 部署完成后，前端地址为：`https://yuranz6.github.io/Voice-clone`

### 2.4 手动触发部署（可选）

如果自动部署没有触发，可以：
1. 进入仓库的 Actions 标签页
2. 选择 "Deploy to GitHub Pages" 工作流
3. 点击 "Run workflow"

## 步骤 3: 验证部署

### 3.1 检查后端

在浏览器或使用 curl 访问：
```
https://your-render-backend-url.onrender.com/api/health
```

应该返回：
```json
{"status":"ok","message":"Voice Clone API is running"}
```

### 3.2 检查前端

1. 访问 `https://yuranz6.github.io/Voice-clone`
2. 打开浏览器开发者工具（F12）
3. 检查 Console 标签页，确认 API 连接正常
4. 如果看到 API Base URL 日志，确认指向正确的 Render 后端

## 配置说明

### 前端配置

- **homepage**: 在 `client/package.json` 中设置为 `https://yuranz6.github.io/Voice-clone`
- **API URL**: 通过环境变量 `REACT_APP_API_URL` 配置，默认指向 `https://clone-i9i8.onrender.com/api`

### 后端配置

- **CORS**: 后端已配置允许来自 `https://yuranz6.github.io` 的请求
- **端口**: Render 使用端口 10000（在 render.yaml 中配置）

## 常见问题

### 1. CORS 错误

**问题**: 前端无法连接后端，出现 CORS 错误

**解决**:
- 确保 Render 后端的 `CLIENT_URL` 环境变量设置为 `https://yuranz6.github.io`
- 不要包含末尾的 `/Voice-clone` 路径

### 2. 前端无法连接后端

**问题**: 前端显示 API 连接失败

**解决**:
- 检查 GitHub Secrets 中的 `REACT_APP_API_URL` 是否正确
- 确保后端 URL 以 `/api` 结尾
- 检查 Render 后端是否正在运行（Free 计划在15分钟无活动后会休眠）

### 3. Render 服务休眠

**问题**: 首次访问需要等待很久

**解决**:
- Free 计划的服务在15分钟无活动后会休眠
- 首次访问需要等待约30秒唤醒
- 可以考虑使用付费计划或添加健康检查 ping

### 4. GitHub Pages 404 错误

**问题**: 访问 GitHub Pages 显示 404

**解决**:
- 确认 GitHub Pages 的 Source 设置为 "GitHub Actions"
- 检查 Actions 标签页，确认部署工作流成功完成
- 确认 `client/package.json` 中的 `homepage` 字段正确

### 5. 更新后端 URL

如果后端 URL 发生变化，需要：
1. 更新 GitHub Secrets 中的 `REACT_APP_API_URL`
2. 重新触发 GitHub Actions 部署工作流

## 更新部署

### 更新前端

只需推送代码到 `main` 或 `master` 分支，GitHub Actions 会自动部署。

### 更新后端

在 Render Dashboard 中：
1. 进入你的 Web Service
2. 点击 "Manual Deploy" → "Deploy latest commit"
3. 或直接推送代码到 GitHub，Render 会自动部署（如果启用了自动部署）

## 监控和维护

### 查看日志

- **前端**: GitHub Actions 工作流日志
- **后端**: Render Dashboard → Logs 标签页

### 健康检查

可以定期访问后端健康检查端点，确保服务正常运行。

## 安全建议

1. **保护 API 密钥**: 不要在代码中硬编码 API 密钥
2. **使用环境变量**: 所有敏感信息都通过环境变量配置
3. **定期更新**: 定期更新依赖包以修复安全漏洞

