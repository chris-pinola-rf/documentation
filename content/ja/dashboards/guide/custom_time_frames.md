---
title: カスタムタイムフレーム
---

## Overview

<div class="alert alert-info">Queries are run in UTC time, but the query time frame is selected according to your <strong>browser's time zone</strong>. Toggle between displaying the default time zone or UTC from the dashboard configure action. For more information, see the <a href="/dashboards/configure/#configuration-actions">Dashboard configuration documentation</a>.</div>

Many views in Datadog can be scoped to a specific time frame. Time controls include a list of common time frames and a calendar picker for quick selection.

To increment by month, day, year, hour, or minute, highlight a portion of the time frame and use the `[↑]` and `[↓]` keys:

{{< img src="dashboards/guide/custom_time_frames/increment_with_arrow_keys.mp4" alt="Increment time with arrow keys" video="true" width="500" >}}

## Supported syntaxes

### Fixed dates

{{< img src="dashboards/guide/custom_time_frames/custom_fixed_time_frame.mp4" alt="Type custom fixed time frame" video="true" width="500" >}}

| Format                       | Examples                                         |
|------------------------------|--------------------------------------------------|
| `{MMM/MMMM} D`               | Jan 1<br>January 1                               |
| `M/D`                        | 1&#8203;/&#8203;1                                |
| `M-D`                        | 1-1                                              |
| `M/D/{YY/YYYY}`              | 1/1/19<br>1/1/2019                               |
| `M-D-{YY/YYYY}`              | 1-1-19<br>1-1-2019                               |
| `{MMM/MMMM} D, h:mm a`       | Jan 1, 1:00 pm<br>January 1, 1:00 pm             |
| `{MMM/MMMM} D, YYYY, h:mm a` | Jan 1, 2019, 1:00 pm<br>January 1, 2019, 1:00 pm |
| `h:mm a`                     | 1:00 pm                                          |
| Unix seconds timestamp       | 1577883600                                       |
| Unix milliseconds timestamp  | 1577883600000                                    |

固定日付はいずれも、範囲の一部として入力できます。例:
  * `1577883600 - 1578009540`
  * `Jan 1 - Jan 2`
  * `6:00 am - 1:00 pm`

### Relative dates

相対的な日付は入力時に計算され、時間の経過とともに更新されることは**ありません**。

{{< img src="dashboards/guide/custom_time_frames/custom_relative_time_frame.mp4" alt="Type custom relative time frame" video="true" width="500" >}}

| Format                                             | Description                                                         |
|----------------------------------------------------|---------------------------------------------------------------------|
| `N{unit}`<br> 以下の使用可能な単位のリストを参照してください。 | 過去 N 個の単位を表示します。例えば、**3 mo** (過去 3 か月) です。|
| `today`                                            | Displays the current calendar day until present                     |
| `yesterday`                                        | Displays the full previous calendar day                             |
| `this month`                                       | Displays the current calendar month until present                   |
| `last month`                                       | Displays the full previous calendar month                           |
| `this year`                                        | Displays the current calendar year until present                    |
| `last year`                                        | Displays the full previous calendar year                            |

The following strings are accepted for any `{unit}` in a relative date:
  * Minutes: `m`, `min`, `mins`, `minute`, `minutes`
  * Hours: `h`, `hr`, `hrs`, `hour`, `hours`
  * Days: `d`, `day`, `days`
  * Weeks: `w`, `week`, `weeks`
  * Months: `mo`, `mos`, `mon`, `mons`, `month`, `months`

### カレンダーに沿った日付

カレンダーに沿った日付は現在の日付を反映して更新されます。

| Format         | Description                                      |
|----------------|--------------------------------------------------|
| `week to date` | 月曜日午前 12 時から現在までの 1 週間を表示します。 |
| `month to date`| 月の初日から現在までを表示します。      |

## URLs

You can manipulate time queries in the URL of a dashboard.

Consider the following dashboard URL:

```
https://app.datadoghq.com/dash/host/<DASHBOARD_ID>?from_ts=<QUERY_START>&to_ts=<QUERY_END>&live=true
```

* The `from_ts` parameter is the Unix milliseconds timestamp of the query starting time. For example, `1683518770980`.
* The `to_ts` parameter is the Unix milliseconds timestamp of the query ending time. For example, `1683605233205`.
* `live=true` indicates that relative time specifications are retained when a query is saved or shared. You can also use `live=false`.