# buddy-checkin

GitHub Actions 每日自动领取 WorkBuddy「Buddy加油站」积分，无需自己的电脑开机。

## 它做什么

- 每天北京时间 09:10 自动运行（cron: UTC 01:10，可自行修改）
- 用 Playwright 无头浏览器带着你的登录态调用签到接口
- 当天已签 / 活动未开始时自动跳过，不会报错刷屏
- 登录态失效导致运行失败时，GitHub 会自动发邮件提醒你重新登录

## 使用方法

1. Fork 本仓库（或新建仓库后上传这两个文件）
2. 本地安装依赖并登录一次：

   ```bash
   pip install "playwright==1.58.0"
   playwright install chromium
   python checkin.py --login
   ```

   会弹出浏览器，在里面登录 WorkBuddy 后自动保存 `state.json`（内容为登录凭据）
3. 回到仓库：Settings → Secrets and variables → Actions → New repository secret
   - Name 填 `WB_STATE_JSON`
   - Secret 粘贴 `state.json` 的全部内容
4. Fork 来的仓库需在 Actions 页面手动点击启用 workflow
5. 手动试跑：Actions → buddy-checkin → Run workflow

## 更新登录态

Cookie 过期（收到 Actions 失败邮件）后：本地重新执行 `python checkin.py --login`，
把新生成的 `state.json` 内容更新到 Secret `WB_STATE_JSON` 即可，其余不用动。

## 安全说明

- 登录凭据只存放在 GitHub Encrypted Secrets 中，运行日志里自动打码，不会出现在代码或输出里
- `state.json` 已列入 `.gitignore`，请勿手动提交或分享给他人
- 仅供个人自动化学习使用，请遵守目标网站服务条款；活动下线或接口变更时脚本可能失效

## 文件结构

```
├── checkin.py                    # 签到脚本（--login 登录 / 无头签到两种模式）
├── .github/workflows/checkin.yml # 定时任务定义
└── .gitignore                    # 防止登录凭据误提交
```
