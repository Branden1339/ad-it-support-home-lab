# Commands Reference

Commands used in the lab and what each one does. Passwords are replaced with placeholders.

## Troubleshooting (Windows)

| Command | Purpose |
|---|---|
| `ping <gateway>`, `ping 8.8.8.8`, `ping google.com` | Layered network test: gateway, then internet IP, then name lookup |
| `ipconfig /all` | Show IP address, gateway, and DNS settings |
| `nslookup <name>` | Test DNS name resolution |
| `whoami /groups` | Show the groups in the current sign in session |
| `gpresult /r /scope user` | Show which Group Policy objects and groups apply to the user |
| `nltest /sc_verify:lab.local` | Check the secure channel to the domain |

## Active Directory (PowerShell)

| Command | Purpose |
|---|---|
| `Get-ADUser <user> -Properties Enabled,LockedOut` | Check whether an account is enabled or locked |
| `Get-ADPrincipalGroupMembership <user>` | List a user's groups |
| `Add-ADGroupMember -Identity <group> -Members <user>` | Add a user to a group |
| `Remove-ADGroupMember -Identity <group> -Members <user>` | Remove a user from a group |
| `Set-ADAccountPassword -Identity <user> -Reset` | Reset a password |
| `Set-ADUser <user> -ChangePasswordAtLogon $true` | Require a new password at next sign in |
| `Set-ADUser <user> -PasswordNeverExpires $false` | Remove the never expires flag |
| `New-ADUser ... -Path "OU=Sales,DC=lab,DC=local"` | Create an account in an OU |
| `Disable-ADAccount -Identity <user>` | Disable an account |
| `Unlock-ADAccount -Identity <user>` | Unlock an account |
| `Search-ADAccount -LockedOut` | List locked accounts |

## Permissions

| Command | Purpose |
|---|---|
| `icacls <folder> /inheritance:r` | Remove inherited permissions |
| `icacls <folder> /grant "LAB\SalesTeam:(OI)(CI)M"` | Give a group Modify on a folder and its contents |

## GLPI server (Linux)

| Command | Purpose |
|---|---|
| `ip -br a` | Show network interfaces and addresses |
| `ssh <user>@<ip>` | Open a secure remote terminal |
| `sudo apt update && sudo apt upgrade -y` | Update the system |
| `sudo apt install -y docker.io docker-compose-v2` | Install Docker and Docker Compose |
| `docker compose up -d` | Start the containers in the background |
| `docker compose ps` | Check container status |
| `docker logs <container> --tail 30` | Read recent container logs |
