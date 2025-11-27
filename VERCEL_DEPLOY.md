# Vercel 部署指南

## 快速部署

### 方法 1：通过 Vercel Dashboard（推荐）

1. **登录 Vercel**
   - 访问 [vercel.com](https://vercel.com)
   - 使用 GitHub/GitLab/Bitbucket 账号登录

2. **导入项目**
   - 点击 "Add New Project"
   - 选择你的 GitHub 仓库（或连接 GitLab/Bitbucket）
   - 选择 `bentopdf` 仓库

3. **配置项目**
   - **Framework Preset**: Vite（会自动检测）
   - **Root Directory**: `./`（默认）
   - **Build Command**: `npm run build`（已配置在 vercel.json）
   - **Output Directory**: `dist`（已配置在 vercel.json）
   - **Install Command**: `npm install`（默认）

4. **环境变量（可选）**
   - 如果需要启用 Simple Mode，添加环境变量：
     - `SIMPLE_MODE` = `true`

5. **部署**
   - 点击 "Deploy"
   - 等待构建完成
   - 访问提供的 URL

### 方法 2：通过 Vercel CLI

1. **安装 Vercel CLI**
   ```bash
   npm i -g vercel
   ```

2. **登录 Vercel**
   ```bash
   vercel login
   ```

3. **部署**
   ```bash
   # 首次部署（会提示配置）
   vercel

   # 生产环境部署
   vercel --prod
   ```

4. **设置环境变量（可选）**
   ```bash
   # 启用 Simple Mode
   vercel env add SIMPLE_MODE
   # 输入: true
   ```

## 配置说明

### vercel.json

项目已包含 `vercel.json` 配置文件，包含以下设置：

- **构建命令**: `npm run build`
- **输出目录**: `dist`
- **路由重写**: 所有路由重定向到 `index.html`（支持 SPA）
- **安全头**: 配置了必要的安全响应头
- **缓存策略**: 静态资源（assets）设置长期缓存

### 环境变量

#### SIMPLE_MODE

启用 Simple Mode（隐藏营销内容）：

- **值**: `true` 或 `false`
- **默认**: `false`
- **说明**: 在构建时启用 Simple Mode，隐藏所有营销内容

在 Vercel Dashboard 中设置：
1. 进入项目设置
2. 选择 "Environment Variables"
3. 添加 `SIMPLE_MODE` = `true`

## 部署后验证

部署成功后，检查以下内容：

1. ✅ 网站可以正常访问
2. ✅ 所有 PDF 工具功能正常
3. ✅ 静态资源加载正常（CSS、JS、图片）
4. ✅ 路由正常工作（直接访问子页面）
5. ✅ 安全头已正确设置

## 自定义域名

1. 在 Vercel Dashboard 中进入项目设置
2. 选择 "Domains"
3. 添加你的自定义域名
4. 按照提示配置 DNS 记录

## 持续部署

Vercel 会自动：
- 监听 Git 推送
- 在每次推送时自动构建和部署
- 为每个 Pull Request 创建预览部署

### 分支部署

- **main/master 分支**: 自动部署到生产环境
- **其他分支**: 自动创建预览部署

## 构建优化

### 构建时间优化

- Vercel 会缓存 `node_modules`
- 使用 `npm ci` 可以加快安装速度（可选）

### 输出大小

- 构建产物在 `dist` 目录
- 已配置静态资源缓存策略
- 使用 Vite 的代码分割优化

## 故障排查

### 构建失败

1. **检查构建日志**
   - 在 Vercel Dashboard 查看详细错误信息

2. **本地测试构建**
   ```bash
   npm run build
   ```

3. **检查 Node.js 版本**
   - Vercel 默认使用 Node.js 18.x
   - 如需指定版本，创建 `.nvmrc` 文件

### 路由问题

如果直接访问子页面返回 404：

1. 确认 `vercel.json` 中的 `rewrites` 配置正确
2. 检查 `dist` 目录中是否有对应的 HTML 文件

### 环境变量未生效

1. 确认环境变量名称正确（区分大小写）
2. 重新部署项目（环境变量在构建时使用）
3. 检查构建日志确认环境变量已加载

## 性能优化建议

1. **启用 Vercel Analytics**（可选）
   - 在项目设置中启用
   - 查看性能指标

2. **配置 Edge Functions**（如需要）
   - 对于需要服务端逻辑的功能

3. **使用 Vercel Image Optimization**（如需要）
   - 自动优化图片加载

## 相关文件

- `vercel.json` - Vercel 部署配置
- `package.json` - 项目依赖和脚本
- `vite.config.ts` - Vite 构建配置

## 注意事项

1. **构建产物大小**
   - 首次部署可能较慢（需要安装依赖）
   - 后续部署会使用缓存，速度更快

2. **环境变量**
   - 环境变量在构建时使用
   - 修改环境变量后需要重新部署

3. **静态资源路径**
   - 确保所有资源路径使用相对路径或绝对路径
   - Vite 会自动处理资源路径

4. **CORS 和 COOP/COEP 头**
   - 已配置必要的安全头
   - 这些头对于 SharedArrayBuffer 等功能是必需的

