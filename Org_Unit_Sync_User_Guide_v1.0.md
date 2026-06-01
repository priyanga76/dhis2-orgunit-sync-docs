# DHIS2 Organisation Unit Sync App  
## User Guide (v1.0.0)

---

## 1. Introduction

**Org Unit Sync** is a DHIS2 web application developed by **HISP Sri Lanka** to safely compare and synchronize organisation unit (OU) metadata from a **source DHIS2 instance** to a **target DHIS2 instance**.

- **Source DHIS2** – authoritative structure  
- **Target DHIS2** – receiving structure  

The app is designed to support national and sub-national health information system implementations where organisation unit hierarchies must remain aligned across multiple DHIS2 instances (e.g. production, training, regional systems).

### Supported Operations

- Adding missing organisation units  
- Updating existing organisation unit attributes  
- Moving organisation units between parents  
- Validating changes via dry run before execution  
- Maintaining audit logs and exportable reports  

### Workflow Overview

1. Connect to the source DHIS2 instance  
2. Define scope and matching rules  
3. Compare organisation units  
4. Perform dry-run and execute synchronization  

> **Important:** No data values are affected — only organisation unit metadata.

---

## 2. Key Concepts

### 2.1 Actions Supported

| Action | Description |
|------|-------------|
| Add | Org units present in Source but missing in Target |
| Update | Attribute changes (name, short name, code, dates, etc.) |
| Move | Parent organisation unit change only |
| Excess | Org units present in Target but not in Source (read-only) |

---

### 2.2 Safe Design Principles

- Dry run before execution  
- Explicit user selection  
- Move separated from Update  
- Audit trail and exports  
- Guards against destructive operations  

---

## 3. Intended Users

This application is intended for:

- DHIS2 system administrators  
- Health informatics officers  
- National / provincial HIS managers  
- HISP implementers  

Users should already be familiar with:

- DHIS2 organisation unit concepts  
- DHIS2 user roles and authorities  
- DHIS2 Routes (for proxy mode)  

---

## 4. Prerequisites

### 4.1 Target DHIS2 Instance

- DHIS2 version **2.38+** (tested with 2.40–2.42)  
- App installed as a DHIS2 Web App  
- User must have appropriate system and metadata authorities  

### 4.2 Source DHIS2 Instance

- Read access to organisation units  
- Accessible either:
  - via **DHIS2 Route** (recommended), or  
  - directly via URL with CORS enabled  

### 4.3 User Permissions (Target)

**Metadata authorities**
- Add organisation unit  
- Update organisation unit  
- Delete organisation unit (optional, not used)  
- Move organisation unit  

**System authorities**
- Organisation unit management  

> Superuser role is fully supported.

---

## 5. Application Overview

The app interface is divided into five functional areas:

1. **Connect** – configure and test source connection  
2. **Scope** – define which organisation units to compare  
3. **Compare** – analyse differences between source and target  
4. **Sync** – apply changes (dry-run and execute)  
5. **Export & Audit** – download logs and reports  

Navigation between tabs is **progressively enabled** based on validation checks.

---

## 6. User Privileges and Security Model

### 6.1 Source Instance (Read-only)

The app **never modifies** the source DHIS2 instance.

Required capability:
- Read access to `/api/organisationUnits`

---

### 6.2 Target Instance (Write Access)

The app automatically checks the logged-in user’s privileges.

#### Required System Authorities
- Move organisation unit  
- Split organisation unit  
- Merge organisation units  
- Add organisation unit profile  
- Update organisation unit level  

#### Required Metadata Authorities
- Organisation Unit (Add / Update / Delete)  
- Organisation Unit Group (Add / Update / Delete)  
- Organisation Unit Group Set (Add / Update / Delete)  

> If metadata authorities cannot be fully evaluated via the API, progression is allowed, but DHIS2 will enforce permissions during execution.

---

## 7. Step-by-Step Usage Guide

---

## 7.1 Connect Tab

### Purpose
Establish a secure connection to the source DHIS2 instance and validate:
- Connectivity  
- Source read access  
- Target user privileges  

### What Happens
- Tests API connectivity through proxy or direct mode  
- Displays source instance information  
- Validates read access to organisation units  
- Validates write permissions on target  

---

### 7.1.1 Connection Mode

#### (a) Proxy via Target (Recommended)

- Uses DHIS2 Routes configured on the target instance  
- Avoids browser CORS issues  
- More secure (credentials not exposed)  

**Steps**
1. Select **Proxy via TARGET**
2. Choose an enabled Route from the dropdown  

---

#### (b) Direct to Source (Advanced)

- Browser directly calls the source instance  
- Requires CORS to be enabled on the source server  

**Steps**
1. Select **Direct to SOURCE**
2. Enter:
   - Source URL  
   - Username  
   - Password  

---

### 7.1.2 Test Connection

- Default test path: `/api/system/info`
- Click **Test connection**

On success, the app displays:
- Connection OK message  
- Source instance details  
- Source user & read access status  
- Target user & privilege evaluation  

> You cannot proceed unless the connection is successful.

---

## 7.2 Scope Tab

### Purpose
Define what organisation units should be compared and how they should be matched.

---

### 7.2.1 Organisation Units (Source)

- Select the root OU from the source instance  
- Options are shown hierarchically  

Examples:
- Root (all organisation units)  
- Ministry of Health  
- Provincial Health Services  

---

### 7.2.2 Matching Identifier

Determines how organisation units are matched between instances.

