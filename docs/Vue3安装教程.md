# Vue3 安装教程

## 1. 安装 Node.js

```bash
# Download and install nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# in lieu of restarting the shell
\. "$HOME/.nvm/nvm.sh"

# Download and install Node.js:
nvm install 22

# Verify the Node.js version:
node -v # Should print "v22.17.1".
nvm current # Should print "v22.17.1".

# Download and install pnpm:
corepack enable pnpm

# Verify pnpm version:
pnpm -v

```

## 2. 安装脚手架并创建项目

安装并执行 create-vue，它是 Vue 官方的项目脚手架工具:

```bash
pnpm create vue@latest
```

你将会看到一些诸如 TypeScript 和测试支持之类的可选功能提示：

```bash
✔ Project name: … <your-project-name>
✔ Add TypeScript? … No / Yes
✔ Add JSX Support? … No / Yes
✔ Add Vue Router for Single Page Application development? … No / Yes
✔ Add Pinia for state management? … No / Yes
✔ Add Vitest for Unit testing? … No / Yes
✔ Add an End-to-End Testing Solution? … No / Cypress / Nightwatch / Playwright
✔ Add ESLint for code quality? … No / Yes
✔ Add Prettier for code formatting? … No / Yes
✔ Add Vue DevTools 7 extension for debugging? (experimental) … No / Yes

Scaffolding project in ./<your-project-name>...
Done.
```

如果不确定是否要开启某个功能，你可以直接按下回车键选择 No。在项目被创建后，通过以下步骤安装依赖并启动开发服务器：

```bash
cd <your-project-name>
pnpm install # 因为网络问题临时切换镜像：pnpm config set registry https://registry.npmmirror.com
pnpm run dev
```

你现在应该已经运行起来了你的第一个 Vue 项目！

当你准备将应用发布到生产环境时，请运行：

```bash
pnpm run build
```

此命令会在 ./dist 文件夹中为你的应用创建一个生产环境的构建版本。