# Enterprise Multi-Department IP-PBX & Call Center System (Asterisk 20)

A production-grade, modular VoIP & Call Center PBX built from source on **Ubuntu Linux 22.04 LTS** using **Asterisk 20 (PJSIP stack)**. This project demonstrates end-to-end enterprise telephony architecture, including automated call distribution (ACD), time-based IVR routing, SIP trunking, compliance call recording, and custom telephony audio transcoding.

## System Architecture & Call Flow

    [ External SIP Trunk / Internal Caller ]
                       |
                       v
        +-----------------------------+
        |   Main Helpline (Exten 500) |
        |  Time-Based Routing Check   |
        +--------------+--------------+
                       |
         +-------------+-------------+
         v (Business Hours)          v (After Hours)
+-------------------------+ +-------------------------+
|   Multi-Level IVR Menu  | |   After-Hours Greeting  |
|     (DTMF Handling)     | |  & Voicemail Fallback   |
+-+----------+----------+-+ +-------------------------+
  |          |          |
[Press 1]  [Press 2]  [Press 0]
  |          |          |
  v          v          v
Sales      Support    Operator
Ring Group ACD Queue  (Exten 1003)
(1001&1003)(Custom MoH)
  |          |          |
  +----------+----------+
             v
+-----------------------------+
|   MixMonitor Call Recording |
|  Voicemail & CDR CSV Audit  |
+-----------------------------+

## Core Engineering Features

* **Modular PJSIP Architecture (`configs/pjsip.conf`):** Uses configuration inheritance templates (`endpoint-template`, `auth-template`, `aor-template`), NAT traversal best practices (`rtp_symmetric=yes`, `force_rport=yes`, `rewrite_contact=yes`, `direct_media=no`), and active endpoint health monitoring (`qualify_frequency=30`).
* **Smart Dialplan & Time-Based Routing (`configs/extensions.conf`):** Conditional time routing via `GotoIfTime()`, IVR with `Background()` DTMF interruption, invalid/timeout handlers (`i` and `t`), and Simultaneous Ring Groups.
* **Automatic Call Distribution (`configs/queues.conf`):** Dedicated `support-queue` using `ringall` strategy, retry intervals, and custom Music on Hold.
* **Compliance Call Recording & CDR Logging:** Automated WAV call recording via `MixMonitor()` with dynamic timestamp/CallerID filenames (`STRFTIME`) and full Call Detail Record (`Master.csv`) logging.
* **Custom Telephony Audio Transcoding:** Transcoded audio into telephony-compliant **8000 Hz, 16-bit, Mono PCM WAV** using `ffmpeg` for Music on Hold (`configs/musiconhold.conf`).
* **SIP Trunking & Digit Manipulation:** Outbound route pattern matching (`_903XXXXXXXXX`) with prefix stripping (`${EXTEN:1}`) and Caller ID presentation.

## Repository Structure

* `configs/pjsip.conf` - Transport, templates, endpoints (1001-1003), and SIP trunk
* `configs/extensions.conf` - Dialplan, IVR, time-based routing, ring groups, outbound/inbound routes
* `configs/queues.conf` - ACD Support Call Queue configuration
* `configs/voicemail.conf` - Mailbox definitions and PIN security
* `configs/musiconhold.conf` - Custom Music on Hold directory configuration
* `README.md` - Architecture documentation and setup guide
