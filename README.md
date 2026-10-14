# Splunk SIEM Investigation Lab: Authentication Anomaly Triage

**Portfolio project | SOC Analyst L1 | Splunk Search & Reporting**

## Objective
Practice a repeatable SOC workflow for identifying unusual authentication activity, validating the evidence, separating suspicious activity from false positives, and documenting an escalation decision.

> **Important:** This starter kit does not claim that an attack occurred. Run the searches against data your Splunk account is authorized to access, record what the data actually shows, and label any simulated evidence clearly.

## Lab constraints
This project is designed to start with Splunk **Search & Reporting** and does not assume administrator rights, Splunk Enterprise Security, or permission to configure forwarders/indexes.

## Repository contents
- `queries/00_inventory.spl` — discover indexes available to your account.
- `queries/01_source_inventory.spl` — inspect event volume by host and sourcetype.
- `queries/02_inspect_events.spl` — inspect recent events in one accessible index.
- `queries/03_windows_failed_logons.spl` — starter search for Windows failed logons (field names may vary).
- `queries/04_authentication_summary.spl` — summarize candidate events by source/user/host.
- `reports/incident-report-template.md` — evidence-led incident report template.
- `reports/lab-notes-template.md` — record searches, results, limitations, and next steps.
- `sample-data/simulated_auth_events.csv` — synthetic events for learning/report-writing; these are not real telemetry and are not automatically indexed into Splunk.

## Step 1 — Create the GitHub repository
1. On GitHub, create a repository named `splunk-siem-authentication-investigation`.
2. Add this project's files. Easiest: extract the ZIP, open the new repository, choose **Add file → Upload files**, drag in the extracted files/folders, then commit.
3. Keep the repository public only if it contains no personal data, secrets, internal hostnames, real IPs, customer data, or confidential logs.
4. Replace this README's status with `In progress` until you have run and documented the searches.

## Step 2 — Inventory data you can search
1. Open Splunk **Search & Reporting**.
2. Set the time picker to **Last 24 hours** (or a period known to contain events).
3. Open `queries/00_inventory.spl`, copy its search into Splunk, and run it.
4. Note the indexes returned. If no results appear, try **Last 7 days**, then **All time** only if allowed and appropriate.
5. Use only an index returned by your account's search. Do not assume `_internal` is readable.

## Step 3 — Inspect the accessible data
1. Replace `YOUR_ACCESSIBLE_INDEX` in `01_source_inventory.spl` with one index you can search.
2. Run the query and note the available `host` and `sourcetype` values.
3. Open `02_inspect_events.spl`, replace the placeholder, run it, and inspect the raw event plus extracted fields.
4. If the selected index does not contain authentication events, choose a suitable sourcetype/index or mark Windows authentication detection as **not testable with current data**. Do not fabricate a successful result.

## Step 4 — Test the Windows failed-logon query
1. Open `03_windows_failed_logons.spl`.
2. Replace `YOUR_ACCESSIBLE_INDEX`.
3. Run with a time range that includes the events.
4. The query supports common Windows event field variants (`EventCode` or `EventID`) but the source field names can differ. Inspect raw events and adapt only after confirming the actual schema.
5. If the query returns no events, that is not proof that no failed logons occurred. It may mean the index, time range, data source, permissions, or field names are unsuitable.

## Step 5 — Investigate, don't just count
For each candidate event, document:
- What triggered the review?
- Which fields support the hypothesis?
- Is there a burst of failures, multiple accounts, or a later successful login?
- Is the source expected/known? Do not label an IP malicious based on unfamiliarity alone.
- What additional evidence is required?
- Is this benign, suspicious, or inconclusive?
- Should the case be closed, monitored, or escalated?

## Step 6 — Publish evidence
Complete the templates under `reports/`. Add screenshots only after running your own searches. Blur/redact usernames, internal hostnames, and other sensitive values. Never upload raw employer/customer logs or credentials.

## Acceptance checklist
- [ ] Inventory search executed and time range recorded
- [ ] Accessible index and sourcetype recorded
- [ ] Raw event fields inspected
- [ ] At least one relevant detection search executed, or a clear data-access limitation documented
- [ ] Findings are based on observed evidence
- [ ] False-positive possibilities and limitations are discussed
- [ ] Incident report completed
- [ ] Screenshots are safe to publish
- [ ] README accurately distinguishes tested work from planned work

## Suggested future improvements
- Add a dashboard using fields confirmed in your data.
- Add a successful-login correlation only if the dataset contains reliable success/failure events and common user/source fields.
- Add a Sigma rule with a test event and expected result.
- Add a small synthetic dataset ingestion walkthrough in a separate disposable Splunk instance.

## References
- [Splunk Search Reference](https://docs.splunk.com/Documentation/SplunkCloud/latest/SearchReference/)
- [GitHub repository quickstart](https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories)
