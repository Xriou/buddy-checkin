# buddy-checkin（云端自动签到）

每天北京时间 09:10 在 GitHub Actions 上自动领取 WorkBuddy「Buddy加油站」积分。

- 登录态存在仓库 Secret `WB_STATE_JSON`（内容为 state.json，即浏览器登录凭据）
- 签到失败（登录态过期）时 Actions 会发邮件通知
- 更新登录态：在电脑上重新登录后，把新的 state.json 内容更新到 Secrets 即可

## 手动触发

仓库页 → Actions → buddy-checkin → Run workflow
