# Splunk: Basics - TryHackMe SOC Level 1

- **Module:** Core SOC Solutions
- **Date completed:** 2026-10-28
- **Room link:** https://tryhackme.com/room/splunk101/
- **Path:** SOC Level 1

## Overview

This room introduces Splunk, one of the leading SIEM platforms. It
covers Splunk's architecture, the web interface, and how data is
ingested, then moves to a hands-on exercise where I uploaded VPN log
data into my own index and searched it.

## Objectives

- Understand the main components of Splunk
- Explore the options available in the Splunk interface
- Understand how log ingestion works
- Ingest logs into Splunk and analyse them

## Tools / Concepts Covered

- Splunk architecture: Forwarder, Indexer, Search Head
- Splunk home screen: Splunk Bar, Apps panel, Explore Splunk, dashboards
- Search & Reporting app and SPL (Search Processing Language)
- Data ingestion workflow: source, source type, input settings, index, review
- JSON log parsing and field extraction
- Basic SPL searches and aggregations on an index

## Analysis Process

### Step 1 - Understanding the Architecture

Splunk has three main components. The **Forwarder** is a lightweight
agent on the monitored endpoint that collects data (web server logs,
Windows event logs and Sysmon, Linux host logs, database logs) and
sends it on. The **Indexer** parses and normalises incoming data into
field-value pairs and stores it as searchable events. The **Search
Head** is where analysts query the indexed data using SPL and turn
results into tables and visualisations.

### Step 2 - Navigating the Interface

I explored the home screen: the Splunk Bar (messages, settings,
activity, help, search), the Apps panel with Search & Reporting as the
default app, the Explore Splunk panel with quick links to add data and
apps, and the home dashboard area for dashboards. I also located the
Add Data options for different ingestion methods.

### Step 3 - Ingesting VPN Logs

I uploaded a VPN log file through the Add Data wizard. The workflow
was: select the source file, choose the source type (JSON), set the
input settings including a dedicated index for the data, review the
configuration, and complete the upload. Creating a separate index
keeps the data organised and easy to query.

### Step 4 - Searching the Data

In the Search & Reporting app I set the time range to cover all the
uploaded events and searched the new index. I used SPL to count
events, filter by user, source IP, and country, and aggregate results.
I also noted that JSON fields may need explicit parsing in a search
before they can be filtered on, which was a good reminder to verify
that fields are extracted correctly before trusting a result.

## Outcome / Classification

Successfully ingested the log file into a dedicated index and answered
the investigation questions about users, IP addresses, and countries
using SPL searches.

## Key Takeaways

Knowing the architecture makes troubleshooting easier: if data is
missing, I can ask whether it was never forwarded, not indexed
correctly, or simply not found by my search (for example, the wrong
time range). The ingestion exercise also showed that search results
are only as good as the parsing behind them. Splunk's field-value
model felt familiar from working with Linux logs and structured data,
now used from a defender's perspective.

## Skills Demonstrated

- Explaining Splunk components and how data flows between them
- Ingesting log files into a dedicated Splunk index
- Writing basic SPL searches with filters and aggregations
- Verifying data parsing and time range before drawing conclusions
