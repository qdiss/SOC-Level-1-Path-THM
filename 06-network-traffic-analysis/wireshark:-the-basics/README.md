# Wireshark: The Basics - TryHackMe SOC Level 1

- **Module:** Network Traffic Analysis
- **Date completed:** 2026-11-14
- **Room link:** https://tryhackme.com/room/wiresharkthebasics
- **Path:** SOC Level 1

## Overview

This is the first room of the Wireshark series. Wireshark is an
open-source packet analyser that can sniff live traffic and inspect
saved captures (PCAP files). The room covers the interface, how a
packet is dissected into OSI layers, useful analysis features such as
searching, marking, and exporting packets, and basic display
filtering. I practised on provided capture files in a lab VM.

## Objectives

- Navigate and configure Wireshark
- Inspect packets and read information from the different layers of the TCP/IP model
- Apply display filters

## Tools / Concepts Covered

- Wireshark interface: toolbar, display filter bar, packet list, packet details, and packet bytes panes
- Loading, merging, and inspecting capture files, including capture file properties
- Packet colouring rules (temporary and permanent)
- Packet dissection by layer (frame, MAC, IP, TCP/UDP, application protocol and data)
- Finding, marking, commenting on, and exporting packets
- Exporting transferred objects (files) from supported protocols
- Time display formats and Expert Information
- Filtering: Apply as Filter, Conversation Filter, Prepare as Filter, Apply as Column, Follow Stream
- Simple display filter queries for protocols, ports, and IP addresses

## Analysis Process

### Step 1 - Learning the Interface

Wireshark's main window has a toolbar, a display filter bar, a recent
files list, capture filter and interface selection, and a status bar.
Once a capture is loaded, three panes show the data: the packet list
for a summary of every packet, the packet details pane for the
protocol breakdown, and the packet bytes pane for the hex and ASCII
view. Colouring rules highlight protocols and conditions so unusual
traffic stands out. I also practised starting and stopping live
captures, merging two capture files, and viewing capture file
properties such as the file hash and comments, which help identify and
prioritise files when working with several captures.

### Step 2 - Packet Dissection

Wireshark decodes each packet layer by layer. For an HTTP packet I
examined the frame details, the source and destination MAC addresses,
the IP header (including TTL), the TCP header and payload size, and
the application layer details such as the content type and entity
tags. Clicking any field highlights the matching bytes, which helps
connect the decoded fields to the raw data.

### Step 3 - Analysis Features

I learned to navigate by packet number, search packets by string,
regex, hex, or display filter in the correct pane, mark packets for
attention, and add comments that stay in the capture file for other
analysts. Packets can be exported to share only the relevant subset,
and files transferred over supported protocols (such as HTTP) can be
extracted for further investigation. Changing the time display format
to UTC makes timelines easier to interpret. Expert Information
summarises possible anomalies such as warnings and malformed packets,
but it only provides suggestions that need analyst judgement.

### Step 4 - Filtering Traffic

The golden rule is that anything clickable can be filtered. I used
Apply as Filter for a single value, Conversation Filter to see all
packets of one conversation, Colourise Conversation to highlight
without hiding packets, Prepare as Filter to build more complex
queries, and Apply as Column to compare a field across packets.
Follow Stream reconstructs application-level data, which can reveal
usernames, passwords, and other transferred content in unencrypted
protocols. I also wrote simple display filters by protocol name, port
number, and IP address.

## Outcome / Classification

Completed the questions on navigation, packet details, search, packet
comments, file extraction, Expert Information, and basic filtering
using the provided capture file.

## Key Takeaways

Wireshark is not an IDS: it does not detect or modify anything, so
finding problems depends on my knowledge and method. The most useful
habits are starting with the packet details and layers, using filters
to remove noise, and following streams to see what the application
actually exchanged. Being able to extract files and comment on packets
also makes it easier to document findings and hand them over to
another analyst. These basics prepare me for the next room, where I
use statistics and more advanced filters.

## Skills Demonstrated

- Navigating Wireshark and reading packet details across OSI layers
- Searching, marking, commenting on, and exporting packets
- Extracting files transferred over network protocols
- Using filters, conversation filters, and stream following to isolate events
- Interpreting Expert Information as hints rather than verdicts
