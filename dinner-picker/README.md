# 晚上吃什么

一个使用 Vue 3 + Vite 创建的独立小程序，用来随机决定今晚吃什么。

## 功能

- 随机抽取晚餐推荐
- 点击候选菜单直接选择
- 添加自定义菜品
- 一键重置默认菜单
- 响应式界面，适配桌面和手机

## 开发

```bash
npm install
npm run dev
```

## 构建

```bash
npm run build
```

## GitHub Pages 自动发布

仓库包含 `.github/workflows/dinner-picker-pages.yml`。当代码推送到 `main` 或 `master` 分支且 `dinner-picker/` 或该工作流发生变化时，GitHub Actions 会自动安装依赖、构建 `dinner-picker/dist`，并发布到 GitHub Pages。

第一次使用前，请在 GitHub 仓库的 **Settings → Pages** 中将发布源设置为 **GitHub Actions**。
