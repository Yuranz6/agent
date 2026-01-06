# 环境变量配置说明

## 重要提示

⚠️ **变量名注意**：代码中使用的是 `GOOGLE_AI_API_KEY`（不是 `GOOGLE_API_KEY`）

你提供的密钥：
- `GOOGLE_API_KEY` → 在 Render 中应配置为 `GOOGLE_AI_API_KEY`
- `ELEVENLABS_API_KEY` → 直接使用
- `GEMINI_MODEL` → 已设置为 `gemini-2.5-flash`

## Render 环境变量配置

在 Render Dashboard 中配置以下环境变量：

### 必需的环境变量

1. **GOOGLE_AI_API_KEY**
   ```
   AIzaSyCEjiKwGd9GeFYEEQKiVss3TfQPLVmNaHU
   ```
   ⚠️ 注意：变量名必须是 `GOOGLE_AI_API_KEY`（不是 `GOOGLE_API_KEY`）

2. **ELEVENLABS_API_KEY**
   ```
   sk_dde56e46229c49b817daef72d4986b7ac2f3e464e08c74f2
   ```

3. **CLIENT_URL**
   ```
   https://yuranz6.github.io
   ```
   （已在 render.yaml 中配置）

4. **NODE_ENV**
   ```
   production
   ```
   （已在 render.yaml 中配置）

5. **PORT**
   ```
   10000
   ```
   （已在 render.yaml 中配置）

### 可选的环境变量

6. **GEMINI_MODEL**
   ```
   gemini-2.5-flash
   ```
   （已在 render.yaml 中配置，默认值）

7. **ELEVENLABS_VOICE_ID**
   ```
   （可选）默认语音 ID
   ```

## 在 Render 中配置步骤

### 方式一：使用 render.yaml（推荐）

1. 登录 Render（yuranzhang6@gmail.com）
2. 点击 "New" → "Apply Manifest"
3. 选择仓库 `yuranz6/Voice-clone`
4. 在环境变量部分，手动设置以下密钥：
   - `GOOGLE_AI_API_KEY` = `AIzaSyCEjiKwGd9GeFYEEQKiVss3TfQPLVmNaHU`
   - `ELEVENLABS_API_KEY` = `sk_dde56e46229c49b817daef72d4986b7ac2f3e464e08c74f2`
5. 点击 "Apply" 开始部署

### 方式二：在现有服务中更新

1. 进入 Render Dashboard
2. 选择你的 Web Service
3. 点击 "Environment" 标签页
4. 添加或更新以下环境变量：

   | Key | Value |
   |-----|-------|
   | `GOOGLE_AI_API_KEY` | `AIzaSyCEjiKwGd9GeFYEEQKiVss3TfQPLVmNaHU` |
   | `ELEVENLABS_API_KEY` | `sk_dde56e46229c49b817daef72d4986b7ac2f3e464e08c74f2` |
   | `GEMINI_MODEL` | `gemini-2.5-flash` |
   | `CLIENT_URL` | `https://yuranz6.github.io` |
   | `NODE_ENV` | `production` |
   | `PORT` | `10000` |

5. 保存后，Render 会自动重新部署

## 验证配置

部署完成后，检查后端健康状态：

```bash
curl https://your-render-backend-url.onrender.com/api/health
```

应该返回：
```json
{"status":"ok","message":"Voice Clone API is running"}
```

## 安全提示

⚠️ **重要**：
- 这些 API 密钥是敏感信息，不要提交到 Git 仓库
- 只在 Render 环境变量中配置
- 不要在任何公开的地方分享这些密钥
- 如果密钥泄露，立即在对应服务中重新生成新密钥

## 本地开发配置

如果需要本地开发，创建 `.env` 文件（不要提交到 Git）：

```env
GOOGLE_AI_API_KEY=AIzaSyCEjiKwGd9GeFYEEQKiVss3TfQPLVmNaHU
ELEVENLABS_API_KEY=sk_dde56e46229c49b817daef72d4986b7ac2f3e464e08c74f2
GEMINI_MODEL=gemini-2.5-flash
PORT=3001
NODE_ENV=development
CLIENT_URL=http://localhost:3000
```

确保 `.env` 文件在 `.gitignore` 中（应该已经存在）。

