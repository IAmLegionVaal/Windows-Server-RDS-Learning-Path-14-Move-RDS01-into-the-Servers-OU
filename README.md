# Windows Server RDS Learning Path 14 — Move RDS01 into the Servers OU

**Level:** Beginner · **Module:** 14/70

## Goal
Place `RDS01` in the dedicated RDS Servers OU and verify the intended Group Policy scope.

## Setup
1. Locate the RDS01 computer object.
2. Move it to `OU=Servers,OU=RDS,DC=corp,DC=lab`.
3. Confirm the OU is protected from accidental deletion.
4. Review delegated rights on the OU.
5. Refresh computer policy on RDS01.
6. Generate an RSoP report and confirm the expected baseline applies.

```powershell
$Target='OU=Servers,OU=RDS,DC=corp,DC=lab'
Move-ADObject -Identity (Get-ADComputer RDS01).DistinguishedName -TargetPath $Target
Get-ADComputer RDS01 | Select-Object Name,DistinguishedName
gpupdate.exe /force
gpresult.exe /h "$env:TEMP\RDS01-RSoP.html" /f
```

## Evidence
Capture the old/new distinguished name, OU properties, ACL/delegation, `gpresult` and GroupPolicy Operational events under `evidence/`.

## Troubleshooting
If the move is denied, verify source/destination permissions and exact DNs. If policy does not apply, inspect link, filtering, replication and computer-side scope.

## Security
Keep RDS servers in a dedicated OU so hardening and delegation remain narrow and reviewable.

## Rollback
Move RDS01 back to its prior OU only if the design is intentionally reversed.

## Next
`Windows-Server-RDS-Learning-Path-15-Apply-an-RDS-Server-Baseline-GPO`
