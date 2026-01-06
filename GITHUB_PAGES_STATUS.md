# GitHub Pages 部署状态说明

## 当前工作流配置

### 1. Voice Clone 前端部署 (`deploy.yml`)
- **工作流名称**: "Deploy to GitHub Pages"
- **触发条件**: 推送到 `main` 或 `master` 分支
- **部署路径**: `https://yuranz6.github.io/Voice-clone`
- **Concurrency Group**: `pages-voice-clone` (避免与其他工作流冲突)
- **后端 API URL**: `https://voice-clone-9jbz.onrender.com/api`

### 2. EHR Agent 部署 (`deploy-ehr-agent.yml`)
- **工作流名称**: "Deploy EHR Agent to GitHub Pages"
- **触发条件**: 推送到 `main` 分支
- **Concurrency Group**: `pages` (可能与 Voice Clone 冲突)

## 检查部署状态

### 在 GitHub 上查看

1. **进入仓库**: https://github.com/Yuranz6/agent
2. **点击 "Actions" 标签页**
3. **查看工作流运行**:
   - "Deploy to GitHub Pages" - Voice Clone 前端
   - "Deploy EHR Agent to GitHub Pages" - EHR Agent

### 工作流状态说明

- ✅ **绿色勾号**: 部署成功
- ⚠️ **黄色圆圈**: 正在运行中
- ❌ **红色叉号**: 部署失败
- ⏸️ **灰色**: 已取消或等待中

### 查看部署日志

1. 点击工作流运行
2. 点击 "build-and-deploy" job
3. 查看各个步骤的日志

## 常见问题

### 1. "pages-build-deployment" 是什么？

这是 GitHub Pages 的默认构建和部署工作流。如果你看到这个名称，可能是：
- GitHub 自动生成的默认工作流
- 或者是你之前配置的工作流

### 2. 两个工作流会冲突吗？

**可能的问题**:
- `deploy.yml` 和 `deploy-ehr-agent.yml` 都部署到同一个 GitHub Pages
- 后部署的会覆盖先部署的内容

**解决方案**:
- ✅ 已更新 `deploy.yml` 使用独立的 concurrency group: `pages-voice-clone`
- ⚠️ 如果两个应用需要同时部署，考虑：
  1. 使用不同的 GitHub Pages 环境
  2. 或者禁用其中一个工作流
  3. 或者将它们部署到不同的路径

### 3. 如何手动触发部署？

1. 进入 GitHub 仓库 → Actions
2. 选择 "Deploy to GitHub Pages" 工作流
3. 点击 "Run workflow"
4. 选择分支（通常是 `main`）
5. 点击 "Run workflow"

### 4. 部署失败怎么办？

**检查清单**:
- [ ] 查看工作流日志中的错误信息
- [ ] 确认 GitHub Secrets 中的 `REACT_APP_API_URL` 已设置
- [ ] 检查 GitHub Pages 设置（Settings → Pages）
- [ ] 确认 Source 设置为 "GitHub Actions"
- [ ] 检查 `client/package.json` 中的 `homepage` 字段

### 5. 如何验证部署成功？

1. **检查前端**:
   ```
   https://yuranz6.github.io/Voice-clone
   ```

2. **检查后端健康状态**:
   ```bash
   curl https://voice-clone-9jbz.onrender.com/api/health
   ```

3. **检查浏览器控制台**:
   - 打开开发者工具 (F12)
   - 查看 Console 标签页
   - 确认没有错误
   - 确认 API Base URL 正确

## 当前配置摘要

### Voice Clone 前端
- **部署 URL**: `https://yuranz6.github.io/Voice-clone`
- **后端 API**: `https://voice-clone-9jbz.onrender.com/api`
- **工作流文件**: `.github/workflows/deploy.yml`
- **构建路径**: `./client/build`

### 后端 API
- **URL**: `https://voice-clone-9jbz.onrender.com/api`
- **健康检查**: `https://voice-clone-9jbz.onrender.com/api/health`
- **根路径**: `https://voice-clone-9jbz.onrender.com/` (返回 API 信息)

## 更新部署配置

如果需要更新配置：

1. **更新 API URL**:
   - 在 GitHub Secrets 中设置 `REACT_APP_API_URL`
   - 或直接修改 `.github/workflows/deploy.yml` 中的默认值

2. **更新 homepage**:
   - 修改 `client/package.json` 中的 `homepage` 字段

3. **推送更改**:
   ```bash
   git add .
   git commit -m "Update deployment configuration"
   git push origin main
   ```

## 监控部署

- **GitHub Actions**: 查看工作流运行状态
- **GitHub Pages**: Settings → Pages → 查看部署历史
- **Render**: Dashboard → 查看后端服务状态和日志

