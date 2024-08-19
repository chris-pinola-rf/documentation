---
description: スパンをフィルターします。現在関心のあるスパンのサブセットに焦点を絞る、拡大する、またはシフトします。
further_reading:
- link: tracing/trace_explorer/query_syntax
  tag: ドキュメント
  text: 検索構文
title: スパンを検索する
---

## Overview

While information from individual spans can be useful visualized as a list, sometimes valuable information can be accessed through aggregation. To access this information, search spans in the Trace Explorer and display them as timeseries, top lists, or tables.

Trace Explorer search consists of a time range and a search query that combines `key:value` and full-text search. Choose which dimension to visualize (count of spans, count of unique values, measure of a quantitative dimension), select a time frame, and group the query by one or multiple dimensions.

## Search query

For example, to find spans from a web store service, with an error status, over the past thirty minutes, create a custom query like `service:web-store status:error`, and set the time range to the `Past 30 minutes`:

{{< img src="tracing/trace_explorer/search/trace_explorer_list_search.png" alt="Trace Explorer list search, where user has searched for 'service:web-store' and 'status:error'. A requests bar chart, errors bar chart, and latency line chart are shown. The Visualize As option is set to List." style="width:100%;">}}

Select a top list view, and group the query by `resource` to see which specific resources are most affected.

{{< img src="tracing/trace_explorer/search/trace_explorer_top_list_search.png" alt="Trace Explorer list search. The Visualize As option is set to Top List." style="width:100%;">}}

{{< site-region region="us,eu,us3,us5,ap1" >}}
**Note**: `key:value` queries **do not** require that you [declare a facet][1] beforehand.

[1]: /ja/tracing/trace_explorer/query_syntax/#facets
{{< /site-region >}}

## Query Syntax

To begin searching for spans in the Trace Explorer, read the [query syntax documentation][2] and the [time frame documentation][3] for more details on custom time frames.


## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/tracing/trace_explorer/query_syntax/#facets
[2]: /ja/tracing/trace_explorer/query_syntax
[3]: /ja/dashboards/guide/custom_time_frames