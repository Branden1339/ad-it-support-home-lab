# Ticket Log

Every ticket worked in the lab. Incidents marked **simulated** were recreated on purpose to practice the full workflow. Passwords are never recorded in tickets or in this repository.

| # | Title | Type | Category | Status |
|---|---|---|---|---|
| 1 | Cannot open Sales shared folder | Incident | Access and Permissions | Closed |
| 2 | Password reset request | Request | Account and Password | Closed |
| 3 | New hire account setup | Request | Onboarding and Offboarding | Closed |
| 4 | Employee departure | Request | Onboarding and Offboarding | Closed |

---

## Ticket 1: Cannot open Sales shared folder

| Field | Detail |
|---|---|
| Type | Incident |
| Category | Access and Permissions |
| Requester | Jordan Lee (Sales) |
| Technician | Branden Francis |
| Urgency | Medium |
| Scenario | Simulated |

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

| Field | Detail |
|---|---|
| Type | Request |
| Category | Account and Password |
| Requester | Sam Carter (Sales) |
| Technician | Branden Francis |
| Urgency | Medium |
| Scenario | Simulated |

**Report:** Employee forgot their password and could not sign in.

**Identity check:** In production, verify the requester before any reset, for example by calling back on the number on file or getting manager approval. This protects against social engineering. Treated as verified in the lab.

**Fix:**
1. Reset the password with `Set-ADAccountPassword -Reset`
2. Tried to require a password change at next sign in with `Set-ADUser -ChangePasswordAtLogon`

**Problem found:** The second command failed because the account had **Password never expires** set, which conflicts with forcing a change at next sign in.

**Resolution:** Turned off `PasswordNeverExpires` on the account, then set the change at next sign in. Verified with `Get-ADUser -Properties PasswordExpired,PasswordNeverExpires`.

**Notes:**
- A blank `PasswordLastSet` is expected after forcing a change, because AD clears the timestamp
- The temporary password is shared through a secure channel, never written in the ticket
- "Password never expires" is a common audit finding and should be limited to specific service accounts

**Result:** Solution approved by the requester and the ticket closed.

---

## Ticket 3: New hire account setup

| Field | Detail |
|---|---|
| Type | Request |
| Category | Onboarding and Offboarding |
| Requester | HR |
| Technician | Branden Francis |
| Urgency | Medium |
| Scenario | Simulated |

**Request:** Create an account for a new Sales hire with access to the Sales shared folder only.

**Plan:** Create the account in the Sales OU, add to SalesTeam only, set a temporary password, and require a change at first sign in.

**Fix:**
1. Created the account with `New-ADUser` in `OU=Sales,DC=lab,DC=local`
2. Added it to SalesTeam with `Add-ADGroupMember`
3. Verified with `Get-ADUser` and `Get-ADPrincipalGroupMembership`: enabled, correct OU, groups are Domain Users and SalesTeam only

**Notes:** Because the account is in the Sales OU, the Group Policy that restricts Control Panel applies automatically. Placing users in the right OU applies the right policies without extra steps.

**Result:** Solution approved by the requester and the ticket closed.

---

## Ticket 4: Employee departure

| Field | Detail |
|---|---|
| Type | Request |
| Category | Onboarding and Offboarding |
| Requester | HR |
| Technician | Branden Francis |
| Urgency | High |
| Scenario | Simulated |

**Request:** Disable the account of a departing HR employee and remove access to shared folders.

**Fix:**
1. Disabled the account with `Disable-ADAccount`
2. Removed it from HRTeam with `Remove-ADGroupMember`
3. Recorded the date and ticket reference in the account description with `Set-ADUser -Description`
4. Verified: `Enabled` is False and only Domain Users remains

**Why disable instead of delete:** Disabling blocks sign in immediately while preserving the account history, ownership, and files for audits or review. Accounts are deleted only after a retention period.

**Result:** Solution approved by the requester and the ticket closed.

---

## Planned tickets

- Locked out account (needs the client machine)
- DNS issue: cannot reach websites by name
- Group Policy not applying

Why disable instead of delete: Disabling blocks sign in immediately while preserving the account history, ownership, and files for audits or review. Accounts are deleted only after a retention period.

Result: Solution approved by the requester and the ticket closed.
