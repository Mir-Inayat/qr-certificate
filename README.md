# QR Certificate Generator

> A TypeScript tool for generating verifiable QR-coded certificates — built for event organizers and workshop coordinators.

![TypeScript](https://img.shields.io/badge/TypeScript-0d1117?style=flat-square&logo=typescript&logoColor=3178c6)
![Node](https://img.shields.io/badge/Node.js-0d1117?style=flat-square&logo=nodedotjs&logoColor=3c873a)

---

## What it does

QR Certificate Generator automates the creation of digital certificates with embedded QR codes for verification. Designed for use at workshops, hackathons, and events where bulk certificate generation and authenticity verification are needed.

**Built for real use** — originally created to generate certificates for CBIT Open Source Community events and Git/GitHub workshops with 1000+ participants.

---

## Features

- **Bulk generation** — generate certificates for hundreds of participants from a CSV or data source
- **QR verification** — each certificate embeds a scannable QR code linking to a verification endpoint
- **Customizable templates** — swap in event branding, name, and date fields
- **TypeScript** — fully typed, easy to extend

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | TypeScript |
| Runtime | Node.js |
| QR Generation | QR code library |
| Output | PDF / PNG certificates |

---

## Setup

```bash
git clone https://github.com/Mir-Inayat/qr-certificate
cd qr-certificate
npm install
npm run generate
```

Configure your participant data and template in the config file before running.

---

## Use Case

Originally built to issue verifiable certificates at scale for CBIT COSC workshops and hackathons. Can be adapted for any event that needs tamper-evident digital certificates.
