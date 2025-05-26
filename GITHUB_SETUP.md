# GitHub设置指南

本指南将帮助您将weather-server项目上传到GitHub并与他人分享。

## 前置准备

1. **GitHub账户**：确保您有GitHub账户
2. **Git安装**：确保本地已安装Git
3. **项目准备**：确保项目已构建成功

## 步骤1：在GitHub上创建仓库

1. 登录GitHub
2. 点击右上角的"+"按钮，选择"New repository"
3. 填写仓库信息：
   - Repository name: `weather-server`
   - Description: `一个基于Model Context Protocol (MCP)的天气查询服务器`
   - 选择Public（公开）或Private（私有）
   - **不要**勾选"Initialize this repository with a README"
4. 点击"Create repository"

## 步骤2：本地Git初始化

在weather-server目录中打开终端，执行以下命令：

```bash
# 初始化Git仓库
git init

# 添加所有文件到暂存区
git add .

# 创建初始提交
git commit -m "Initial commit: Weather Server MCP implementation"

# 添加远程仓库（替换YOUR_USERNAME为您的GitHub用户名）
git remote add origin https://github.com/YOUR_USERNAME/weather-server.git

# 推送到GitHub
git push -u origin main
```

## 步骤3：验证上传

1. 刷新GitHub仓库页面
2. 确认所有文件已上传
3. 检查README.md是否正确显示

## 步骤4：更新README中的链接

上传完成后，需要更新README.md中的占位符：

1. 将所有`YOUR_USERNAME`替换为您的实际GitHub用户名
2. 更新package.json中的repository信息

## 分享给他人

完成上传后，您可以通过以下方式分享：

### 方式1：直接分享仓库链接
```
https://github.com/YOUR_USERNAME/weather-server
```

### 方式2：提供安装命令
```bash
git clone https://github.com/YOUR_USERNAME/weather-server.git
cd weather-server
npm install
cp .env.example .env
# 编辑.env文件添加API密钥
npm run build
npm start
```

### 方式3：创建Release
1. 在GitHub仓库页面点击"Releases"
2. 点击"Create a new release"
3. 填写版本号（如v1.0.0）和发布说明
4. 点击"Publish release"

## 后续维护

### 更新代码
```bash
git add .
git commit -m "描述您的更改"
git push
```

### 创建分支
```bash
git checkout -b feature/new-feature
# 进行开发
git add .
git commit -m "Add new feature"
git push -u origin feature/new-feature
```

## 常见问题

**Q: 推送时要求输入用户名和密码**
A: GitHub现在要求使用Personal Access Token。请到GitHub Settings > Developer settings > Personal access tokens创建token。

**Q: 如何让其他人贡献代码？**
A: 其他人可以Fork您的仓库，进行修改后提交Pull Request。

**Q: 如何处理敏感信息？**
A: 确保.env文件在.gitignore中，永远不要提交API密钥等敏感信息。

## 下一步

1. 完善项目文档
2. 添加单元测试
3. 设置CI/CD流程
4. 考虑发布到npm

---

**提示：** 记得将此文件中的`YOUR_USERNAME`替换为您的实际GitHub用户名！
