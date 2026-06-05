# QuantumFlow Virtual Appliance (OVA) — Getting Started

This is a ready-to-run virtual machine that contains the entire QuantumFlow flow-monitoring stack. Import it into VMware ESXi, power it on, and open the dashboard in your browser. No software installation required.

---

## Quick reference

### Which file do I use?

| File | Use this if your VMware ESXi version is… |
| --- | --- |
| `QuantumFlow-v0.12.0-esxi67.ova` | **ESXi 6.7** |
| `QuantumFlow-v0.12.0-esxi7plus.ova` | **ESXi 7.0 or newer** |

### Login credentials

> Keep these somewhere safe. You will need them to log in to the dashboard and (optionally) the virtual machine itself.

**Dashboard (Kibana) — the main web page you'll use:**

| User | Password |
| --- | --- |
| `oyuai` | `elastic` |

**Virtual machine console / SSH (advanced, usually not needed):**

| Role | User | Password |
| --- | --- | --- |
| System login (sudo) | `quantumflow` | `quantumflow` |

**Internal service accounts (you normally never touch these):**

| Account | User | Password |
| --- | --- | --- |
| Search backend (Kibana ↔ Elasticsearch) | `kibana_system` | `changeme` |
| Search admin (Elasticsearch) | `elastic` | `changeme` |

---

## Step-by-step: get up and running

You only need a web browser and access to your VMware ESXi server.

### Step 1 — Pick the right file

Look at the table above and choose the `.ova` file that matches your ESXi version.

### Step 2 — Import the appliance into ESXi

1. Open your **VMware ESXi web page** in a browser and log in.
2. Click **Create / Register VM**.
3. Choose **Deploy a virtual machine from an OVF or OVA file**, then click **Next**.
4. Give it a name (for example, `QuantumFlow`), then drag in the `.ova` file you chose in Step 1.
5. Click **Next** through the remaining screens, accept the defaults, and click **Finish**.
6. Wait for the upload to complete. (The file is large — around 12 GB — so this can take a while.)

### Step 3 — Turn it on

1. In ESXi, select your new `QuantumFlow` virtual machine.
2. Click the **Power on** button.
3. Give it a few minutes to start up the first time.

### Step 4 — Find its address

1. With the virtual machine selected in ESXi, look at the **Summary** page.
2. Note the **IP address** shown for the machine (it looks something like `192.168.1.50`).

### Step 5 — Open the dashboard

1. On any computer on the same network, open a web browser.
2. Go to: **`http://<IP-address>:5601`**
   (for example, `http://192.168.1.50:5601`)
3. Log in with:
   - **Username:** `oyuai`
   - **Password:** `elastic`

You're in! This is the Kibana dashboard where you'll view your flow data.

### Step 6 — Send your network data to it

Point your network devices' flow exporters (NetFlow / IPFIX / sFlow) at the appliance's IP address. Default ports:

- NetFlow v9: `2055` (UDP)
- IPFIX: `4739` (UDP)
- sFlow: `6343` (UDP)

Once data starts arriving, it will appear in the dashboard.

---

## Caveats & notes

- **First boot is slow.** The search engine inside the appliance needs a few minutes to initialize before the dashboard becomes available. If `http://<IP>:5601` doesn't load immediately, wait a couple of minutes and refresh.
- **File size.** Each OVA is roughly 12 GB. Make sure you have the disk space and a stable connection before importing.
- **Change the default passwords.** The credentials above are shipped defaults. For any deployment beyond testing, change them — anyone who can reach the appliance can log in with these.
- **No HTTPS by default.** The dashboard is served over plain `http://` on port 5601. Do not expose it directly to the public internet without putting it behind a secure gateway/VPN.
- **The `quantumflow` / `quantumflow` login** is the operating-system account on the VM (used via the ESXi console or SSH). You only need it for advanced maintenance — day-to-day use happens entirely in the web dashboard.
- **Resources.** The appliance runs a full Elasticsearch + Kibana stack. Give the VM at least **8 GB of RAM** for smooth operation.
- **Ports.** Make sure your network/firewall allows the flow ports (2055, 4739, 6343 UDP) to reach the appliance, and port 5601 (TCP) for the dashboard.
- **License.** The appliance ships with a bundled license. If flow processing reports a license error, contact OyuAI for an updated token.
