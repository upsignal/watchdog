# UpSignal watchdog

Checks UpSignal's `GET /v1/system-health` every 5 minutes from GitHub Actions,
outside the UpSignal server. A failed run means one of these:

- the server, its database or Redis is unreachable;
- the scheduler, worker or notifier stopped;
- checks or alerts are more than 5 minutes behind.

GitHub then emails the person who last changed the workflow file.

Each check tries three times, a minute apart, so a deploy or one slow moment
does not send an alert. GitHub starts scheduled runs late at busy times and
sometimes skips one, so expect an alert within about 10–20 minutes.

## Why a separate public repository

Actions minutes are free in public repositories. Checking every 5 minutes from
the private API repository would use about 8,600 minutes a month. This
repository holds no secrets: the health endpoint is public and shows only
up, down or behind for each part.

GitHub disables schedules in public repositories after 60 days without
activity, so a monthly job re-enables the workflow.

## Set up

1. Create a **public** GitHub repository, for example `upsignal/watchdog`,
   and push this folder to it.
2. Make sure GitHub emails you about failed workflows: Settings →
   Notifications → Actions → **Only notify for failed workflows**, with email
   ticked.
3. Run **Watchdog** once from the Actions tab and check that it passes.

To check another address, set the repository variable `HEALTH_URL` (default
`https://api.upsignal.cloud/v1/system-health`).

What each check means is documented in the API repository, in
`docs/deployment.md` under "External watchdog".
