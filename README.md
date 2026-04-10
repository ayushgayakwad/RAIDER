# RAIDER
### **R**einforcement **A**gents for **I**ntelligent **D**iscovery and **E**xploit **R**esearch

**RAIDER** is an autonomous AI Red Teaming system that combines **Reinforcement Learning (RL)** for strategic decision-making with **Large Language Models (LLM)** for tactical execution.

Instead of hard-coded scripts, RAIDER uses a Q-Learning "Commander" to decide *when* to scan and *which*  specific attack vector (SQLi or XSS) to deploy. An LLM-driven "Exploit Agent" analyzes web pages in real-time, navigates complex DOM structures (including iFrames), and generates context-aware payloads to bypass authentication or execute arbitrary JavaScript.

---

## Project Report

Full project report available here:  
[View Report](https://github.com/ayushgayakwad/RAIDER/blob/main/RAIDER%20-%20Project%20Report.pdf)

---

## Dashboard Preview

![RAIDER Dashboard](figures/assets/RAIDER%20Dashboard.jpg)

*The interactive RAIDER dashboard showing mission control, live intelligence, and agent logs in real time.*

---

## RAIDER in Action

https://github.com/user-attachments/assets/8bbd0deb-cb1d-4662-8c1f-663ef6b341ca

*Autonomous reconnaissance and exploitation using RL-driven decision making and LLM-based tactical execution.*

---

## Architecture

RAIDER operates using a **Blackboard Architecture** where specialized agents collaborate:

1.  **The Commander (RL Core)** 
    * **Logic:** Q-Learning (Reinforcement Learning).
    * **Role:** Learns the optimal "Kill Chain" order. It dynamically chooses between **Recon**, **SQL Injection**, **XSS** or **WAIT** based on the target's state, previous actions and rewards.
2.  **The Discovery Agent (Recon)**
    * **Tools:** Nmap + Script Engine (```--script```).
    * **Role:** Identifies open ports, detects Operating Systems, fingerprints services, and scans for known vulnerabilities (CVEs).
3.  **The Intelligent Agent (Exploit)**
    * **Tools:** Google Gemini + Selenium (Context-Aware).
    * **Capabilities:**
        * **SQL Injection:** Parses HTML forms to generate payloads that bypass login screens.
        * **XSS:** Intelligently navigates through pages, prioritizes input vectors using AI, and performs deep scanning of iFrames to inject JavaScript payloads. 
4.  **The Blackboard (Shared Memory)**
    * **Role:** Acts as the central data repository. All agents read the mission state (scan results, flags, history) from here and publish their findings (vulnerabilities, logs) back to it, ensuring asynchronous collaboration.
5.  **The Reporter**
    * **Tools:** FPDF.
    * **Role:** Compiles all agent findings, logs, and evidence into a professional PDF audit report at the end of the mission.

---

## Installation

### 1. Critical Prerequisite: Nmap Binary
**You must install the Nmap application on your system.**
The Python library (`python-nmap`) is only a wrapper.

* **Windows:**  

    * Download the setup `.exe` from the official website: [https://nmap.org/download.html](https://nmap.org/download.html)

* **Linux (Debian/Ubuntu):**  

    ```bash
    sudo apt-get install nmap
    ```

* **macOS:**  

    ```bash
    brew install nmap
    ```

### 2. Project Setup

1.  **Clone the repository**
    ```bash
    git clone https://github.com/ayushgayakwad/RAIDER.git
    cd RAIDER
    ```

2.  **Install Python Dependencies**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Environment Configuration**
    Create a ```.env``` file in the root directory and add your Google Gemini API key (Generate Google Gemini API key from https://aistudio.google.com/api-keys):
    ```env
    GEMINI_API_KEY=your_actual_api_key_here
    ```

---

## Usage

### 1. GUI (Interactive Dashboard)
For a visual experience with real-time feedback, mission control, and intelligence monitoring, launch the graphical dashboard.

```bash
python gui_app.py
```

* **Features:**
    * **Mission Control:** Start, Pause, Resume, and Terminate missions with a click.
    * **Real-time Intelligence:** View open ports, OS detection, and vulnerability findings as they happen in the "Intelligence" panel.
    * **Live Logs:** Watch the agents collaborate in the integrated console.
    * **Metric Visualization:** Track the RL Agent's rewards and current step progress.
    * **One-Click Reporting:** Open the generated PDF report directly from the dashboard upon completion.

### 2. CLI (Command Line Interface)
### Option A: Quick Start (Mock Mode)
To see RAIDER in action without an external target, simply run the main script and press **ENTER** when prompted for a URL. This launches a local vulnerable Flask server (The "SecureCorp Bank").

```bash
python main.py
Enter Target URL (or press Enter for Localhost): # Just press Enter to activate the Mock Mode
```

* **Target**: http://127.0.0.1:5000
* **Vulnerabilities to Find**: 
    1. **SQL Injection**: Bypass the login screen and capture the flag ```FLAG{MULTI_AGENT_DOMINATION}```.
    2. **XSS**: Inject scripts into the "Feedback" page (navigated to automatically by the AI).

### Option B: Live Targeting (Admin Rights Recommended)
You can point RAIDER at a specific URL or IP address (ensure you have permission!).

For Deep Recon (OS Detection & UDP Scans), run the script with Administrator/Root privileges.

**Windows (PowerShell as Admin):**
```bash
python main.py
Enter Target URL (or press Enter for Localhost): # Enter the URL of the target website
```

**Linux/Mac:**
```bash
sudo python main.py
Enter Target URL (or press Enter for Localhost): # Enter the URL of the target website
```

* A list of few sample live target websites is available in ```sample_live_targets.md```.

* **Output:** Upon completion, RAIDER generates a ```Mission_Report_YYYYMMDD.pdf``` containing a timeline of attacks, vulnerabilities found, and system details.

---

## Project Structure

| File | Description |
| :--- | :--- |
| `gui_app.py` | A modern GUI dashboard for interactive mission control, visualization, and real-time monitoring. |
| `main.py` | The CLI entry point. Initializes the Blackboard, Agents, and starts the mission loop. |
| `coordination_core.py` | The Reinforcement Learning brain (Q-Learning) that manages the strategic decision-making process, choosing between Recon, SQLi, XSS, and Wait actions based on rewards. |
| `agents_exploit.py` | The AI-driven offensive unit. Leverages Selenium and Gemini to perform SQL Injection and XSS attacks. |
| `agents_recon.py` | The scanner agent. Wraps Nmap to perform deep inspection. |
| `blackboard.py` | Shared memory state. Agents read/write findings here. |
| `reporting.py` | Generates professional PDF audit reports from mission data. |
| `mock_target.py` | A vulnerable Flask application used for training and demo purposes. |
| `train_manager.py` | Simulation script to pre-train the RL brain on the multi-vector attack strategy. |
| `inspect_brain.py` | A utility script to visualize the learned Q-Table policies, showing the agent's preferred actions for different system states. |
| `mission_control.pkl` | The serialized "brain" of the RL agent (saved Q-Table). |

---

## Disclaimer
### DISCLAIMER: FOR EDUCATIONAL PURPOSES ONLY.

RAIDER is a proof-of-concept tool designed for **authorized security research** and **capture-the-flag (CTF) challenges**.

* Do not use this tool against targets you do not have explicit permission to test.
* The developer is not responsible for any misuse of this software.
