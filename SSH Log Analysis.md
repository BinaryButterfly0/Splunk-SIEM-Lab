## 1️⃣ SSH Log Analysis

### 🎯 Objective

To analyze **SSH authentication logs** and detect patterns in login activity, such as failed attempts, successful logins, and source/destination mapping.



### 🛠️ Setup

* **Dataset used**: [`ssh_logs.json`](./logs/ssh_logs.json→ Zeek-style SSH logs in JSON format.
* **Index created**: `ssh_index`| main
* **Method**: Uploaded the dataset into Splunk and applied SPL queries to extract insights.

---

### 📊 Key SPL Queries

**Top Source IPs**

```spl
index=ssh_index
| stats count by id.orig_h
| rename id.orig_h as "Source IP"
| sort - count
```

**Top Destination IPs**

```spl
index=ssh_index
| stats count by id.resp_h
| rename id.resp_h as "Destination IP"
| sort - count
```

**Failed SSH Logins**

```spl
index=ssh_index auth_success=false
| stats count as "Failed SSH Logins"
```

**Successful SSH Logins**

```spl
index=ssh_index auth_success=true
| stats count as "Successful SSH Logins"
```

**Source → Destination Mapping**

```spl
index=ssh_index
| stats count by id.orig_h, id.resp_h
| rename id.orig_h as "Source IP", id.resp_h as "Destination IP", count as "Total Connections"
| sort - "Total Connections"
```

Dashoadrd Captutes
![Dashboard Screenshot](./Dashboard%20captures/image%20copy.png)
![Dashboard Screenshot](./Dashboard%20captures/image-1.png)
![Dashboard Screenshot](./Dashboard%20captures/image.png)


### 📌 Insights

* Ingested **real SSH logs** (`ssh_logs.json`) into Splunk.
* Identified **top attacker IPs** responsible for failed SSH login attempts.
* Distinguished between **failed vs successful authentication events**.
* Built a **source-to-destination mapping** to visualize SSH connection flows.
