# Header Analysis Playbook

## Objective

Verify the authenticity of the sender and identify signs of spoofing.

---

## Step 1 — Open Original Email

Obtain:

- .eml file
- Show Original
- Internet Headers

---

## Step 2 — Verify From

Check:

- Display Name
- Email Address
- Domain

Questions:

- Who claims to have sent the email?
- Does the domain look legitimate?

---

## Step 3 — Verify To

Check:

- Targeted recipient
- CC
- BCC

Questions:

- Single target?
- Multiple targets?
- Spear phishing?

---

## Step 4 — Verify Date

Check:

- Date
- Time
- Timezone

Correlate with:

- SIEM
- EDR
- Firewall

---

## Step 5 — Verify Subject

Check for:

- Urgency
- Payment
- Password Reset
- Invoice
- HR

Search mail gateway for identical subjects.

---

## Step 6 — Compare From and Reply-To

Questions:

- Same domain?
- Different organization?
- Redirected replies?

---

## Step 7 — Verify Return-Path

Questions:

- Same organization?
- Infrastructure looks legitimate?

---

## Step 8 — Analyze Received Headers

Extract:

- Sending Mail Server
- Public IP
- Receiving Mail Server
- Timestamp

Remember:

Read Bottom → Top

---

## Step 9 — Verify SMTP Server

Compare:

Received Header

↓

MX Records

↓

SPF

↓

DKIM

↓

DMARC

---

## Step 10 — Review Authentication-Results

Check:

- SPF
- DKIM
- DMARC

Remember:

PASS ≠ Safe

---

## Step 11 — Review Message-ID

Check:

- Present?
- Properly formatted?
- Search SIEM

---

## Step 12 — Review X-Spam-Status

Check:

- Spam Score
- Threshold
- Spam Decision

---

## Evidence Collected

- Claimed Sender
- Sending Mail Server
- SMTP IP
- Reply-To
- Return-Path
- SPF
- DKIM
- DMARC
- Message-ID
- Spam Status