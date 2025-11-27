# Vercel PDF 功能修复指南

## 问题描述

在 Vercel 上重新构建后，PDF 功能不可用。

## 可能的原因

1. **CORS 头配置问题**：`Cross-Origin-Embedder-Policy: require-corp` 要求所有资源都必须有 `Cross-Origin-Resource-Policy: cross-origin` 头
2. **静态资源路径问题**：某些资源路径可能不正确
3. **Worker 文件加载问题**：PDF.js worker 文件可能无法正确加载

## 已应用的修复

### 1. 更新了 vercel.json 配置

为所有静态资源添加了正确的 CORS 头：

- `/assets/*.mjs`, `*.js`, `*.wasm` 等文件：添加了 `Cross-Origin-Resource-Policy: cross-origin`
- `/images/*`：添加了 CORS 头
- `/workers/*`：添加了 CORS 头
- `/pdfjs-viewer/*`：添加了 CORS 头
- `/pdfjs-annotation-viewer/*`：添加了 CORS 头
- `qpdf.wasm` 和 `coherentpdf.browser.min.js`：添加了 CORS 头

### 2. 验证构建

本地构建成功，所有资源文件都正确复制到 `dist` 目录：
- ✅ `dist/images/` - 图片资源
- ✅ `dist/workers/` - Worker 文件
- ✅ `dist/pdfjs-viewer/` - PDF.js 查看器
- ✅ `dist/pdfjs-annotation-viewer/` - PDF.js 注释查看器
- ✅ `dist/qpdf.wasm` - qpdf WASM 文件
- ✅ `dist/assets/pdf.worker.min-*.mjs` - PDF.js worker 文件

## 部署步骤

1. **提交更改**：
   ```bash
   git add vercel.json
   git commit -m "Fix Vercel CORS headers for PDF functionality"
   git push origin main
   ```

2. **在 Vercel 中重新部署**：
   - Vercel 会自动检测到新的提交并触发部署
   - 或者手动在 Vercel Dashboard 中点击 "Redeploy"

3. **验证部署**：
   - 检查浏览器控制台是否有 CORS 错误
   - 测试 PDF 文件上传和处理功能
   - 检查 Network 标签，确认所有资源都返回了正确的 CORS 头

## 故障排查

### 如果 PDF 功能仍然不可用

1. **检查浏览器控制台**：
   - 打开浏览器开发者工具
   - 查看 Console 标签中的错误信息
   - 查看 Network 标签，检查哪些资源加载失败

2. **检查资源路径**：
   - 确认所有资源路径都是相对路径或绝对路径
   - 检查 PDF.js worker 文件是否正确加载

3. **检查 CORS 头**：
   - 在 Network 标签中检查响应头
   - 确认所有资源都有 `Cross-Origin-Resource-Policy: cross-origin` 头

4. **临时禁用 COEP 头**（仅用于测试）：
   如果问题仍然存在，可以临时将 `Cross-Origin-Embedder-Policy` 从 `require-corp` 改为 `unsafe-none` 来测试：
   ```json
   {
     "key": "Cross-Origin-Embedder-Policy",
     "value": "unsafe-none"
   }
   ```
   **注意**：这可能会影响 SharedArrayBuffer 等功能，仅用于测试。

## 相关文件

- `vercel.json` - Vercel 部署配置
- `vite.config.ts` - Vite 构建配置
- `src/js/main.ts` - PDF.js worker 配置

## 技术说明

### Cross-Origin-Embedder-Policy (COEP)

`Cross-Origin-Embedder-Policy: require-corp` 是一个安全策略，要求：
- 所有跨域资源都必须明确声明 `Cross-Origin-Resource-Policy: cross-origin`
- 这对于使用 SharedArrayBuffer 的功能（如 PDF.js）是必需的

### 为什么需要这些头

PDF.js 和其他 PDF 处理库需要：
- `SharedArrayBuffer` 支持（需要 COOP + COEP）
- Worker 文件正确加载
- WASM 文件正确加载

所有这些都需要正确的 CORS 配置。

