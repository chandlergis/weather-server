# 分支管理指南 - Weather Server

本指南说明如何管理GitHub仓库的分支，特别是从master迁移到main分支。

## 当前状态

您现在有两个分支：
- `master` - 原始分支
- `main` - 新的主分支（推荐使用）

## 在GitHub上设置main为默认分支

### 步骤1：访问仓库设置

1. 打开您的GitHub仓库：https://github.com/chandlergis/weather-server
2. 点击"Settings"标签页
3. 在左侧菜单中找到"Branches"

### 步骤2：更改默认分支

1. 在"Default branch"部分，点击分支名称旁边的切换按钮
2. 选择"main"作为新的默认分支
3. 点击"Update"
4. 在确认对话框中点击"I understand, update the default branch"

### 步骤3：删除master分支（可选）

**注意：只有在确认main分支包含所有内容后才执行此步骤**

在GitHub上删除master分支：
1. 在仓库主页，点击分支下拉菜单
2. 找到master分支
3. 点击垃圾桶图标删除

在本地删除master分支：
```bash
# 确保当前在main分支
git checkout main

# 删除本地master分支
git branch -d master

# 删除远程master分支
git push origin --delete master
```

## 本地分支管理命令

### 查看所有分支
```bash
# 查看本地分支
git branch

# 查看所有分支（包括远程）
git branch -a

# 查看远程分支
git branch -r
```

### 分支切换
```bash
# 切换到main分支
git checkout main

# 创建并切换到新分支
git checkout -b feature/new-feature

# 切换到已存在的分支
git checkout existing-branch
```

### 分支合并
```bash
# 将feature分支合并到main
git checkout main
git merge feature/new-feature

# 删除已合并的分支
git branch -d feature/new-feature
```

### 同步远程分支
```bash
# 获取远程分支信息
git fetch origin

# 拉取最新的main分支
git pull origin main

# 推送本地分支到远程
git push origin branch-name
```

## 分支命名规范

### 推荐的分支命名：

**功能分支：**
```
feature/weather-cache
feature/error-handling
feature/batch-queries
```

**修复分支：**
```
fix/api-timeout
fix/memory-leak
hotfix/critical-bug
```

**发布分支：**
```
release/v1.1.0
release/v2.0.0
```

**文档分支：**
```
docs/api-reference
docs/usage-examples
```

## 工作流程示例

### 开发新功能的完整流程：

```bash
# 1. 确保在最新的main分支
git checkout main
git pull origin main

# 2. 创建功能分支
git checkout -b feature/weather-cache

# 3. 进行开发
# ... 编写代码 ...

# 4. 提交更改
git add .
git commit -m "Add: 实现天气数据缓存功能"

# 5. 推送分支
git push -u origin feature/weather-cache

# 6. 在GitHub创建Pull Request
# 访问GitHub仓库，点击"Compare & pull request"

# 7. 合并后清理（在GitHub合并PR后）
git checkout main
git pull origin main
git branch -d feature/weather-cache
git push origin --delete feature/weather-cache
```

## 紧急修复流程

```bash
# 1. 从main创建hotfix分支
git checkout main
git checkout -b hotfix/critical-api-fix

# 2. 快速修复
# ... 修复代码 ...

# 3. 提交并推送
git add .
git commit -m "Fix: 修复API超时问题"
git push -u origin hotfix/critical-api-fix

# 4. 创建PR并立即合并
# 5. 部署修复版本
```

## 版本发布流程

```bash
# 1. 创建发布分支
git checkout main
git checkout -b release/v1.1.0

# 2. 更新版本号
# 编辑package.json中的version字段

# 3. 提交版本更新
git add package.json
git commit -m "Bump version to 1.1.0"

# 4. 合并到main
git checkout main
git merge release/v1.1.0

# 5. 创建标签
git tag -a v1.1.0 -m "Release version 1.1.0"

# 6. 推送所有内容
git push origin main
git push origin v1.1.0

# 7. 在GitHub创建Release
```

## 常见问题解决

### Q: 如何撤销合并？
```bash
# 撤销最后一次合并（如果还没推送）
git reset --hard HEAD~1

# 如果已经推送，创建反向合并
git revert -m 1 HEAD
```

### Q: 如何解决合并冲突？
```bash
# 1. 合并时出现冲突
git merge feature-branch
# Auto-merging file.txt
# CONFLICT (content): Merge conflict in file.txt

# 2. 手动编辑冲突文件，解决冲突标记
# <<<<<<< HEAD
# 当前分支的内容
# =======
# 要合并分支的内容
# >>>>>>> feature-branch

# 3. 标记冲突已解决
git add file.txt

# 4. 完成合并
git commit
```

### Q: 如何重命名分支？
```bash
# 重命名当前分支
git branch -m new-branch-name

# 重命名其他分支
git branch -m old-name new-name

# 更新远程分支
git push origin :old-name new-name
git push origin -u new-name
```

## 最佳实践

1. **保持main分支稳定**：只合并经过测试的代码
2. **使用描述性分支名**：清楚表达分支用途
3. **定期清理分支**：删除已合并的功能分支
4. **小而频繁的提交**：便于代码审查和回滚
5. **写清楚的提交信息**：帮助团队理解更改内容

## GitHub分支保护规则

建议在GitHub上设置分支保护规则：

1. 访问Settings > Branches
2. 点击"Add rule"
3. 设置规则：
   - Branch name pattern: `main`
   - ✅ Require pull request reviews before merging
   - ✅ Require status checks to pass before merging
   - ✅ Require branches to be up to date before merging
   - ✅ Include administrators

这样可以确保所有对main分支的更改都经过代码审查。

---

现在您的仓库已经正确设置了main作为主分支！
