# Enterprise IP-PBX & Call Center System (Asterisk 20)

## Project Overview

This project is an enterprise-style telephone system built using **Asterisk 20, PJSIP, and Ubuntu Linux 22.04**.

It allows a company to manage incoming and outgoing calls automatically and route customers to different departments such as Sales, Support, and Operator.

The system includes an IVR menu, automatic call routing, support queues, call recording, voicemail, and call history.

## How It Works

When a customer calls the main helpline (Extension 500), the system follows this process:

1. **Business Hours Check:** Checks whether the company is open or closed.
2. **IVR Menu:** During business hours, callers hear a menu:
   - Press 1 for Sales.
   - Press 2 for Support.
   - Press 0 for Operator.
3. **Call Routing:** Calls are automatically sent to the selected department.
4. **After-Hours Handling:** When the company is closed, callers hear an after-hours greeting and can leave a voicemail.
5. **Call Recording:** Calls are recorded automatically.
6. **Call Logging:** Call details are saved for monitoring and auditing.

## System Architecture

```text
              Customer / SIP Trunk
                       |
                       v
                Main Helpline
                 Extension 500
                       |
                       v
              Business Hours Check
                 /           \
                /             \
          Open Hours        Closed Hours
              |                  |
              v                  v
           IVR Menu         Greeting & Voicemail
              |
       +------+------+------+
       |             |      |
     Press 1       Press 2  Press 0
       |             |      |
       v             v      v
     Sales         Support  Operator
       |             |      |
       v             v      v
   Ring Group     ACD Queue Extension 1003
       |             |
       +------+------+
              |
              v
       Call Recording
        & Call Logging
```

## Main Features

- **SIP Extensions:** Configured three internal extensions (1001, 1002, and 1003) using PJSIP.
- **IVR System:** Voice menu with keypad input (DTMF) for department selection.
- **Time-Based Routing:** Automatically handles calls differently during and outside business hours.
- **Ring Groups:** Rings multiple Sales extensions simultaneously.
- **ACD Queue:** Manages incoming Support calls using the `ringall` strategy.
- **Music on Hold:** Plays custom audio while callers wait in the queue.
- **Call Recording:** Records calls using Asterisk `MixMonitor()`.
- **Call Logging:** Stores call information through CDR (Call Detail Records).
- **Voicemail:** Provides voicemail functionality for after-hours calls.
- **SIP Trunking:** Supports external telephone connectivity and outbound call routing.
- **NAT Support:** Includes PJSIP settings to help maintain SIP and audio communication across NAT networks.

## Technologies Used

| Technology | Purpose |
|---|---|
| Ubuntu Linux 22.04 | Operating system |
| Asterisk 20 | PBX and call management |
| PJSIP | SIP communication |
| SIP | Call signaling |
| RTP | Voice transmission |
| FFmpeg | Audio conversion |
| CSV / CDR | Call logging |

## Project Structure

```text
project/
│
├── configs/
│   ├── pjsip.conf          # SIP endpoints and trunk configuration
│   ├── extensions.conf    # Call routing and IVR dialplan
│   ├── queues.conf        # Support queue configuration
│   ├── voicemail.conf     # Voicemail settings
│   └── musiconhold.conf   # Music on Hold settings
│
└── README.md
```

## Key Asterisk Functions

| Function | Purpose |
|---|---|
| `Dial()` | Connects calls to extensions |
| `GotoIfTime()` | Checks business hours |
| `Background()` | Plays IVR audio and accepts keypad input |
| `Queue()` | Sends calls to a support queue |
| `MixMonitor()` | Records calls |
| `VoiceMail()` | Handles voicemail |
| `Goto()` | Moves between dialplan sections |

## What This Project Demonstrates

- Understanding of IP-PBX architecture.
- SIP and PJSIP configuration.
- Call routing and dialplan development.
- IVR and DTMF handling.
- Automatic Call Distribution (ACD).
- Call recording and monitoring.
- Linux-based telephony system configuration.
- SIP trunking and NAT configuration.

## Platform

- **Operating System:** Ubuntu Linux 22.04 LTS
- **PBX Software:** Asterisk 20
- **SIP Stack:** PJSIP
