# Ticket Log

Every ticket worked in the lab. Incidents marked **simulated** were recreated on purpose to practice the full workflow.

## Ticket 1: Cannot open Sales shared folder

| Field | Detail |
|---|---|
| Type | Incident |
| Category | Access and Permissions |
| Requester | Jordan Lee (Sales) |
| Technician | Branden Francis |
| Urgency | Medium |
| Scenario | Simulated |
| Status | Closed |

**Report:** Access denied when opening \\LAB-DC01\Sales. It worked the day before.

**First response:** Acknowledged the ticket and asked for the exact error and whether other shares were affected.

**Diagnosis:**
1. Checked the account in Active Directory: enabled, not locked out
2. Checked group membership: only Domain Users, SalesTeam was missing
3. Root cause: the Sales share grants access through the SalesTeam group, so without membership the permissions did not apply

**Fix:** Added the user back to SalesTeam with `Add-ADGroupMember`, then verified with `Get-ADPrincipalGroupMembership`.

**Follow up for the user:** Sign out and back in so the new group is picked up.

**Result:** Solution approved by the requester and the ticket closed.

**Prevention:** Group membership is the access control, so changes to it should always come through a ticket.

---

## Ticket 2: Password reset request

Pending.

## Ticket 3: New hire onboarding

Pending.

## Ticket 4: Employee leaves

Pending.
