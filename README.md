# UpSignal watchdog

Checks `https://api.upsignal.cloud/v1/system-health` every 5 minutes. If it
fails, GitHub emails you.

## Set up

1. Push this repository to GitHub as a **public** repository.
2. Turn on email for failed workflows: Settings → Notifications → Actions.
3. Run **Watchdog** once from the Actions tab.

To check another address, set the repository variable `HEALTH_URL`.
