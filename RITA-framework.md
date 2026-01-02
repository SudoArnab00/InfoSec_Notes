# RITA - Real Intelligence Threat Analytics
- Open source (by Active Countermeasures)
- Only accepts network traffic input as Zeek logs
- Main power is Analytics
	- corelates captured data
	- data normalization
	- runs analysis modules

## Primary features
- C2 beacon detection
- DNS tunneling detection
- Long connection detection
- Data exfiltration detection
- Checking threat intel feeds
- Score connections by severity
- Show the number of hosts communicating with external IP
- Show date time when the external host was first seen on network

## Notes
- Threat Modified that tells us the number of hosts communicating to destination is **Prevalance**
- RITA excels at beacon detection - regular, timed check-ins from infected hosts to C2 servers.
- High Beacon % + High Connection Count + Long/consistent Duration = **classic C2 beaconing.**
- Small datasets → more false positives & lower "First Seen" impact.
- Always cross-check findings:
	- VirusTotal for IPs/domains
	- Pivot back to raw Zeek logs or PCAP for deeper analysis

## Usage
- Search bar (press `/` to focus)
- Uses field-specific syntax:
	- Destination: `dst:malicious.com`
	- Source: `src:10.0.0.13`
	- Beacon score: `beacon:>=70`
	- Severity: `severity:high`
	- Combine: `dst:malhare.net beacon:>=70 sort:duration-desc`
- Clear filter: `Ctrl + X`
- Help: press `?` in search mode
- Browser report: `rita html-report <name>`
- List datasets: `rita list`
- Remove dataset: `rita delete <name>`
 
### Convert Zeek logs for RITA
` $ zeek readpcap pcaps/asyncRAT.pcap zeek_logs/asyncRAT`
### Import to RITA
` $ cd /zeek_logs/asyncRAT/ & ls`
` $ rita import --logs /asyncRAT/ --database asyncrat`
### View Results
` $ rita view asyncrat`

## Things to look for?
- **No direct connections**→ Hidden/sophisticated C2
- **Large outgoing data** → Possible exfiltration
- **Missing host header** → Common attacker oversight
- **MIME type/URI mismatch** → HTTP header says one thing, URI says another (evasion attempt)
- **Rare Signature** → Unusual patterns (e.g., unique User-Agent, TLS cert details) not seen elsewhere on network
- **First Seen**→ How new the destination is (recent = more suspicious)
- **Connection Count** → High count often = beaconing
- **Total Bytes** → High outgoing = possible exfiltration
- **Port : Proto : Service**→ Non-standard ports or plain HTTP = suspicious