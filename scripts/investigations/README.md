# 历史调查脚本

这里保存针对特定历史版本的对照与取证脚本，不加入常规回归入口。

`round-review-20261115.mjs` 保留 2026-11-15 修复审查的六项检查，以及固定基线
`455f3ab744ba360b50f7af80803e91d2eaee15be` 的对照逻辑。它会拉取该提交，在独立目录中
解压基线与当前 `HEAD`，生成差异和回滚脚本，再检查恢复后的历史收藏空态行为。
取证和回滚都在隔离副本上执行，不重置当前工作区。

只有需要复查这次历史修复时，才在已安装依赖和 Chromium 的 GitHub Actions 环境中手动运行：

```sh
node scripts/investigations/round-review-20261115.mjs
```

脚本保留 `GITHUB_ACTIONS` 环境检查；结果写入
`test-results/investigations/round-review-20261115/`。这里记录的是固定历史问题的预期，
不能作为当前代码通用的验证入口。

常规审查继续使用 [`../test-round-review.mjs`](../test-round-review.mjs)，由现有
[`playback.yml`](../../.github/workflows/playback.yml) 的 `review` 场景调用。它只执行当前
代码的七项回归（原有六项和网络设置交互），把结果写入 `test-results/round-review/report.json`，
不拉取历史基线或生成历史回滚证据。
