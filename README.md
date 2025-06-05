# GrafanaReportToSlackGitHubAction

This GitHub Action workflow automates the process of rendering a Grafana dashboard panel, saving the resulting image to your repository, and sending it to a Slack channel on a schedule (defaulting to 6 hours for demonstration purposes) or via manual trigger.

## Features

- **Scheduled Reporting:** Runs every 6 hours (customizable via cron) or on manual dispatch.
- **Grafana Integration:** Renders a specified Grafana dashboard panel as an image.
- **Repository Storage:** Commits and pushes the generated panel image (`panel.png`) to the `github-actions-example` folder in your repository.
- **Slack Notification:** Sends a message to a Slack channel with the image attached and/or linked, using a Slack webhook.

## How It Works

1. **Checkout Repository:** The workflow checks out the latest code.
2. **Prepare Folder:** Ensures the `github-actions-example` folder exists and is empty. We want the panel.png file to be regenerated and committed into this repository every time the job runs. 
3. **Render Grafana Panel:** Uses the [Grafana Image Rendering API](https://grafana.com/docs/grafana/latest/developers/http_api/image_rendering/) to generate a PNG of the specified dashboard panel. This can easily be changed to generate a PNG of a dashboard if you remove the query string `panelId=${{ vars.GRAFANA_PANEL_ID }}&`.
4. **Commit Image:** Commits and pushes the new `panel.png` to the repository.
5. **Generate Permalink:** Constructs a permalink to the committed image.
6. **Send to Slack:** Uses [rtCamp/action-slack-notify](https://github.com/rtCamp/action-slack-notify) to send a message (with image) to Slack via webhook.

## Usage

### Required Secrets & Variables

- `GH_PAT`: GitHub Personal Access Token with repo write permissions.
- `GRAFANA_SERVICEACCOUNT_TOKEN`: Grafana API token with dashboard rendering permissions.
- `SLACK_WEBHOOK_URL`: Slack Incoming Webhook URL.
- `SLACK_BOT_TOKEN`: Slack Token with Scopes: incoming-webhook, files:read, files:write, chat:write
- `GRAFANA_INSTANCE`, `GRAFANA_DASHBOARD_ID`, `GRAFANA_PANEL_ID`: Set as repository variables or workflow `vars`.
- `SLACK_CHANNEL`: Slack channel name or ID.

### Example Workflow

See [`.github/workflows/grafana-report-to-slack.yml`](.github/workflows/grafana-report-to-slack.yml) for the full workflow.

### Slack Message Example

The workflow sends a message to Slack with the panel image attached and/or linked using a permalink.

## References

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Grafana HTTP API: Image Rendering](https://grafana.com/docs/grafana/latest/developers/http_api/image_rendering/)
- [Slack API: Incoming Webhooks](https://api.slack.com/messaging/webhooks)
- [Slack API: files.upload (deprecated for some apps)](https://api.slack.com/methods/files.upload)
- [Slack API: External File Upload (recommended)](https://api.slack.com/methods/files.getUploadURLExternal)
- [rtCamp/action-slack-notify GitHub Action](https://github.com/rtCamp/action-slack-notify)

## Notes

- For **private repositories**, Slack cannot display images from GitHub permalinks unless the image is uploaded directly to Slack.
- For **public repositories**, the image permalink will work in Slack messages.
- Adjust the workflow as needed for your organization's security and compliance requirements.

---
