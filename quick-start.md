---
title: Quick Start Guide
layout: default
nav_order: 2
---

# ⚡ Org Unit Sync – Quick Start Guide (v1.0)

---

## Purpose

This **Quick Start Guide** provides a **minimal, safe workflow** for synchronizing
organisation unit (OU) metadata between DHIS2 instances using the **Org Unit Sync**
application.

📘 For detailed explanations, edge cases, and governance guidance, refer to the
**Org Unit Sync – User Guide v1.0**.

---

## Target Audience

This guide is intended for **experienced DHIS2 administrators** and implementers:

- DHIS2 system administrators
- National / provincial HIS managers
- HISP implementers

You are expected to already understand:
- Organisation unit hierarchies
- DHIS2 user roles and authorities
- DHIS2 Routes (recommended)

---

## What This App Does (Quick Summary)

✅ Add missing organisation units  
✅ Update organisation unit attributes  
✅ Move organisation units between parents  
✅ Validate changes via **Dry Run** before execution  
✅ Generate audit-ready logs and reports  

🚫 Does **not** sync data values  
🚫 Does **not** delete organisation units  

---

## Prerequisites Checklist

### Target DHIS2 Instance
- Org Unit Sync app installed via **App Management**
- Logged-in user has:
  - Organisation Unit **Add / Update**
  - Organisation Unit **Move**
  - Required system authorities
- **Superuser role** is fully supported

### Source DHIS2 Instance
- Read access to organisation units
- Accessible via:
  - ✅ **DHIS2 Route (recommended)**
  - ⚠️ Direct URL (CORS must be enabled)

---

## Quick Workflow Overview


⚠️ **Never skip the Dry Run step**

---

## Step 1: Connect

1. Open **Org Unit Sync**
2. Select **Proxy via TARGET**
3. Choose an enabled **DHIS2 Route**
4. Click **Test connection**

✅ Proceed only when **“Connection OK”** is shown  
❌ Do not proceed if the connection fails

---

## Step 2: Define Scope

1. Select **Root organisation unit** (source)
2. Choose **Matching Identifier**
   - Recommended: **UID**
3. (Optional) Apply filters:
   - Organisation Unit Levels
   - Organisation Unit Groups
4. Leave behaviour rules at default unless you fully understand them
5. Click **Continue to Compare**

🔒 Scope is now locked and cannot be changed without restarting

---

## Step 3: Compare

1. Click **Run Compare**
2. Review the **Compare Summary**
3. Open the **Detailed View**
4. Review:
   - Adds
   - Updates
   - Moves
   - Excess (read-only)

📥 **Download Compare CSV** for review and approval (recommended)

---

## Step 4: Select Changes

- ✅ Select **Adds + Updates** first
- ❌ Do **not** select **Update + Move** for the same org unit
- Use:
  - Select All
  - Clear All
  - Row-level selection

---

## Step 5: Dry Run (Mandatory)

1. Go to **Sync** tab
2. Click **Dry Run**
3. Confirm:
   - No errors
   - Warnings are acceptable
4. If blocked:
   - Resolve **Update + Move conflict**
   - Re-run Dry Run

🚫 Never execute without a successful Dry Run

---

## Step 6: Execute

1. Click **Execute Sync**
2. Monitor live execution output
3. Confirm final status for all org units

📥 Download **Execution Report** (CSV / JSON)

---

## Recommended Safe Production Workflow


This minimizes hierarchy risks and ensures clean transitions.

---

## Common Mistakes to Avoid

❌ Skipping Dry Run  
❌ Mixing Update + Move in one execution  
❌ Using Code-based matching without strict governance  
❌ Running directly on production without review  
❌ Ignoring audit exports  

---

## Outputs to Archive

You should archive:
- Compare CSV
- Dry Run report
- Execution report
- Audit log (CSV / JSON)

These are required for:
- Ministry of Health governance
- WHO reporting
- Change management records

---

## When to Stop and Seek Support

- Unexpectedly large number of Moves
- Repeated Dry Run failures
- Permission-related errors
- Unclear hierarchy changes

---

## Support

**Developer:** HISP Sri Lanka  
**Application:** Org Unit Sync v1.0.0  

📘 Refer to the **User Guide** for full documentation.

---

*End of Quick Start Guide*
