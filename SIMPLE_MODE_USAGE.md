# Simple Mode 使用指南

## 概述

Simple Mode 是 BentoPDF 的内置功能，用于隐藏所有营销内容，只保留核心的 PDF 工具功能。这对于企业内部使用或需要简洁界面的场景非常有用。

## 启用方式

### 方式 1：使用 npm 脚本（推荐 - 本地开发）

最简单的方式是使用内置的 npm 脚本：

```bash
npm run serve:simple
```

这个命令会：
- 自动设置 `SIMPLE_MODE=true`
- 构建项目
- 在 `http://localhost:3000` 启动预览服务器

### 方式 2：手动设置环境变量构建

```bash
# 设置环境变量并构建
SIMPLE_MODE=true npm run build

# 启动预览服务器
npx serve dist -p 3000
```

或者使用其他静态文件服务器：

```bash
# Python
cd dist
python -m http.server 8000

# Node.js serve
npx serve dist -p 3000
```

### 方式 3：使用 Docker（生产环境）

#### 使用预构建的 Simple Mode 镜像

```bash
# Docker Hub
docker run -p 3000:8080 bentopdf/bentopdf-simple:latest

# GitHub Container Registry
docker run -p 3000:8080 ghcr.io/alam00000/bentopdf-simple:latest
```

#### 使用 Docker Compose

编辑 `docker-compose.yml`：

```yaml
services:
  bentopdf:
    image: bentopdf/bentopdf-simple:latest  # 使用 Simple Mode 镜像
    container_name: bentopdf
    restart: unless-stopped
    ports:
      - '3000:8080'
```

然后运行：

```bash
docker-compose up -d
```

#### 从源码构建 Simple Mode 镜像

```bash
docker build --build-arg SIMPLE_MODE=true -t bentopdf-simple .
docker run -p 3000:8080 bentopdf-simple
```

### 方式 4：使用 Docker Compose 开发模式

```bash
docker compose -f docker-compose.dev.yml build --build-arg SIMPLE_MODE=true
docker compose -f docker-compose.dev.yml up -d
```

## Simple Mode 效果

启用 Simple Mode 后，以下内容会被隐藏：

- ✅ **导航栏** - 替换为只包含 Logo 的简化导航栏
- ✅ **Hero 区域** - "The PDF Toolkit built for privacy" 等营销内容
- ✅ **特性展示区域** - "Why BentoPDF?" 卡片
- ✅ **安全合规区域** - GDPR/CCPA/HIPAA 合规说明
- ✅ **FAQ 区域** - 常见问题部分
- ✅ **用户评价区域** - 用户评价卡片
- ✅ **支持区域** - "Buy Me a Coffee" 按钮
- ✅ **完整页脚** - 替换为简化版页脚（仅保留版权信息）
- ✅ **GitHub Stars** - GitHub 星标数显示
- ✅ **控制台消息** - 营销相关的控制台输出

**保留的内容：**

- ✅ 所有 PDF 工具功能（完全保留）
- ✅ 工具搜索栏
- ✅ 工具分类和卡片
- ✅ 简化的 Logo 导航栏
- ✅ 简化的页脚（版权信息）

## 页面变化

启用 Simple Mode 后，页面标题会变为：
- **正常模式**: "BentoPDF - The Privacy First PDF Toolkit"
- **Simple Mode**: "BentoPDF - PDF Tools"

工具区域的标题和副标题也会更新：
- **标题**: "PDF Tools"
- **副标题**: "Select a tool to get started"

## 验证 Simple Mode 是否生效

启用 Simple Mode 后，你应该看到：

✅ **应该看到：**
- 简洁的 "PDF Tools" 标题
- "Select a tool to get started" 副标题
- 工具搜索栏
- 所有 PDF 工具卡片（按分类组织）
- 简化的导航栏（只有 Logo）
- 简化的页脚

❌ **不应该看到：**
- 完整的导航菜单
- Hero 区域（"The PDF Toolkit built for privacy"）
- "Why BentoPDF?" 特性卡片
- 安全合规说明（GDPR/CCPA/HIPAA）
- FAQ 常见问题
- 用户评价
- "Buy Me a Coffee" 支持按钮
- 完整的页脚链接

## 技术说明

- **构建时配置**: Simple Mode 是构建时配置，不是运行时配置
- **环境变量**: 通过 `SIMPLE_MODE=true` 环境变量在构建时启用
- **代码优化**: Simple Mode 会进行代码优化，移除未使用的营销相关代码
- **功能完整**: 所有 PDF 工具功能在两种模式下完全相同

## 切换模式

要切换回正常模式：

1. **npm 脚本方式**: 使用 `npm run serve` 或 `npm run dev`
2. **环境变量方式**: 不设置 `SIMPLE_MODE` 或设置为 `false`，然后重新构建
3. **Docker 方式**: 使用 `bentopdf/bentopdf:latest` 镜像而不是 `bentopdf/bentopdf-simple:latest`

## 常见问题

**Q: Simple Mode 会影响功能吗？**  
A: 不会。所有 PDF 工具功能在 Simple Mode 下完全可用，只是隐藏了营销内容。

**Q: 可以在运行时切换模式吗？**  
A: 不可以。Simple Mode 是构建时配置，需要重新构建才能切换。

**Q: Simple Mode 的构建产物更小吗？**  
A: 是的，Simple Mode 会进行代码优化，移除未使用的营销相关代码，构建产物会更小。

**Q: 如何确认 Simple Mode 已启用？**  
A: 检查页面标题是否为 "PDF Tools"，以及是否看不到 Hero 区域、FAQ 等营销内容。

## 相关文件

- `vite.config.ts` - 定义 `__SIMPLE_MODE__` 变量
- `src/js/main.ts` - Simple Mode 的隐藏逻辑实现
- `package.json` - `serve:simple` 脚本定义
- `SIMPLE_MODE.md` - Simple Mode 完整文档
- `Dockerfile` - Docker 构建配置
- `docker-compose.yml` - Docker Compose 配置

