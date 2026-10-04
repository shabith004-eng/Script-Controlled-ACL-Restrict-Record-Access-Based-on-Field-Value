# Script-Controlled ACL – Restrict Record Access Based on Field Value

| | |
|---|---|
| **Team ID** | SWTID-2026-7530 |
| **Date** | 4/10/2026 |
| **Platform** | ServiceNow |
| **Maximum Marks** | 5 Marks |

## Overview

This project improves record-level security in ServiceNow by restricting access to **Institution Details** records based on the user's role and the **Branch** field value. Script-controlled Access Control Lists (ACLs) are used to control the Read, Create, Write, and Delete operations on the `u_institution_details` table, so that only authorized users can work with protected records and administrators retain full access.

**User story:** As a ServiceNow administrator, I want to restrict access to Institution Details records based on user roles and branch values so that only authorized users can view, create, modify, and delete records.

## Objectives

- Provide controlled access to Institution Details records.
- Allow users to perform only the operations assigned to their roles.
- Restrict unauthorized users from accessing protected records.
- Allow administrators to retain full access to all records.

## Table: `u_institution_details`

| Field | Notes |
|---|---|
| Student Roll Number | |
| Student Name | |
| Faculty Name | |
| Branch | Choices: ECE, EEE, CSE |
| Email | |
| Phone Number | |
| Description | |

## Roles and ACL Design

| Operation | Required Role | Data Condition | Notes |
|---|---|---|---|
| Read | `bb1` | Branch is EEE | Advanced enabled; script allows admin and users with the required role |
| Create | `bb2` | None | Used together with Read access |
| Write | `bb3` | None | Used together with Read access |
| Delete | `bb4` | None | Used together with Read access |

Administrators have full access to all records regardless of Branch.

## Implementation Steps

1. Open ServiceNow with administrator access, create the **EEE User**, and create the roles `bb1`, `bb2`, `bb3`, and `bb4`. Assign the roles to the test user as required.
2. Create the `u_institution_details` table under **System Definition → Tables** with the fields above, then add test records with ECE, EEE, and CSE branch values.
3. Open **Access Control (ACL)**, elevate the `security_admin` role, and create the **Read** ACL on `u_institution_details` with Advanced enabled, role `bb1`, data condition *Branch is EEE*, and the access script.
4. Create separate **Create**, **Write**, and **Delete** ACLs requiring `bb2`, `bb3`, and `bb4` respectively.

## Testing

Testing is done by impersonating users in ServiceNow and opening `u_institution_details.list`.

| Test | User / Roles | Expected Result |
|---|---|---|
| Read | EEE user with `bb1` | EEE branch records are accessible |
| Read (denied) | User without the required role | Records cannot be accessed |
| Create | `bb1` + `bb2` | **New** is available and a record can be created |
| Write | `bb1` + `bb2` + `bb3` | An allowed EEE record can be edited and saved |
| Delete | `bb1` + `bb2` + `bb3` + `bb4` | An allowed EEE record can be deleted |
| Admin | Administrator | Full access to all records |

## Project Documentation

The project is documented in eight phases:

1. Brainstorming & Ideation Phase
2. Requirement Analysis Phase
3. Project Design Phase
4. Project Planning Phase
5. Project Development Phase
6. Project Testing Phase
7. Project Documentation Phase
8. Project Demonstration Phase

## Conclusion

The Script-Controlled ACL implementation provides structured record-level security in ServiceNow by combining roles, field conditions, and scripts to control access to Institution Details records.
