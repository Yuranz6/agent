# 重新部署说明

所有配置已更新为新的 URL：
- **前端地址**: `https://yuranz6.github.io/Voice-clone`
- **后端 CLIENT_URL**: `https://yuranz6.github.io`（不包含路径）

## 已更新的文件

✅ `client/package.json` - homepage 已更新
✅ `render.yaml` - CLIENT_URL 已更新
✅ `server/index.js` - 默认 CORS 配置已更新
✅ 所有文档文件中的 URL 引用已更新

## 重新部署步骤

### 1. 后端部署到 Render（yuranzhang6@gmail.com 账户）

#### 方式一：使用 render.yaml（推荐）

1. 登录 Render（使用 yuranzhang6@gmail.com）
   - 访问 https://render.com
   - 使用 GitHub 账号登录

2. 应用 Manifest
   - 点击 "New" → "Apply Manifest"
   - 选择 GitHub 仓库 `yuranz6/Voice-clone`
   - Render 会自动读取 `render.yaml` 配置

3. 配置环境变量
   在环境变量部分设置：
   - `ELEVENLABS_API_KEY`: 你的 ElevenLabs API 密钥
   - `GOOGLE_AI_API_KEY`: 你的 Google AI API 密钥
   - `ELEVENLABS_VOICE_ID`: （可选）默认语音 ID

4. 确认 CLIENT_URL
   - 确保 `CLIENT_URL` 环境变量为 `https://yuranz6.github.io`
   - 已在 `render.yaml` 中配置，但请确认

5. 点击 "Apply" 开始部署

#### 方式二：手动创建新服务

1. 在 Render Dashboard 点击 "New" → "Web Service"
2. 连接 GitHub 仓库 `yuranz6/Voice-clone`
3. 配置：
   - **Name**: `voice-clone-backend`（或自定义）
   - **Environment**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `node server/index.js`
   - **Plan**: Free（或根据需要选择）

4. 环境变量：
   ```
   NODE_ENV=production
   PORT=10000
   CLIENT_URL=https://yuranz6.github.io
   ELEVENLABS_API_KEY=your_key_here
   GOOGLE_AI_API_KEY=your_key_here
   ELEVENLABS_VOICE_ID=your_voice_id（可选）
   ```

5. 创建服务并等待部署完成

6. **重要**: 记下后端 URL（例如：`https://voice-clone-backend-xxxx.onrender.com`）

### 2. 前端部署到 GitHub Pages

1. **配置 GitHub Secrets**
   - 进入仓库 `yuranz6/Voice-clone`
   - Settings → Secrets and variables → Actions
   - 添加或更新 secret：
     - **Name**: `REACT_APP_API_URL`
     - **Value**: `https://your-render-backend-url.onrender.com/api`
     - （将 `your-render-backend-url` 替换为步骤 1 中获取的实际 Render 后端 URL）

2. **启用 GitHub Pages**
   - Settings → Pages
   - Source 选择 "GitHub Actions"
   - 保存设置

3. **推送代码触发部署**
   ```bash
   git add .
   git commit -m "Update deployment URLs to yuranz6.github.io/Voice-clone"
   git push origin main
   ```

4. **查看部署状态**
   - 进入仓库的 Actions 标签页
   - 查看 "Deploy to GitHub Pages" 工作流
   - 等待部署完成（约 3-5 分钟）

5. **访问前端**
   - 部署完成后访问：`https://yuranz6.github.io/Voice-clone`

## 验证部署

### 检查后端
```bash
curl https://your-render-backend-url.onrender.com/api/health
```
应该返回：`{"status":"ok","message":"Voice Clone API is running"}`

### 检查前端
1. 访问 `https://yuranz6.github.io/Voice-clone`
2. 打开浏览器开发者工具（F12）
3. 查看 Console，确认：
   - API Base URL 指向正确的 Render 后端
   - 没有 CORS 错误
   - 没有连接错误

## 重要提示

⚠️ **CORS 配置**
- Render 后端的 `CLIENT_URL` 必须设置为 `https://yuranz6.github.io`
- **不要**包含 `/Voice-clone` 路径
- 后端代码会自动处理子路径

⚠️ **API URL 配置**
- GitHub Secrets 中的 `REACT_APP_API_URL` 必须以 `/api` 结尾
- 例如：`https://voice-clone-backend-xxxx.onrender.com/api`

⚠️ **Render Free 计划**
- 服务在 15 分钟无活动后会休眠
- 首次访问需要等待约 30 秒唤醒
- 考虑使用付费计划或添加健康检查 ping

## 如果遇到问题

### CORS 错误
- 检查 Render 后端的 `CLIENT_URL` 环境变量
- 确保值为 `https://yuranz6.github.io`（不包含路径）

### 前端无法连接后端
- 检查 GitHub Secrets 中的 `REACT_APP_API_URL`
- 确保 URL 以 `/api` 结尾
- 检查 Render 后端是否正在运行

### GitHub Pages 404
- 确认 GitHub Pages Source 设置为 "GitHub Actions"
- 检查 Actions 工作流是否成功完成
- 确认 `client/package.json` 中的 `homepage` 字段正确

## 更新现有 Render 服务（如果已存在）

如果你已经有 Render 服务，可以：

1. **更新环境变量**
   - 进入 Render Dashboard → 你的服务
   - Environment → 编辑 `CLIENT_URL`
   - 改为：`https://yuranz6.github.io`
   - 保存并重新部署

2. **或者重新部署**
   - 在 Render Dashboard 中
   - 点击 "Manual Deploy" → "Deploy latest commit"
   - 或推送代码到 GitHub，Render 会自动部署