- **UID (recommended)** – safest and most reliable  
- **Code** – useful when UIDs differ but codes are harmonized  

---

### 7.2.3 Filters (Optional)

#### Organisation Unit Levels
- Restrict comparison to specific levels  
- Multiple selection allowed  

#### Organisation Unit Groups
- Restrict comparison to selected OU groups  

If no filters are selected, all org units in scope are included.

---

### 7.2.4 Behaviour Rules

- Treat same code under different parents as conflict  
- Ignore organisation units without code (code-based matching)  
- Allow parent reassignment (move)  

Click **Continue to Compare** to proceed.

This action:
- Locks the scope  
- Enables the Compare tab  
- Clears previous Sync outputs  

---

## 7.3 Compare Tab

### Purpose
Perform a **read-only analysis** of differences between source and target organisation units.

---

### 7.3.1 Run Compare

- Click **Run Compare**
- The app fetches all scoped organisation units from both instances  

---

### 7.3.2 Compare Summary

Displayed metrics:
- Total org units in source  
- Total org units after scope filters  
- Total org units in target  
- Matching org units  
- Org units to Add  
- Org units to Update  
- Org units to Move  
- Org units Excess in target  

---

### 7.3.3 Detailed View

Result categories:
- **Add** – present in source, missing in target  
- **Update** – attribute differences only  
- **Move** – parent differs only  
- **Excess** – present in target but not in source  

Features:
- Pagination  
- Column-wise comparison  
- Field-level discrepancy inspection  
- Select / deselect individual rows  

---

### 7.3.4 Add Section

Shows org units missing in target.

Columns:
- Select  
- Name  
- Short name  
- Code  
- Opening date  
- Parent org unit  
- Date created  

Buttons:
- Select All  
- Clear All  

---

### 7.3.5 Update Section

Shows org units where attributes differ (excluding parent).

You may safely update:
- Name  
- Short name  
- Code  
- Dates  
- Contact details  
- Coordinates  
- Comments  

---

### 7.3.6 Move Section

Shows org units where **only the parent differs**.

- Code is preserved  
- Names are preserved  
- Only `parent.id` is changed  

---

### 7.3.7 Update + Move Behaviour (Important)

If an org unit requires both:
- Attribute changes, and  
- Parent change  

It will appear in **both Update and Move sections**.

> You cannot run Update and Move for the same org unit in one execution.  
> The app enforces this with a guard.

---

### 7.3.8 Export Compare Results

Available downloads:
- Compare results (CSV)  
- Selected rows only (CSV)  
- Full comparison (JSON)  

Recommended for:
- Peer review  
- Audit documentation  
- Approval workflows  

---

## 7.4 Sync Tab

### Purpose
Safely apply the approved organisation unit changes to the target instance.

---

### 7.4.1 Ready for Dry Run Panel

Automatically updates counts based on selections:
- Organisation units to add  
- Organisation units to update  
- Organisation units to move  

---

### 7.4.2 Guard: Update + Move Conflict

If the same org unit is selected in both Update and Move:

> “Cannot run Update + Move for the same org unit in one execution.  
> Please run Add/Update first, then Move in a separate execution.”

Affected UIDs are listed.

---

### 7.4.3 Dry Run (Validation)

**What it does**
- Uses `importMode=VALIDATE`
- Fully simulates execution
- No changes are saved

**Outputs**
- Success count  
- Ignored items  
- Warnings  
- Errors  

Dry-run must complete successfully before execution is enabled.

---

### 7.4.4 Execute Sync

Enabled only after successful Dry Run.

- Executes selected Adds, Updates, Moves  
- Applies changes to Target instance  
- Generates detailed execution logs  

---

## 7.5 Logs and Audit Trail

### 7.5.1 Export Reports

Generated reports:
- Compare log  
- Dry-run report  
- Execution report  

Formats:
- CSV  
- JSON  

Each section exports separately:
- Adds  
- Updates  
- Moves  
- Summary  

---

### 7.5.2 Audit Trail (Local Storage)

Stored information:
- Timestamp  
- Logged-in user  
- Source instance  
- Target instance  
- Selected counts  
- Execution results  

Downloadable as:
- Combined CSV  
- Combined JSON  

---

## 8. Best Practices

- Always run **Compare → Dry-run → Review → Execute**
- Prefer UID-based matching unless codes are strictly governed
- Test on staging before production
- Archive exported logs
- Restrict usage to trained administrators

### Recommended Production Workflow

1. Compare  
2. Select Adds + Updates  
3. Dry Run  
4. Execute  
5. Re-Compare  
6. Select Moves  
7. Dry Run  
8. Execute  

---

## 9. Known Limitations

- Large hierarchies may take time to compare  
- Metadata authority evaluation depends on DHIS2 API exposure  
- Direct mode requires correct CORS configuration  
- Update + Move cannot run together  
- Excess org units are read-only  
- No automatic deletes  
- No data value synchronization  

---

## 10. Support and Maintenance

**Developer:** HISP Sri Lanka  
**Application:** Org Unit Sync v1.0.0  

For enhancements, bug fixes, or training:
- Coordinate with HISP Sri Lanka  
- Align changes with DHIS2 upgrade cycles  

---

## 11. Suggested Future Enhancements

- Role-based UI restrictions  
- Visual hierarchy diff view  
- Automated rollback support  
- Scheduled sync (read-only monitoring)  
- Integration with DHIS2 Maintenance App  

---
