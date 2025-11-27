# Vercel 手动触发重新部署指南

## 情况说明

代码已成功推送到 GitHub（提交 `ec6f193`），但 Vercel 没有自动触发重新构建。

## 解决方案

### 方法 1：在 Vercel Dashboard 中手动触发重新部署（推荐）

1. **登录 Vercel Dashboard**
   - 访问 [vercel.com](https://vercel.com)
   - 使用 GitHub 账号登录

2. **进入项目**
   - 在 Dashboard 中找到你的 `bentopdf` 项目
   - 点击项目名称进入项目详情页

3. **查看部署历史**
   - 在项目页面，你会看到 "Deployments" 部分
   - 找到最新的部署记录

4. **手动触发重新部署**
   - 点击最新部署右侧的 **"..."** 菜单（三个点）
   - 选择 **"Redeploy"**
   - 确认重新部署

5. **等待部署完成**
   - 部署通常需要 2-5 分钟
   - 可以在部署详情页查看构建日志

### 方法 2：通过 Vercel CLI 触发重新部署

如果你安装了 Vercel CLI：

```bash
# 1. 登录 Vercel（如果还没登录）
vercel login

# 2. 在项目目录中运行
cd /Users/mingjiehe/Desktop/development/bentopdf

# 3. 触发生产环境重新部署
vercel --prod

# 或者使用 deploy 命令
vercel deploy --prod
```

### 方法 3：检查并修复自动部署设置

如果自动部署没有触发，可能是设置问题：

1. **进入项目设置**
   - 在 Vercel Dashboard 中，进入你的项目
   - 点击 **"Settings"** 标签

2. **检查 Git 集成**
   - 进入 **"Git"** 部分
   - 确认 GitHub 仓库已正确连接
   - 确认 **"Production Branch"** 设置为 `main`

3. **检查部署设置**
   - 进入 **"Deployments"** 部分
   - 确认 **"Automatic deployments from Git"** 已启用
   - 确认 **"Production Branch"** 是 `main`

4. **重新连接仓库（如果需要）**
   - 如果仓库连接有问题，可以：
     - 断开连接
     - 重新连接 GitHub 仓库
     - 重新导入项目

### 方法 4：创建一个空提交来触发部署

如果其他方法都不行，可以创建一个空提交来触发自动部署：

```bash
# 创建一个空提交
git commit --allow-empty -m "Trigger Vercel redeploy"

# 推送到 GitHub
git push origin main
```

## 验证部署

部署完成后，检查：

1. **查看部署日志**
   - 在 Vercel Dashboard 中查看构建日志
   - 确认构建成功，没有错误

2. **测试 PDF 功能**
   - 访问部署的网站
   - 打开浏览器开发者工具（F12）
   - 查看 Console 是否有错误
   - 查看 Network 标签，确认资源加载正常
   - 测试上传 PDF 文件并处理

3. **检查 CORS 头**
   - 在 Network 标签中，点击任意资源文件
   - 查看 Response Headers
   - 确认有 `Cross-Origin-Resource-Policy: cross-origin` 头

## 常见问题

### Q: 为什么 Vercel 没有自动部署？

可能的原因：
1. **项目还没有连接到 GitHub**：需要先在 Vercel 中导入项目
2. **自动部署被禁用**：检查项目设置中的自动部署选项
3. **分支设置错误**：确认 Production Branch 是 `main`
4. **GitHub 集成权限问题**：检查 Vercel 应用的 GitHub 权限

### Q: 如何确认项目已连接到 GitHub？

在 Vercel Dashboard 中：
- 进入项目 → Settings → Git
- 应该能看到连接的 GitHub 仓库信息
- 如果显示 "Not connected"，需要重新连接

### Q: 部署后 PDF 功能仍然不可用怎么办？

1. **检查浏览器控制台错误**
2. **检查 Network 标签中的资源加载情况**
3. **查看 Vercel 部署日志中的错误信息**
4. **参考 `VERCEL_FIX.md` 中的故障排查步骤**

## 快速检查清单

- [ ] 代码已推送到 GitHub（✅ 已完成）
- [ ] Vercel 项目已连接到 GitHub 仓库
- [ ] 自动部署已启用
- [ ] Production Branch 设置为 `main`
- [ ] 手动触发重新部署
- [ ] 部署成功完成
- [ ] PDF 功能测试通过

## 需要帮助？

如果问题仍然存在，请提供：
1. Vercel 部署日志中的错误信息
2. 浏览器控制台中的错误信息
3. Network 标签中失败的资源请求

