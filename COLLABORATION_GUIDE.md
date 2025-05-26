# 协作指南 - Weather Server

本指南介绍如何与他人协作开发weather-server项目，包括Pull Request流程。

## 场景1：其他人向您的项目贡献代码

### 贡献者步骤：

1. **Fork您的仓库**
   - 访问 https://github.com/chandlergis/weather-server
   - 点击右上角的"Fork"按钮
   - 选择自己的账户

2. **克隆Fork的仓库**
   ```bash
   git clone https://github.com/CONTRIBUTOR_USERNAME/weather-server.git
   cd weather-server
   ```

3. **创建功能分支**
   ```bash
   git checkout -b feature/new-feature-name
   ```

4. **进行开发**
   - 修改代码
   - 测试功能
   - 确保代码质量

5. **提交更改**
   ```bash
   git add .
   git commit -m "Add: 描述新功能"
   git push origin feature/new-feature-name
   ```

6. **创建Pull Request**
   - 访问GitHub上的Fork仓库
   - 点击"Compare & pull request"
   - 填写PR标题和描述
   - 点击"Create pull request"

### 您作为项目维护者的步骤：

1. **审查PR**
   - 查看代码更改
   - 运行测试
   - 检查功能是否正常

2. **合并PR**
   - 如果满意，点击"Merge pull request"
   - 选择合并方式（通常选择"Squash and merge"）
   - 确认合并

## 场景2：您向其他项目贡献代码

如果您想向其他MCP服务器项目贡献代码：

1. **Fork目标项目**
2. **克隆到本地**
3. **创建功能分支**
4. **开发新功能**
5. **提交PR**

## 场景3：团队内部协作

### 使用分支进行功能开发：

1. **创建功能分支**
   ```bash
   git checkout -b feature/weather-cache
   ```

2. **开发功能**
   ```bash
   # 进行开发...
   git add .
   git commit -m "Add weather data caching"
   ```

3. **推送分支**
   ```bash
   git push -u origin feature/weather-cache
   ```

4. **在GitHub创建PR**
   - 访问仓库页面
   - GitHub会提示创建PR
   - 填写PR信息并创建

5. **自己审查并合并**
   - 作为项目所有者，您可以直接合并
   - 或者邀请其他人审查

## PR最佳实践

### PR标题格式：
```
类型: 简短描述

例如：
- Add: 新增天气缓存功能
- Fix: 修复API错误处理
- Update: 更新依赖版本
- Docs: 完善README文档
```

### PR描述模板：
```markdown
## 更改内容
- 描述主要更改

## 测试
- [ ] 本地测试通过
- [ ] 添加了单元测试
- [ ] 文档已更新

## 相关Issue
Closes #issue_number
```

## 代码审查清单

作为审查者，检查以下内容：

- [ ] 代码风格一致
- [ ] 功能正常工作
- [ ] 没有破坏现有功能
- [ ] 添加了必要的测试
- [ ] 文档已更新
- [ ] 没有敏感信息泄露

## 分支管理策略

### 推荐的分支结构：
```
master/main     # 主分支，稳定版本
develop         # 开发分支
feature/*       # 功能分支
hotfix/*        # 紧急修复分支
release/*       # 发布分支
```

### 常用Git命令：

```bash
# 查看所有分支
git branch -a

# 切换分支
git checkout branch-name

# 创建并切换到新分支
git checkout -b new-branch

# 合并分支
git merge feature-branch

# 删除分支
git branch -d feature-branch

# 查看提交历史
git log --oneline

# 查看远程仓库
git remote -v
```

## 发布流程

1. **准备发布**
   ```bash
   git checkout master
   git pull origin master
   ```

2. **更新版本号**
   - 修改package.json中的version
   - 更新CHANGELOG.md

3. **创建标签**
   ```bash
   git tag -a v1.1.0 -m "Release version 1.1.0"
   git push origin v1.1.0
   ```

4. **GitHub Release**
   - 在GitHub创建Release
   - 添加发布说明
   - 上传构建文件（如需要）

## 常见问题

**Q: 如何同步Fork的仓库？**
```bash
git remote add upstream https://github.com/chandlergis/weather-server.git
git fetch upstream
git checkout master
git merge upstream/master
git push origin master
```

**Q: 如何撤销提交？**
```bash
# 撤销最后一次提交（保留更改）
git reset --soft HEAD~1

# 撤销最后一次提交（丢弃更改）
git reset --hard HEAD~1
```

**Q: 如何解决合并冲突？**
1. Git会标记冲突文件
2. 手动编辑解决冲突
3. 添加解决后的文件：`git add .`
4. 完成合并：`git commit`

---

这个协作指南将帮助您和其他开发者更好地协作开发weather-server项目！
