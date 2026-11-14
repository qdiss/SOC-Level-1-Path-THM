# Wireshark: Packet Operations - TryHackMe SOC Level 1

- **Module:** Network Traffic Analysis
- **Date completed:** 2026-11-14
- **Room link:** https://tryhackme.com/room/wiresharkpacketoperations  
- **Path:** SOC Level 1

## Overview

This is the second room of the Wireshark series. It covers the
fundamentals of packet analysis: using Wireshark's statistics views to
understand a capture file as a whole, then filtering packets with
capture and display filters, protocol filters, and advanced operators
to investigate an event of interest at packet level. I worked through
a provided capture file in a lab VM.

## Objectives

- Investigate network traffic captures
- View statistics, including summaries and protocol details
- Apply packet filtering principles
- Apply protocol filters
- Apply advanced filtering

## Tools / Concepts Covered

- Wireshark statistics: Resolved Addresses, Protocol Hierarchy, Conversations, Endpoints, IPv4/IPv6 statistics, DNS and HTTP statistics
- Name resolution (MAC vendor, IP, port) and GeoIP mapping
- Capture filters vs. display filters
- Comparison and logical operators
- Protocol filters for IP, TCP, UDP, HTTP, and DNS
- Advanced operators and functions: contains, matches, in, upper, lower, string
- Filter bookmarks, filter buttons, and configuration profiles

## Analysis Process

### Step 1 - Getting the Big Picture with Statistics

Before searching individual packets, I used the Statistics menu to
understand the capture. Resolved Addresses maps IP addresses to the
hostnames seen in DNS answers, Protocol Hierarchy shows which
protocols make up the traffic and in what proportion, and
Conversations and Endpoints show who talked to whom and how much data
moved. Name resolution can translate MAC addresses to vendor names, and
GeoIP data (if configured with a database) adds location context. The
IP, DNS, and HTTP statistics views summarise protocol-specific details
such as request and response codes, query types, and response times.
These views help form a hypothesis before going packet by packet.

### Step 2 - Filtering Principles

Wireshark has two types of filters. Capture filters decide what is
saved before capturing starts and cannot be changed during the capture,
so they are best left to experienced users. Display filters narrow what
is shown and can be changed at any time, which is why the typical
workflow is to capture everything and filter later. I learned the
display filter syntax, comparison operators (equal, not equal, greater
than, less than), logical operators (and, or, not), and the colour
feedback in the filter bar that shows whether a filter is valid,
invalid, or unreliable.

### Step 3 - Protocol Filters

I applied filters at different layers: IP filters for addresses,
subnets, and TTL values; TCP and UDP filters for ports; and
application filters for HTTP (methods and response codes) and DNS
(requests, responses, and record types). An important detail is that
filtering by address without direction matches both source and
destination, while source and destination filters respect packet
direction. The Display Filter Expression builder helps when I cannot
remember a protocol's field names.

### Step 4 - Advanced Filtering

Advanced operators allow more precise searches: `contains` for
case-sensitive text in a field, `matches` for regular expressions,
`in` for matching a set of values, and the `upper`, `lower`, and
`string` functions to normalise or convert values before comparing.
I also saved useful filters as bookmarks and buttons for one-click
reuse, and used profiles to keep different investigations'
configurations (colouring rules, buttons, preferences) separate,
including a profile for checking checksum errors.

## Outcome / Classification

Completed the questions on statistics, protocol filters, and advanced
filtering using the provided capture file.

## Key Takeaways

Statistics first, filters second is a good workflow: the overview
shows what is unusual, and precise display filters let me confirm it
at packet level. Learning the filter syntax pays off quickly because
the same few patterns (address, port, protocol field, operator) cover
most investigations. Bookmarks, buttons, and profiles make repeated
investigations faster and more consistent. These skills connect to the
earlier rooms where I saw that logs show only fragments, while packet
analysis shows the actual content.

## Skills Demonstrated

- Using Wireshark statistics to summarise a capture and form a hypothesis
- Writing display filters with comparison and logical operators
- Filtering by IP, TCP, UDP, HTTP, and DNS fields
- Using contains, matches, in, and string functions for advanced searches
- Organising investigations with bookmarks, filter buttons, and profiles
