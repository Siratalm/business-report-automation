# 📊 Daily · Weekly · Monthly Business Report Automation

Automatically sends business performance reports to the owner
via Telegram every night. Covers sales, profit, stock levels,
and debt alerts across 3 shops.

Built with n8n + Google Sheets + Telegram Bot API.

---

## 🔁 What It Does

- **Daily** — revenue, profit, top product per shop, stock alerts, debt summary
- **Weekly** — shop rankings, best day, profit margin, week's debt collected
- **Monthly** — vs target, top 3 products, shop rankings, debt recovery

Debt alerts fire separately — only when there is activity that day.

---

## 📋 Google Sheets Structure

One spreadsheet with 5 tabs:

**shop_A / shop_B / shop_C**
| date | product_name | qty_sold | unit_price | unit_cost | total_sale | total_profit | shop_id |

**stock**
| product_name | current_stock | low_stock_alert | unit_cost | last_updated |

**Debt**
| debtor_name | shop_id | amount_owed | amount_paid | due_date | status | paid_date | notes |

---

## 🔁 Workflow Flow

Schedule Trigger (11 PM nightly)
  → Identifi Schedule (detect daily/weekly/monthly)
  → Read Shop A + B + C + Stock + Debt (parallel)
  → Combine All Data (Merge — 5 inputs)
  → Calculate Report (build messages)
  → Send Report to Owner (Telegram)
  → If Debt Exist → Send Debt Alert (Telegram)

---

## ⚙️ Setup

1. Import the JSON into n8n
2. Create a Google Spreadsheet with the 5 tabs above
3. Create a Telegram bot via @BotFather — save the token
4. Get your Telegram Chat ID via @userinfobot
5. Add credentials in n8n:
   - Google Sheets OAuth2
   - Telegram Bot API
6. Update the Spreadsheet ID in all Google Sheets nodes
7. Update Chat ID in both Telegram nodes
8. Set BUDGET value in Calculate Report code (line: const BUDGET = 1200000)
9. Publish the workflow

---

## 🗓️ Report Type Logic

| Condition | Report |
|---|---|
| 1st of the month | Monthly |
| Saturday | Weekly |
| All other days | Daily |

To change the weekly day — edit `=== 6` in Identifi Schedule
(0=Sun 1=Mon 2=Tue 3=Wed 4=Thu 5=Fri 6=Sat)

---

## 👤 Author

MD MUSTAKIM ALI (siratal)
Built with n8n · Google Sheets · Telegram
