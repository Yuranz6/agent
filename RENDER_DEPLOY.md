# Render 部署指南

本指南将帮助你将 EHR Agent 部署到 Render。

## 前置条件

- GitHub 账号
- Render 账号（可使用 GitHub 登录）
- Google API Key（用于 Gemini AI）
- ElevenLabs API Key（可选，用于文本转语音）

## 部署步骤

### 方式一：使用 render.yaml（推荐）

1. **登录 Render**
   - 访问 https://render.com
   - 使用 GitHub 账号登录

2. **创建新服务**
   - 点击 "New" → "Blueprint"
   - 连接 GitHub 仓库 `Yuranz6/agent`
   - Render 会自动检测 `render.yaml` 文件

3. **配置环境变量**
   - 在服务设置中找到 "Environment Variables"
   - 添加以下环境变量：
     ```
     GOOGLE_API_KEY=你的Google_API_Key
     ELEVENLABS_API_KEY=你的ElevenLabs_API_Key（可选）
     GEMINI_MODEL=gemini-2.5-flash
     ```
   - 注意：`GOOGLE_API_KEY` 和 `ELEVENLABS_API_KEY` 需要手动设置（sync: false）

4. **部署**
   - 点击 "Apply" 开始部署
   - 等待部署完成（约 5-10 分钟）

### 方式二：手动创建 Web Service

1. **登录 Render**
   - 访问 https://render.com
   - 使用 GitHub 账号登录

2. **创建 Web Service**
   - 点击 "New" → "Web Service"
   - 连接 GitHub 仓库 `Yuranz6/agent`

3. **配置服务**
   - **Name**: `ehr-agent-backend`（或自定义）
   - **Environment**: `Python 3`
   - **Region**: 选择最近的区域（如 Singapore）
   - **Branch**: `main`
   - **Root Directory**: 留空（使用项目根目录）
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `gunicorn app:app --bind 0.0.0.0:$PORT --workers 2 --threads 2 --timeout 120`
   - **Plan**: Free（或根据需要选择付费计划）

4. **配置环境变量**
   在 "Environment Variables" 部分添加：
   ```
   GOOGLE_API_KEY=你的Google_API_Key
   ELEVENLABS_API_KEY=你的ElevenLabs_API_Key（可选）
   GEMINI_MODEL=gemini-2.5-flash
   ```

5. **部署**
   - 点击 "Create Web Service"
   - Render 会自动开始部署
   - 部署完成后会获得服务 URL，例如：`https://ehr-agent-backend.onrender.com`

## 验证部署

### 1. 检查健康状态

访问健康检查端点：
```bash
curl https://your-service-url.onrender.com/health
```

应该返回：
```json
{"status": "ok", "message": "EHR Agent API is running"}
```

### 2. 访问 Web 界面

在浏览器中打开：
```
https://your-service-url.onrender.com
```

### 3. 测试 API

测试 SOAP 生成接口：
```bash
curl -X POST https://your-service-url.onrender.com/api/generate-soap \
  -H "Content-Type: application/json" \
  -d '{"transcription": "患者主诉头痛三天"}'
```

## 注意事项

1. **免费计划限制**
   - Render 免费计划在 15 分钟无活动后会自动休眠
   - 首次访问休眠服务需要等待约 30-60 秒唤醒
   - 建议升级到付费计划以获得更好的性能

2. **环境变量安全**
   - 不要在代码中硬编码 API Key
   - 使用 Render 的环境变量功能安全存储敏感信息
   - `.env` 文件已在 `.gitignore` 中，不会被提交

3. **端口配置**
   - Render 会自动设置 `PORT` 环境变量
   - 应用会自动使用该端口，无需手动配置

4. **依赖安装**
   - `pyaudio` 已从 `requirements.txt` 中移除，因为 Web 应用使用浏览器的 Web Speech API，不需要服务器端的音频录制功能
   - 如果需要本地命令行版本的语音录制功能，可以手动安装：`pip install pyaudio`（需要先安装 PortAudio 系统库）

5. **日志查看**
   - 在 Render Dashboard 中可以查看实时日志
   - 有助于调试部署问题

## 故障排除

### 部署失败

1. **检查构建日志**
   - 在 Render Dashboard 中查看构建日志
   - 确认所有依赖都正确安装

2. **检查启动命令**
   - 确保 `gunicorn` 已添加到 `requirements.txt`
   - 确认启动命令格式正确

3. **检查环境变量**
   - 确认所有必需的环境变量都已设置
   - 检查 API Key 是否正确

### 服务无法访问

1. **检查服务状态**
   - 在 Dashboard 中确认服务状态为 "Live"
   - 如果显示 "Sleeping"，等待自动唤醒或手动唤醒

2. **检查健康检查端点**
   - 访问 `/health` 端点确认服务正常运行

3. **查看日志**
   - 检查实时日志中的错误信息

## 更新部署

当你推送新代码到 GitHub 时，Render 会自动：
1. 检测到代码变更
2. 重新构建应用
3. 重新部署服务

你可以在 Dashboard 中查看部署进度。

## 获取帮助

如果遇到问题：
1. 查看 Render Dashboard 中的日志
2. 检查 [Render 文档](https://render.com/docs)
3. 查看项目的 GitHub Issues

