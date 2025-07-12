# Vercel 部署

此部署方式利用 Vercel 的无服务器环境运行，并**强制要求启用 GitHub 同步**功能以实现数据持久化。

## 前置要求

1. **准备 GitHub 仓库和 PAT**（必需）:
   
   * 你需要一个**自己的** GitHub 仓库来存储同步的数据。建议使用私有仓库。
   * 创建一个 GitHub Personal Access Token (PAT)，并确保勾选了 `repo` 权限范围。**请妥善保管此 Token**。
   * 具体操作步骤详见[GitHub配置同步教程](../GitHub/GitHub同步.md)

2. **Vercel 账户**:
   * 访问 [Vercel](https://vercel.com/) 并注册账户
   * 可以使用 GitHub 账户直接登录

## 部署步骤

### 方式一：通过 GitHub 仓库部署（推荐）

1. **Fork 或克隆项目**:
   ```bash
   git clone https://github.com/dreamhartley/gemini-proxy-panel.git
   cd gemini-proxy-panel
   ```

2. **推送到你的 GitHub 仓库**:
   ```bash
   git remote set-url origin https://github.com/你的用户名/你的仓库名.git
   git push -u origin main
   ```

3. **在 Vercel 中导入项目**:
   * 登录 Vercel 控制台
   * 点击 "New Project"
   * 选择 "Import Git Repository"
   * 选择你刚才推送的仓库
   * 点击 "Import"

4. **配置环境变量**:
   在 Vercel 项目设置中的 "Environment Variables" 部分添加以下变量：

   **必需变量**:
   ```
   ADMIN_PASSWORD=你的管理员密码
   SESSION_SECRET_KEY=一个长且随机的字符串（使用 openssl rand -base64 32 生成）
   GITHUB_PROJECT=你的用户名/数据仓库名
   GITHUB_PROJECT_PAT=你的GitHub Personal Access Token
   GITHUB_ENCRYPT_KEY=32位或更长的加密密钥
   ```

   **可选变量**:
   ```
   GEMINI_BASE_URL=https://generativelanguage.googleapis.com
   PROXY=socks5://user:pass@proxy1.com:1080（如果需要代理）
   ```

5. **部署**:
   * 配置完环境变量后，点击 "Deploy"
   * Vercel 会自动构建和部署你的应用

### 方式二：通过 Vercel CLI 部署

1. **安装 Vercel CLI**:
   ```bash
   npm install -g vercel
   ```

2. **登录 Vercel**:
   ```bash
   vercel login
   ```

3. **在项目目录中部署**:
   ```bash
   vercel
   ```

4. **配置环境变量**:
   ```bash
   vercel env add ADMIN_PASSWORD
   vercel env add SESSION_SECRET_KEY
   vercel env add GITHUB_PROJECT
   vercel env add GITHUB_PROJECT_PAT
   vercel env add GITHUB_ENCRYPT_KEY
   ```

## 重要注意事项

1. **数据持久化**: Vercel 是无服务器环境，不支持本地文件存储。因此**必须**配置 GitHub 同步功能来实现数据持久化。

2. **冷启动**: 由于 Vercel 的无服务器特性，应用可能会有冷启动时间。

3. **函数超时**: Vercel 免费版本有 10 秒的函数执行时间限制，付费版本可以延长到 60 秒。

4. **域名**: 部署成功后，Vercel 会提供一个 `.vercel.app` 域名，你也可以绑定自定义域名。

## 访问应用

部署成功后：
* 管理界面: `https://你的应用名.vercel.app/admin`
* API 端点: `https://你的应用名.vercel.app/v1`
* 登录页面: `https://你的应用名.vercel.app/login.html`

## 故障排除

1. **部署失败**: 检查环境变量是否正确配置，特别是 GitHub 相关变量。

2. **数据丢失**: 确保 GitHub 同步功能正常工作，检查 GitHub 仓库中是否有数据文件。

3. **函数超时**: 如果遇到超时问题，考虑升级到 Vercel Pro 计划。

4. **静态文件访问问题**: 确保 `vercel.json` 配置正确，静态文件路由设置正确。
