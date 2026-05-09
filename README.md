 5G QoS & Multi-Plane Anomaly Command Center
A real-time 5G Core Network monitoring dashboard built with Streamlit. Simulates and visualizes AMF, SMF, and UPF network function loads, detects anomalies using Z-Score statistical analysis, monitors live QoS metrics via Scapy packet capture, and supports NSSF network slicing visualization — all in a dark-themed, glassmorphism UI.

Live Dashboard Preview

Run streamlit run app7.py and open http://localhost:8501

┌─────────────────────────────────────────────────────────────┐
│         5G QoS & Multi-Plane Anomaly Center                 │
├──────────────┬──────────────┬──────────────┬────────────────┤
│  AMF 42.3%   │  SMF 58.1%   │  UPF 71.4%   │  Active UEs 87 │
│  Dev: 1.2σ   │  Dev: 0.8σ   │ ⚠ Dev: 2.9σ  │                │
├──────────────────────────────┬──────────────────────────────┤
│  Latency: 12.4ms             │  URLLC Health: 78%  ████░░  │
│  Jitter:   3.1ms             │  eMBB Resource: 64% ███░░░  │
│  Throughput: 48.2 Kbps       │                              │
├──────────────────────────────┴──────────────────────────────┤
│  📊 Core Plane Chart    │  📊 QoS Trends Chart              │
├─────────────────────────────────────────────────────────────┤
│  📑 Anomaly Event Log                                       │
└─────────────────────────────────────────────────────────────┘

 Features

5G Core Network Functions — real-time simulation of AMF (Control Plane), SMF (Session Plane), and UPF (Data Plane) with load percentages
NWDAF Anomaly Detection — Z-Score statistical engine flags deviations above 2.5σ in real time
Attack Simulation — inject Signaling Storms, DDoS/Heavy Load, and Session Exhaustion scenarios with one click
NSSF Network Slicing — live URLLC and eMBB slice health bars with dynamic resource allocation
Live Packet Capture — Scapy sniffs real packets to compute actual latency, jitter, and throughput
Dual Live Charts — Core Plane performance graph + QoS trends area chart, both updating in real time
Event Intelligence Log — timestamped anomaly events with plane identification and deviation score
Glassmorphism UI — dark radial gradient background with glass-effect cards and animated slice bars


 Tech Stack
ToolPurposeStreamlitInteractive real-time web dashboardScapyLive packet sniffing for QoS metricsNumPyZ-Score anomaly detection, Poisson UE modelingPandasEvent log and chart data formattingPython dequeEfficient rolling time-window buffers

 Getting Started
Prerequisites
bashpip install streamlit scapy pandas numpy

 Run as Administrator (Windows) or with sudo (Linux/Mac) — Scapy requires raw socket access for live packet capture.

Run
bashstreamlit run app7.py
Then open your browser at:
http://localhost:8501

 Project Structure
5G_project/
│
└── app7.py        # Full dashboard — Core engine, anomaly detector, UI, live charts

How It Works
5G Core Simulation — CoreNetwork class
Each refresh cycle simulates realistic load on all three network functions:
AMF Load  = UE count × 0.5  (+spike if Signaling Storm attack)
SMF Load  = UE count × 0.6  (+lag if Session Exhaustion attack)
UPF Load  = UE count × 0.8  (+spike if DDoS attack)
UE Count  = Poisson arrival − Poisson departure (random walk)
Anomaly Detection — NWDAF Z-Score Engine
Z = |current_value − rolling_mean| / rolling_std

Z > 2.5σ  →  Anomaly flagged, card turns RED, event logged
Z ≤ 2.5σ  →  Normal operation, card stays CYAN
Requires minimum 15 data points before scoring begins (cold-start protection).
QoS Metrics — Live via Scapy
Latency    = time delta between consecutive packets (ms)
Jitter     = |current latency − previous latency| (ms)
Throughput = packet size × 8 / 1024 (Kbps)
Network Slicing — NSSF Engine
URLLC Health  = max(10,  95 − AMF_load × 0.5)   [critical services]
eMBB Resource = min(90,  UPF_load × 0.9)          [broadband services]

 Attack Simulation Guide
ButtonAttack TypeWhat it Does🚨 AMF STORMSignaling StormFloods AMF with 18 UE/s arrival rate, spikes control plane🚨 UPF DDoSDDoS / Heavy LoadAdds 45% load spike on UPF, degrades data plane✅ SYSTEM RECOVERYNoneResets all attack modes, network returns to baseline

 Sample Anomaly Log
TimePlaneDeviation18:42:11UPF3.2σ18:42:15AMF2.8σ18:42:19UPF4.1σ

 Future Improvements

 Export anomaly logs to CSV with one click
 SMS / email alert integration for critical anomalies
 mMTC slice support alongside URLLC and eMBB
 Historical playback mode for past incidents
 Docker containerization for easy deployment


License
MIT License — free to use, modify, and distribute.
