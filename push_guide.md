# 子模块修改推送指南

## 重要说明
GitHub 已经不再支持密码认证，需要使用个人访问令牌 (Personal Access Token) 进行认证。

## 步骤 1: 生成个人访问令牌
1. 登录 GitHub
2. 进入 Settings -> Developer settings -> Personal access tokens
3. 点击 "Generate new token"
4. 设置合适的过期时间
5. 勾选 "repo" 权限（包含所有子权限）
6. 点击 "Generate token"
7. **复制令牌并保存，离开页面后将无法再次查看**

## 步骤 2: 配置子模块的远程仓库

### 方式 A: 将子模块推送到您自己的仓库
如果您想将子模块推送到自己的仓库，需要先在 GitHub 上创建对应的仓库，然后执行：

```bash
# 进入子模块目录
cd libs/hbb_common

# 添加您自己的远程仓库（替换为您的GitHub用户名和仓库名）
git remote add my_remote https://github.com/your_username/hbb_common.git

# 切换到主分支（如果还没有）
git checkout main

# 提交子模块的修改
git add src/config.rs
git commit -m "Update rendezvous server configuration"

# 推送到您的远程仓库（使用个人访问令牌作为密码）
git push my_remote main
```

### 方式 B: 如果您只是修改了原始仓库的子模块

```bash
# 进入子模块目录
cd libs/hbb_common

# 提交修改
git add src/config.rs
git commit -m "Update rendezvous server configuration"

# 推送到原始仓库（需要权限，否则会失败）
git push origin main
```

## 步骤 3: 更新主仓库中的子模块引用

```bash
# 返回主仓库目录
cd ..

# 提交子模块引用的更新
git add libs/hbb_common
git commit -m "Update submodule reference to latest commit"

# 推送到您的主仓库（替换为您的远程仓库地址）
git push origin master
```

## 步骤 4: 使用个人访问令牌进行认证

当 Git 提示输入密码时，**不要输入您的 GitHub 密码**，而是输入您在步骤 1 中生成的个人访问令牌。

## 常见问题

### 1. 推送失败：权限被拒绝
- 确保您有目标仓库的推送权限
- 确保使用了正确的个人访问令牌（包含 repo 权限）

### 2. 子模块处于 detached HEAD 状态
- 执行 `git checkout main` 切换到主分支
- 然后再执行提交和推送操作

### 3. 主仓库中的子模块引用没有更新
- 确保在主仓库中执行了 `git add libs/hbb_common` 和 `git commit`

## 注意事项
- 永远不要在代码或配置文件中硬编码您的个人访问令牌
- 定期更新您的个人访问令牌以保证安全
- 推送前确保所有修改都已正确提交

如果您有任何问题，请随时咨询！