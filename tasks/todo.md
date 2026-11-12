# Cloudflare 构建失败修复（pnpm 版本漂移）

## Plan
- [x] 定位根因：Cloudflare 使用 pnpm@9.10.0，不读取 pnpm-workspace.yaml 中的 overrides
- [x] 本地验证：pnpm 12.11.2 + `pnpm install --frozen-lockfile` 通过
- [x] 恢复 package.json 的 `"packageManager": "pnpm@12.11.2"`（被 commit 4ad1ff9 误删）
- [ ] Cloudflare 项目设置环境变量 `PNPM_VERSION=12.11.2`（Production + Preview）
- [ ] push 并重新部署，确认日志显示 pnpm@12.11.2 且依赖安装通过

## Review
- 根因：CI 与本地 pnpm 主版本不一致。pnpm 11+ 起 overrides 的配置位置迁移到 pnpm-workspace.yaml，pnpm 9 读取不到该位置，导致 `--frozen-lockfile` 的配置校验失败（`ERR_PNPM_LOCKFILE_CONFIG_MISMATCH`）。
- 部署验证：待执行。
