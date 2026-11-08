# Elastic Stack (ELK) - TryHackMe SOC Level 1

- **Module:** Core SOC Solutions
- **Date completed:** 2026-10-28
- **Room link:** https://tryhackme.com/room/investigatingwithelk101
- **Path:** SOC Level 1

## Overview

This room shows how the Elastic Stack (ELK) is used for log analysis
and investigation in a SOC. ELK is not a traditional SIEM, but many
teams use it like one because of its strong search and visualisation
features. I explored its components, searched VPN logs in Kibana with
KQL, and built a visualisation and a dashboard.

## Objectives

- Understand the ELK components and how they are used in a SOC
- Explore the main Kibana features
- Search and filter data using KQL
- Investigate VPN logs to find anomalies
- Create visualisations and dashboards

## Tools / Concepts Covered

- ELK components: Elasticsearch, Logstash, Beats, Kibana
- Logstash configuration structure: input, filter, output
- Kibana Discover tab: index patterns, fields pane, time filter, timeline, tables
- KQL (Kibana Query Language): free text, field-based search, wildcards, AND / OR / NOT
- Visualisations, saved searches, and dashboards

## Analysis Process

### Step 1 - Understanding the Components

Elasticsearch is the search and analytics engine that stores JSON
documents. Logstash ingests data from different sources, normalises it
into field-value pairs through input, filter, and output stages, and
forwards it. Beats are lightweight host agents that ship specific
data (for example Windows event logs or network flows) to the stack.
Kibana is the web interface used to search, visualise, and build
dashboards. As an analyst, I mostly work in Kibana, but knowing the
pipeline helps me understand where data may be missing or mis-parsed.

### Step 2 - Working in the Discover Tab

Discover is where analysts spend most of their time. I used the index
pattern for the VPN logs, the fields pane (which shows top values and
lets me include or exclude a value with one click), the time filter,
and the event timeline, which makes spikes in activity easy to spot.
I also built a custom table of selected fields and saved it, which
removes noise and makes the logs easier to read. Setting the correct
absolute time range was essential, since the data covered a specific
period.

### Step 3 - Searching with KQL

KQL supports free-text searches and field-based searches in the form
`field : value`. Searches match whole terms, so a wildcard is needed
to match partial words. I combined conditions with AND, OR, and NOT to
narrow results, for example filtering by country and user, or checking
for activity after a given date, which is a realistic way to detect
access by terminated accounts.

### Step 4 - Visualisations and Dashboard

I created a visualisation showing users and source IP addresses for
failed VPN connection attempts by filtering on the connection action,
then saved it with a title and description. I added the saved
searches and visualisations to a custom dashboard, which gives a
single view of VPN activity and makes patterns such as repeated
failures easier to notice.

## Outcome / Classification

Completed the VPN log investigation, including targeted KQL searches,
a saved table, a failed-connection visualisation, and a custom
dashboard.

## Key Takeaways

Compared with Splunk, the workflow felt similar (ingest, normalise,
search, visualise), but KQL and the Discover interface make
point-and-click filtering quick. The biggest lesson was how much
depends on the basics: the right index pattern and the right time
range. A wrong time range silently returns nothing, which could be
mistaken for "no suspicious activity". Visualisations and dashboards
turn raw logs into something a team can monitor continuously.

## Skills Demonstrated

- Explaining the ELK components and the data pipeline
- Searching and filtering logs with KQL (field searches, wildcards, boolean operators)
- Investigating VPN logs for anomalies, such as unusual sources and activity after account termination
- Building saved tables, visualisations, and dashboards in Kibana
