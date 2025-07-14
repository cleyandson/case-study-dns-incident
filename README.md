# Cybersecurity Incident Report: Network Traffic Analysis

### Project Context
This project is a practical case study developed as part of the <a href="https://www.coursera.org/google-certificates/cybersecurity-certificate">Google Cybersecurity Professional Certificate</a>. It documents the analysis of a simulated incident where users were unable to access the website `www.yummyrecipesforme.com`, encountering a "destination port unreachable" error.

The incident began when multiple clients reported they could not access the website, receiving a "destination port unreachable" error message. As a cybersecurity analyst, my task was to investigate the root cause of the problem by analyzing network traffic.

### Tools and Technologies
* **Packet Analysis:** `tcpdump`
* **Protocols Analyzed:** `UDP`, `ICMP`, `DNS`
* **Methodology:**
  1. Confirmation of the website access error.
  2. Capture of network packets during a new connection attempt for detailed analysis.
  3. Inspection of `UDP`, `DNS`, and `ICMP` protocols to identify the anomaly.

## Analysis and Findings
The analysis of the captured traffic revealed a clear pattern that pointed to a failure in the domain name resolution process.

1. **The Request:** "My computer" (`IP 192.51.100.15`) sent a standard `DNS` query via `UDP` to the server at `203.0.113.2`. The query asked for the IP address corresponding to `yummyrecipesforme.com`.
2. **The Evidence:** Instead of a `DNS` response, the server returned an `ICMP` error packet, as seen in the log below.
   ![TCPDump Log showing the ICMP error](https://github.com/cleyandson/case-study-dns-incident/blob/e04976d69d2b7580eafaf9c5c3d9e7080551657c/Documents/log-trafego-tcpdump.png)
3. **The Anomaly:** The error message within the `ICMP` packet was explicit: **`udp port 53 unreachable`**.

## Conclusion

The root cause of the incident was not a failure of the website's web server, but rather of the **DNS service** on the server `203.0.113.2`.

The "unreachable" message indicates that although the server was online to respond with an `ICMP` error, there was no service or process "listening" on port 53 to actually resolve the DNS query. This prevented any user from translating the website's name into an IP address, making access impossible.

The issue was properly documented and reported to the security engineers for investigation and correction on the affected server.

### Full Report in PDF
* [**Cybersecurity Incident Report**](https://github.com/cleyandson/case-study-dns-incident/blob/e04976d69d2b7580eafaf9c5c3d9e7080551657c/Documents/Cybersecurity%20incident%20report%20network%20traffic%20analysis.pdf)

---
*This project demonstrates practical skills in network analysis and incident documentation, as covered in the Google Cybersecurity Professional Certificate curriculum.*

