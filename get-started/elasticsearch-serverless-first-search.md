---
products:
  - id: elasticsearch
applies_to:
  serverless:
description: Create an Elasticsearch Serverless project, load sample web logs,
  explore the data in Discover, run your first ES|QL query, and save a
  visualization to a dashboard.
---

# Get started with {{es}} Serverless

Use this guide to go from an existing {{es}} Serverless project to your first useful search and dashboard using Elastic sample data.

Follow this guide to:

- Load sample web log data
- Explore the data in Discover
- Filter HTTP 404 responses with ES|QL
- Summarize 404 responses by operating system
- Save the visualization to a dashboard

## Before you begin

To follow this guide, you need:

- An {{ecloud}} account
- An {{es}} Serverless project that you can open in {{ecloud}}

This guide begins after project creation and focuses on reaching your first useful result with {{es}}.

## Load sample web logs

Start with sample data so you can explore {{es}} without preparing your own dataset.

1. Open your {{es}} Serverless project.
2. From **Home**, select **Get started with Elasticsearch**.
3. Under **Start with some sample data**, select **View sample data**.
4. On the **Add data** page, find **Sample web logs**.
5. Select **Install data**.

![Sample web logs card on the Add data page showing the Installed state and View data action.](/get-started/images/sample-web-logs.png)

When installation finishes, Elastic marks the dataset as **Installed** and changes the action to **View data**.

## Explore the data in Discover

After you load the sample web logs, use Discover to inspect and search the data.

In {{es}}, each log entry is stored as a document. A document contains fields such as a timestamp, URL, HTTP response code, client IP address, operating system, and tags.

1. On the **Sample web logs** card, select **View data**.
2. Select **Discover**.

Discover opens the sample dataset and displays:

- A histogram of events over time
- A table of individual log documents
- Available fields that you can use to search, filter, and analyze the data

![Discover showing the sample web logs, available fields, event histogram, and log documents.](/get-started/images/discover-sample-web-logs.png)

At this point, the sample data is loaded and ready to query.

## Filter 404 responses with ES|QL

Use ES|QL to narrow the sample data to HTTP requests that returned a 404 response.

Before you run the query, here is what each command does:

- `FROM` selects the dataset to search.
- `WHERE` keeps only documents that match a condition.
- `SORT` orders the results.
- `LIMIT` sets the maximum number of results to return.

In Discover, replace the current query with:

```esql
FROM kibana_sample_data_logs
| WHERE response.keyword == "404"
| SORT @timestamp DESC
| LIMIT 1000
```

Run the query.

The results now contain only requests with an HTTP 404 response. The number of matching documents can vary depending on the selected time range.

## Summarize 404 responses by operating system

After filtering the data to 404 responses, summarize the results to see which client operating systems generated the most requests.

`STATS` groups and aggregates the matching documents. In this example, it counts the 404 responses for each operating system.

Replace the current query with:

```esql
FROM kibana_sample_data_logs
| WHERE response.keyword == "404"
| STATS requests = COUNT(*) BY machine.os.keyword
| SORT requests DESC
| LIMIT 10
```

Run the query.

![HTTP 404 responses summarized by client operating system in Discover.](/get-started/images/404-responses-by-operating-system.png)

Discover displays the request count for each operating system as both a visualization and grouped results. Exact counts can vary with the selected time range.

## Save the visualization to a dashboard

Save the chart so you can return to the analysis without rebuilding the query.

1. In the upper-right corner of the visualization, select **Save visualization to dashboard**.
2. Enter a title for the visualization. For example:

   `404 Responses by Operating System`

3. Optionally, add a description that explains what the visualization shows.
4. Under **Add to dashboard**, select **New**.
5. Select **Save and go to dashboard**.
6. If Discover warns that the current session has unsaved changes, select **Leave without saving**. This keeps the saved visualization while discarding the temporary Discover session.
7. On the new dashboard, select **Save**.
8. Enter a dashboard title. For example:

   `Web Log 404 Analysis`

9. Optionally, add a description and enable **Store time with dashboard**.
10. Select **Save**.

![Web Log 404 Analysis dashboard showing HTTP 404 responses grouped by client operating system.](/get-started/images/web-log-404-analysis-dashboard.png)

The dashboard now contains a reusable visualization of HTTP 404 responses grouped by client operating system.

## What you accomplished

You have now completed a basic {{es}} workflow:

1. Loaded sample data into {{es}}.
2. Explored individual documents and fields in Discover.
3. Filtered the data with ES|QL.
4. Aggregated matching documents with `STATS`.
5. Created a visualization from the results.
6. Saved the visualization to a dashboard.

This workflow demonstrates a common {{es}} pattern: ingest data, search it, analyze it, and present the results.