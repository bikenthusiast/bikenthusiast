# Tobias Kapfhammer

**Systems engineer in Munich.** Requirements, verification and release decisions for safety-relevant embedded systems at CARIAD, BMW and Audi.

In my own time I build small systems end to end and write down why they are built the way they are. Three of them run together on one Raspberry Pi behind a smart mirror.

## How it fits together

A presence sensor wakes the display. A time-of-flight sensor picks up hand gestures; pi-edge-ai turns them into events and publishes them over MQTT to the mirror, which switches a lamp over Bluetooth LE.

```mermaid
flowchart TB
    PIR[Presence sensor] -- GPIO --> PRES
    TOF[ToF sensor 8×8<br/>DFRobot SEN0628] -- UART --> EDGE

    subgraph PI["Raspberry Pi 4"]
        PRES[Presence service<br/>raspberrypi-magicmirror]
        EDGE[pi-edge-ai<br/>gesture recognition + events]
        MQ[[Mosquitto<br/>MQTT broker]]
        MM[MagicMirror]
        LAMPD[Lamp daemon<br/>dyson-lamp-ble]
        EDGE -- gesture events --> MQ
        MQ --> MM
        MM -- gesture action<br/>Unix socket --> LAMPD
    end

    PRES -- HDMI-CEC<br/>standby / wake --> DISP[Display]
    MM --> DISP
    LAMPD -- Bluetooth LE --> LAMP[Lamp]
```

## Projects

**[pi-edge-ai](https://github.com/bikenthusiast/pi-edge-ai)** · Python, LiteRT, SQLite, MQTT  
Machine learning on a Raspberry Pi: image classification and gesture recognition, turned into clean events.  
*Shows:* benchmarks under documented conditions that keep model agreement apart from accuracy, Architecture Decision Records, a versioned event schema, deployment only after the tests pass.

**[dyson-lamp-ble](https://github.com/bikenthusiast/dyson-lamp-ble)** · Python, Bluetooth LE, GitHub Actions  
Local control of a Bluetooth LE lamp: the protocol analysed and re-implemented for interoperability.  
*Shows:* a written interface specification, a secure challenge / response handshake, cryptography checked against official RFC and NIST test vectors.

**[raspberrypi-magicmirror](https://github.com/bikenthusiast/raspberrypi-magicmirror)** · Python, systemd, embedded Linux  
The smart mirror that ties it together.  
*Shows:* a reproducible rebuild from one bootstrap script, hardened services, secrets kept out of Git.

**[airgap-k8s-lab](https://github.com/bikenthusiast/airgap-k8s-lab)** · Kubernetes, Shell, PKI  
A four-node Kubernetes cluster built from scratch without internet access.  
*Shows:* an own certificate authority and registry, deliberate failure injection, an error journal per exercise.

## Background

- **CARIAD SE**, since 2023: Systems Development Engineer. Traceability for 100+ requirements in IBM DOORS, release recommendations, test automation that cut commissioning runs from five days to one
- **BMW** and **Audi**, 2020–2023: field validation of a driver assistance function; a Hardware-in-the-Loop test bench built and commissioned
- **Technical University of Munich**: M.Sc. Automotive Engineering, B.Sc. Mechanical Engineering and Management
- Certified in software architecture (iSAQB CPSA-F) and software testing (ISTQB CTFL)

## Contact

[Tobias Kapfhammer on LinkedIn](https://linkedin.com/in/tobias-kapfhammer-tum)
