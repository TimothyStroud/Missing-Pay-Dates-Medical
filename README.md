# Missing Pay Dates

Replicas of the `frmMain` / `rptClientReport` view from
`M:\DIG\MissingPayDatesNpt3.accdb` — Q4 2025-forward payment-dates calendar
showing per-client, per-day cert totals with `Weekend`, `Not Received`,
`Holiday`, and `Negative Paid` flags.

Two delivery surfaces, fed from the same SQL Server source
(`trgintp3.DataOperations.dbo.vw_PayAmount`):

| File | Purpose |
| --- | --- |
| `missing_pay_dates_html.py` | **Active.** Generates a self-contained
`MissingPayDates.html` dashboard (vanilla JS filters by DatabaseName / Year /
Month / Status / date range / low-volume threshold). Drops the file on the
shared drive + OneDrive. Linked from the Data Operations Notion page. |
| `missing_pay_dates_sync.py` | Sync to a Notion database. Currently
deprioritized — needs a workspace integration token to run unattended.
Kept in source control in case the path is revived later. |

## Generate the HTML report

```
python missing_pay_dates_html.py
```

Writes:

- `\\trgfile1\Shared\DIG\Data Business Delivery Team\Delivery Schedule\Daily Status Reports\MissingPayDates.html`
- `C:\Users\tls2\.claude\projects\H--\MissingPayDates.html`
- `C:\Users\tls2\OneDrive - Machinify\Documents\Reports\MissingPayDates.html`

## Schedule

Windows Scheduled Task `MissingPayDates HTML Report` — Mon-Fri 08:30 local.

## Data window

Configured by `WINDOW_START` in `missing_pay_dates_sync.py` (currently
`2025-10-01`).

## Federal holidays

Comments are augmented with `Holiday: <name>` when a date is a US federal
holiday (observed-rules applied). See `us_federal_holidays()` in the sync
module.

## Notion token (sync mode)

If you ever want to run `missing_pay_dates_sync.py` against Notion directly,
drop the integration secret at
`C:\Users\tls2\.claude\secrets\notion_token.txt` or export the
`NOTION_INTEGRATION_TOKEN` environment variable. Never commit the secret.
