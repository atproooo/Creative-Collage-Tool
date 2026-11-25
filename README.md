# 拼贴素材工作台

一个面向电脑和手机的轻量拼贴素材网页工具。当前仅完成阶段 1 的页面框架，导入、裁切和下载等功能尚未开放；完整目标和后续步骤见 [第一版计划](./PROJECT_PLAN.md)。

## 使用 Docker 开发（Windows）

先启动 Docker Desktop，并确保使用 Linux 容器。打开 VS Code 终端，在 PowerShell 中进入项目目录，启动容器：

```powershell
cd D:\Projects\Collage-Tool
docker run --rm -it --mount "type=bind,source=${PWD},target=/app" --mount "type=volume,source=collage-tool-node-modules,target=/app/node_modules" -w /app -p 127.0.0.1:4173:4173 node:26-alpine sh
```

首次运行时会下载 Node.js 镜像。进入容器后，执行：

```sh
npm install
npm run dev -- --host 0.0.0.0 --port 4173 --strictPort
```

在电脑浏览器打开 <http://localhost:4173/>。修改并保存项目文件后，页面通常会自动刷新。

按 `Ctrl+C` 停止开发服务器，输入 `exit` 退出容器。再次启动时，运行上面的 `docker run` 命令，然后在容器内运行 `npm run dev -- --host 0.0.0.0 --port 4173 --strictPort`；依赖未变化时通常无需重新执行 `npm install`。

项目文件与容器共享，修改会保存在电脑上。依赖保存在 Docker 的 `collage-tool-node-modules` 卷中，与 Windows 的 `node_modules` 分开；如果依赖发生变化，需在容器内重新运行 `npm install`。

要验证生产构建，先用 `Ctrl+C` 停止开发服务器，再在容器内运行：

```sh
npm run build
```

上述端口配置仅允许本机访问，不会向局域网开放。

## 不使用 Docker

也可以在电脑上安装 Node.js 20.19 或更高的 20.x 版本，或者 22.12 及以上版本，并安装 npm。在项目目录中运行：

```powershell
npm.cmd install
npm.cmd run dev
```

使用终端输出的网址打开网页；运行 `npm.cmd run build` 可以验证生产构建。
