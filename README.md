# Project 2 — Azure NSG Flow Logs + Log Analytics (Network Observability)

## Overview

This project demonstrates how to enable and validate **Azure NSG Flow Logs (v2)** for a non-production workload, store logs in a **Storage Account**, and run **traffic analysis using Log Analytics (KQL)**. The goal is to prove end-to-end network observability: configuration → traffic generation → log ingestion → query evidence.

**Use case:** Security operations and cloud platform teams need visibility into allowed/denied flows for troubleshooting, auditing, and governance.

## Objectives

* Enable **NSG Flow Logs v2** for a target NSG
* Store flow logs in **Storage Account (Blob)**
* (Optional but recommended) Connect to **Log Analytics** for KQL analysis / Traffic Analytics
* Generate real traffic and prove logs are being produced
* Provide evidence screenshots and repeatable steps

## Architecture Summary

* Workload VM / NIC produces traffic (RDP/SSH, DNS, HTTP/HTTPS, etc.)
* NSG enforces rules and emits Flow Logs
* Storage Account stores raw flow log JSON blobs
* Log Analytics queries validate traffic patterns and troubleshooting scenarios

## Components

* Network Security Group (NSG): `<name>`
* Storage Account: `<name>`
* Log Analytics Workspace (LAW): `<name>`
* Workload VM (or test source): `<name>`
* (Optional) Network Watcher / Traffic Analytics: `<enabled/region>`

## Deployment Steps (High-Level)

1. Create/confirm **Storage Account** for flow logs.
2. In the NSG → **NSG flow logs** → **Create**:

   * Set **Flow log type: NSG**
   * Select the NSG
   * Choose Storage Account
   * Set retention (optional)
   * (Optional) Enable Traffic Analytics + select Log Analytics workspace
3. Generate traffic:

   * From VM, run outbound requests (HTTP/HTTPS) and DNS lookups
   * Confirm inbound test (only if rules allow)
4. Validate logs:

   * Storage: confirm blobs are created under the flow log container path
   * Log Analytics: run KQL queries and capture results

## Validation Evidence

This repository includes screenshots proving:

* ✅ Flow log created successfully (NSG selected, not VNet)
* ✅ Storage container contains new flow log blobs
* ✅ Time window aligns (wait period + traffic generation)
* ✅ KQL query returns matching flows
* ✅ Filters cleared / correct subscription & resource group

## Key Findings / Troubleshooting Notes

* If KQL shows **no results**, usually one of these is true:

  * No traffic occurred during the window
  * Flow logs haven’t had time to write (allow 10–20 minutes)
  * Flow logs are stored in Storage but not configured for Log Analytics/Traffic Analytics
  * Wrong workspace/time range in query
  * Filters on the blade hide the created flow log (clear filters)

## Cost Controls

* Use a small test VM and deallocate when done
* Storage lifecycle/retention to control costs
* Avoid always-on analytics unless needed

## Repo Structure

* `/docs` — walkthrough steps & notes
* `/screenshots` — evidence images (named consistently)
* `/kql` — saved queries used for validation

## Author

Luis Antonio Santiago-Ramirez
Cloud Engineering / Azure Governance & Automation
