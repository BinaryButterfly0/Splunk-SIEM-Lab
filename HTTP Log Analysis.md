## 3️⃣ HTTP Log Analysis

### 🎯 Objective

Analyze **web (HTTP) traffic** to detect server/client errors, scripted attacks, and large file transfers.

---

### 🛠️ Setup

* **Dataset used**: [`ssh_logs.json`](./logs/http_logs.json)
* **Index created:** `http_lab`
* **Method:** Ingested the dataset into Splunk and ran SPL to surface web anomalies.

---

### 📊 Key SPL Queries

**Top 10 Endpoints (by source host)**

```spl
index=http_lab sourcetype="json"
| stats count by id.orig_h
| sort - count
| head 10
```

**Server Errors (5xx)**

```spl
index=http_lab sourcetype="json" status_code>=500 status_code<600
| stats count as server_errors
```

**Client Errors (4xx) — quick view**

```spl
index=http_lab sourcetype="json" status_code>=400 status_code<500
| stats count as client_errors
```

**Suspicious User-Agents (common tools/bots)**

```spl
index=http_lab sourcetype="json" user_agent IN ("sqlmap/1.5.1","curl/7.68.0","python-requests/2.25.1","botnet-checker/1.0")
| stats count by user_agent
| sort - count
```

**Large File Transfers (>500 KB)**

```spl
index=http_lab sourcetype="json" resp_body_len>500000
| table ts id.orig_h id.resp_h uri resp_body_len
| sort - resp_body_len
```

**Top URIs Requested (optional)**

```spl
index=http_lab sourcetype="json"
| stats count by uri
| sort - count
| head 20
```

**Status Code Distribution (optional)**

```spl
index=http_lab sourcetype="json"
| stats count by status_code
| sort - count
```

---

### 📌 Insights

* Highlighted **noisy sources** driving the most requests.
* Quantified **server-side failures (5xx)** and **client issues (4xx)**.
* Flagged **automated/scanner traffic** via suspicious User-Agent strings.
* Surfaced potential **data exfiltration / heavy downloads** by finding large responses.
