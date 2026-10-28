# ocharag-uptime

An outside uptime probe for `rag.hpcs.tools`. Every 10 minutes the `probe` workflow checks:

- `/ops-status`, written every 5 minutes by a CronJob inside the cluster (`k8s/07-ops-status.yaml` in
  the main repo). It fails when any check is false or the file is more than 20 minutes old. A stale
  file means the cluster could not run its own check, for example because a node pool was lost.
- `/health` and `/login` answer.

A failed scheduled run emails the repository owner. The repository is public only because Actions
minutes are free for public repositories, and it holds no secrets.

GitHub disables scheduled workflows in a repository with no activity for 60 days. The `keepalive`
workflow commits a heartbeat on the 1st of each month to prevent that. If the probe is ever disabled
anyway, re-enable it from the Actions tab.

**What this cannot catch:** GitHub sometimes delays or drops scheduled runs, and a run that never
starts sends no email. So a quiet inbox means "no failure reported", not "verified up".

Failure email goes to the account that last changed the workflow's `schedule:` line.
