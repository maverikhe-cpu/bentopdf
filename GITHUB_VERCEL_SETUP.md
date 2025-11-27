# GitHub 到 Vercel 部署指南

## ✅ 第一步：代码已推送到 GitHub

你的代码已经推送到 GitHub 仓库：`https://github.com/maverikhe-cpu/bentopdf.git`

## 🚀 第二步：在 Vercel 中部署

### 方法 1：通过 Vercel Dashboard（推荐）

#### 1. 登录 Vercel

1. 访问 [vercel.com](https://vercel.com)
2. 点击右上角 **"Sign Up"** 或 **"Log In"**
3. 选择 **"Continue with GitHub"** 使用 GitHub 账号登录
   - 这会授权 Vercel 访问你的 GitHub 仓库

#### 2. 导入项目

1. 登录后，点击 **"Add New..."** → **"Project"**
2. 在 **"Import Git Repository"** 页面，你会看到你的 GitHub 仓库列表
3. 找到 **`bentopdf`** 仓库（或 `maverikhe-cpu/bentopdf`）
4. 点击 **"Import"** 按钮

#### 3. 配置项目

Vercel 会自动检测到这是一个 Vite 项目，配置如下：

- **Framework Preset**: `Vite` ✅（自动检测）
- **Root Directory**: `./` ✅（默认）
- **Build Command**: `npm run build` ✅（已在 vercel.json 中配置）
- **Output Directory**: `dist` ✅（已在 vercel.json 中配置）
- **Install Command**: `npm install` ✅（默认）

**无需修改任何配置，直接点击 "Deploy"！**

#### 4. 等待部署完成

- Vercel 会自动：
  1. 安装依赖（`npm install`）
  2. 构建项目（`npm run build`）
  3. 部署到全球 CDN

- 部署时间通常为 **2-5 分钟**

#### 5. 访问你的网站

部署完成后，你会看到：
- ✅ **Production URL**: `https://bentopdf-xxxxx.vercel.app`
- ✅ 部署状态和日志
- ✅ 可以点击 URL 立即访问

### 方法 2：通过 Vercel CLI

如果你更喜欢使用命令行：

```bash
# 1. 安装 Vercel CLI
npm i -g vercel

# 2. 登录 Vercel（会打开浏览器）
vercel login

# 3. 在项目目录中运行
cd /Users/mingjiehe/Desktop/development/bentopdf
vercel

# 4. 按照提示操作：
#    - Link to existing project? No（首次部署）
#    - What's your project's name? bentopdf
#    - In which directory is your code located? ./
#    - Want to override the settings? No

# 5. 部署到生产环境
vercel --prod
```

## ⚙️ 第三步：可选配置

### 启用 Simple Mode（隐藏营销内容）

如果你想启用 Simple Mode：

1. 在 Vercel Dashboard 中，进入你的项目
2. 点击 **"Settings"** → **"Environment Variables"**
3. 点击 **"Add New"**
4. 添加环境变量：
   - **Name**: `SIMPLE_MODE`
   - **Value**: `true`
   - **Environment**: 选择 `Production`, `Preview`, `Development`（或全部）
5. 点击 **"Save"**
6. 重新部署：进入 **"Deployments"**，点击最新部署右侧的 **"..."** → **"Redeploy"**

### 自定义域名

1. 在项目设置中，点击 **"Domains"**
2. 输入你的域名（如 `bentopdf.example.com`）
3. 按照提示配置 DNS 记录
4. 等待 DNS 生效（通常几分钟到几小时）

## 📊 第四步：验证部署

部署成功后，检查：

- ✅ 网站可以正常访问
- ✅ 所有 PDF 工具功能正常
- ✅ 静态资源（CSS、JS、图片）加载正常
- ✅ 多页面路由正常工作（如 `/about.html`, `/contact.html`）

## 🔄 自动部署

配置完成后，Vercel 会自动：

- ✅ **每次推送到 `main` 分支** → 自动部署到生产环境
- ✅ **创建 Pull Request** → 自动创建预览部署
- ✅ **推送到其他分支** → 自动创建预览部署

## 📝 部署状态检查

在 Vercel Dashboard 中，你可以：

1. **查看部署历史**：所有部署记录
2. **查看构建日志**：详细的构建和部署日志
3. **回滚部署**：如果新部署有问题，可以快速回滚
4. **查看分析**：访问量、性能指标等（需启用 Analytics）

## 🐛 故障排查

### 构建失败

1. **查看构建日志**
   - 在 Vercel Dashboard 中点击失败的部署
   - 查看详细的错误信息

2. **本地测试构建**
   ```bash
   npm run build
   ```
   如果本地构建失败，修复后再推送

3. **检查 Node.js 版本**
   - Vercel 默认使用 Node.js 18.x
   - 如需指定版本，创建 `.nvmrc` 文件：
     ```bash
     echo "18" > .nvmrc
     ```

### 页面 404 错误

- 确认 `vercel.json` 配置正确
- 检查 `dist` 目录中是否有对应的 HTML 文件

### 环境变量未生效

- 确认环境变量名称正确（区分大小写）
- 重新部署项目（环境变量在构建时使用）

## 📚 相关资源

- [Vercel 文档](https://vercel.com/docs)
- [Vite 部署指南](https://vitejs.dev/guide/static-deploy.html#vercel)
- 项目配置文件：`vercel.json`
- 部署文档：`VERCEL_DEPLOY.md`

## 🎉 完成！

部署完成后，你的 BentoPDF 应用就可以通过 Vercel 提供的 URL 访问了！

**下一步建议：**
- 配置自定义域名
- 启用 Vercel Analytics（可选）
- 设置环境变量（如需要 Simple Mode）

