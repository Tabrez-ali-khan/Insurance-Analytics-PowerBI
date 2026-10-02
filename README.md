<div align="center">

<img width="1280" height="400" alt="Image" src="https://github.com/user-attachments/assets/b4dfbf6c-062a-4b4d-b5fd-03cae8540195" />

<br>

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-8E44AD?style=for-the-badge)
![Power Query](https://img.shields.io/badge/Power_Query-217346?style=for-the-badge)
![CSV](https://img.shields.io/badge/CSV-555555?style=for-the-badge)
![Synthetic data](https://img.shields.io/badge/Data-Synthetic-F2C811?style=for-the-badge)

**[See the dashboard](#the-dashboard)** &nbsp;|&nbsp; **[Key findings](#what-the-data-says)** &nbsp;|&nbsp; **[What I'd do next](#what-i-would-do-next)** &nbsp;|&nbsp; **[Run it yourself](#run-it-yourself)**

</div>

<br>

> [!NOTE]
> This project uses a **randomly generated (synthetic) dataset** built for practice. It does not represent a real insurer or real customers, and the data has no currency field, so amounts are shown as plain numbers.

## The short version

A Power BI report that answers one question for an insurance portfolio: **where does the premium come from, and where do the claims go?** It covers five policy types, claims by status and age group, and active versus inactive policies, with a drill-through page for policy-level detail.

<table align="center">
  <tr>
    <td align="center" width="33%">
      <h2>41%</h2>
      <sub>of premium comes from<br><b>Travel</b>, the largest line</sub>
    </td>
    <td align="center" width="33%">
      <h2>43.5%</h2>
      <sub>of claim records are<br><b>rejected</b> (4,355 of 10,004)</sub>
    </td>
    <td align="center" width="33%">
      <h2>6.81M</h2>
      <sub>of claim value is still<br><b>pending</b> (2,263 claims)</sub>
    </td>
  </tr>
</table>

---

## The dashboard

<div align="center">
  <img width="1527" height="925" alt="Image" src="https://github.com/user-attachments/assets/37792684-4816-4f2e-b79f-246aed65f173" />
  <br>
  <sub><b>Overview page.</b> KPI cards, claims by status, premium by policy type, claims by age group, active vs inactive policies, and a status matrix.</sub>
</div>

<br>

<div align="center">
  <img width="1641" height="895" alt="Image" src="https://github.com/user-attachments/assets/8eec47a2-1c99-426f-b91e-ee21f216d84d" />
  <br>
  <sub><b>Drill-through page.</b> Right-click a policy type on the overview to open customer, policy and claim detail. The back button returns to the overview.</sub>
</div>

---

## What the data says

**Claims by status**

<img width="900" height="270" alt="Image" src="https://github.com/user-attachments/assets/79292332-f963-4ee4-868f-6a5650cd37ba" />

| Finding | Detail |
| --- | --- |
| Travel carries the portfolio | 4,148 policies, about 2.5M of the 5.98M premium and about 7.1M of the 16.91M claims (roughly 41% and 42%). |
| Settled value is only part of the total | Of the 16.91M claim amount, 10.11M is settled and 6.81M is still pending (about 60% and 40%). |
| Adults generate the most claim value | Adults (25 to 59) account for 8.77M, Elders (60+) 6.39M and Young Adults (24 and under) 1.75M. |
| Most policies are active | 5,815 active (58.1%) and 4,189 inactive (41.9%). |

Premium by policy type: **Travel 2.5M · Health 1.2M · Auto 1.0M · Life 0.7M · Home 0.6M.**

---

## What I would do next

1. **Review Travel pricing and claims first**, since it drives the largest share of both premium and claim value.
2. **Add a rejection reason to the data.** A 43.5% rejection rate means little without knowing if rejections come from policy terms, missing documents or fraud checks.
3. **Work the 2,263 pending claims** and track how long they stay pending.
4. **Target renewals at the 42% inactive policies**, starting with the policy types and age groups that lapse most.
5. **Look closely at Adult and Elder customers**, who generate about 90% of claim value.

---

## Under the hood

<details>
<summary><b>Business questions this report answers</b></summary>
<br>

1. How much premium, coverage and claim value does the portfolio hold, and which policy types drive it?
2. How are claims split between settled, pending and rejected?
3. Which age groups account for the most claim value?
4. How many policies are active and how many are inactive?
5. What does the policy-level detail look like behind each policy type?

</details>

<details>
<summary><b>Dataset and derived columns</b></summary>
<br>

The main file is [`data/InsuranceData.csv`]([InsuranceData.csv](https://github.com/user-attachments/files/32984280/InsuranceData.csv). One row is one policy with its claim.

| Column | Description |
| --- | --- |
| PolicyNumber, CustomerID, ClaimNumber | Identifiers |
| Gender, Age | Customer attributes (5,001 female, 5,003 male, aged 18 to 87) |
| PolicyType | Auto, Health, Home, Life or Travel |
| PolicyStartDate, PolicyEndDate | Policy period, one year per policy (starts 14 Jul 2023 to 12 Jul 2024) |
| PremiumAmount, CoverageAmount | Premium paid and sum covered |
| ClaimDate, ClaimAmount, ClaimStatus | Claim details (Settled, Pending or Rejected) |

**Derived columns used in the report**

| Column | Rule |
| --- | --- |
| Age Group | Young Adult: 24 and under · Adult: 25 to 59 · Elder: 60 and over |
| Active / Inactive | Active if the policy end date is on or after 11 Dec 2024, otherwise Inactive |

</details>

<details>
<summary><b>Data notes</b></summary>
<br>

- 10,004 rows hold 10,000 unique policies. 4 rows are exact duplicates (policies P1, P2 and P4) and are included in the report totals. The effect is under 0.1%.
- All 4,355 rejected claims have a claim amount of 0 and no claim date.
- [`data/Insurance_Customer_Feedback.xlsx`](data/Insurance_Customer_Feedback.xlsx) holds 97 customer comments. It is supplementary and is not connected to the report, because the comments carry customer names but no customer IDs.

</details>

<details>
<summary><b>Limitations</b></summary>
<br>

- The data is synthetic, so the findings illustrate the method and are not real business results.
- Claims total about 2.8 times premiums (settled claims about 1.7 times), which is unrealistic and typical of generated data, so no loss-ratio conclusions should be drawn.
- Active / Inactive uses a fixed reference date (11 Dec 2024), not today's date.
- The report has no time-trend page, and the feedback comments are not linked to customers.

</details>

<details>
<summary><b>Repository structure</b></summary>
<br>

```
Insurance-Analytics-PowerBI/
├── README.md
├── assets/
│   └── banner.svg
├── data/
│   ├── InsuranceData.csv
│   └── Insurance_Customer_Feedback.xlsx
├── powerbi/
│   └── Insurance_report.pbix
└── screenshots/
    ├── 01_Insurance_dashboard_overview.png
    └── 02_Drill_through_by_policy_type.png
```

</details>

---

## Run it yourself

1. Download `Insurance_report.pbix` from the `powerbi/` folder and open it in **Power BI Desktop**.
2. Use the slicers on the overview page to filter by claim, policy or customer.
3. Right-click a policy type and choose **Drill through** to open the detail page.
4. If Power BI asks for the data source path, point it to `data/InsuranceData.csv`.

---

<div align="center">

**Mohammed Tabrez Ali Khan** · Data Analyst
Riyadh, Saudi Arabia · [LinkedIn](https://www.linkedin.com/in/md-tabrez-ali-khan) · mdtabrezalik@gmail.com

</div>
