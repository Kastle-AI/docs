# Relcu DTMF Menu Setup and Routing Coordination with Kastle

## Overview
Kastle’s Warm Transfer Agent connects qualified borrowers to Relcu’s distribution system.  
Routing is handled through **DTMF input** — short sequences of keypress tones sent during the call.

To ensure seamless transfers, both systems coordinate on which digits correspond to each routing path.  
Relcu defines the digit-to-path mapping, and Kastle’s Warm Transfer Agent sends those digits automatically during each transfer.

---

## Call Flow Diagram

```
 Borrower
     │
     ▼
┌────────────────────┐
│ Kastle Voice Agent │
│  (identifies lead) │
└─────────┬──────────┘
          │
          ▼
┌────────────────────────┐
│ Kastle Warm Transfer    │
│  Agent                  │
│  - Calls Relcu number   │
│  - Sends DTMF digits    │
└─────────┬──────────────┘
          │
          ▼
┌────────────────────────┐
│ Relcu IVR / Distribution│
│  - Receives DTMF input  │
│  - Routes call to team  │
└─────────┬──────────────┘
          │
          ▼
┌────────────────────────┐
│ Loan Officer / Team     │
│  Receives live transfer │
└────────────────────────┘
          ▲
          │
          │ (Lead data + DTMF digits)
          │
┌────────────────────────┐
│ Relcu → Kastle Data Feed│
│  - Sends lead details   │
│  - Includes DTMF digits │
└────────────────────────┘
```

---

## Call Flow Description

1. **Kastle Voice Agent A**  
   - Determines the borrower is ready for a live transfer.  
   - Passes control to Kastle’s Warm Transfer Agent.

2. **Warm Transfer Agent → Relcu**  
   - Calls Relcu’s main distribution number.  
   - Waits for Relcu’s IVR to start listening.  
   - Sends the DTMF digits associated with the correct routing path.  
   - Relcu routes the call accordingly, and the borrower is connected to the right team or loan officer.

3. **Relcu → Kastle (Data Feed)**  
   - Relcu already sends lead data that Kastle uses for outbound calling.  
   - As part of that data payload, Relcu should also include the **DTMF digits** Kastle must send when performing a warm transfer for that lead.  
   - Kastle will store those digits in its system and automatically press them when initiating the transfer.

---

## What Relcu Needs to Implement

### 1. Create a DTMF Menu
Relcu should create a short IVR menu that allows calls to be routed using DTMF tones.

| Option | DTMF Input | Routes To |
|--------|-------------|-----------|
| 1 | `1` | Distribution Path A |
| 2 | `2` | Distribution Path B |
| 3 | `3` | Distribution Path C |
| 4 | `4` | Distribution Path D |
| 9 | `9` | Default or test route |

Each route should align with an internal queue, team, or campaign within Relcu’s system.

---

### 2. Provide DTMF Digits in Lead Data
When sending lead data to Kastle, Relcu should include an additional field, such as:

| Field | Example Value | Description |
|--------|----------------|-------------|
| `dtmf_digits` | `2` | The digits Kastle should send to Relcu’s IVR for this lead’s route |

This ensures that when Kastle later connects a borrower via Warm Transfer, the correct digits are automatically pressed — no manual mapping or routing logic is required on Kastle’s side.

---

### 3. Keep the IVR Greeting Short
The IVR greeting should be brief to allow the Warm Transfer Agent to send tones right after the call connects.  

Example:  
> “Thank you for calling. Please enter your routing code.”

---

### 4. Timeout and Default Routing
- Allow at least five seconds for DTMF input before defaulting to a fallback route.  
- If no input is received, send the call to the general queue.

---

## Testing Procedure

1. Relcu provides test numbers for each menu option and confirms the corresponding digit mappings.  
2. Relcu includes the correct DTMF digits in sample lead data sent to Kastle.  
3. Kastle initiates test transfers using those digits.  
4. Verify that the call reaches the correct Relcu route.  
5. Confirm that end-to-end data and call routing are consistent.

---

## Summary

✅ **Relcu responsibilities**
- Create a DTMF menu for inbound transfers.  
- Keep greetings short and allow time for tone entry.  
- Send the corresponding DTMF digits as part of each lead’s data payload to Kastle.  
- Maintain alignment between DTMF options and routing logic.

✅ **Kastle responsibilities**
- Automatically dial the correct DTMF digits during warm transfers based on the data received.  
- Manage call initiation, borrower handoff, and confirmation testing.

---

*Version 1.0 — Kastle x Relcu Integration Guide*
