# Diocese of Kigoma Multi-Parish Architecture Plan

## Status
Design plan only. The live application remains a single-parish Kazuramimba system. Do not onboard another parish or change production Firestore rules until migration and security tests are complete.

## Goals
- Support multiple parishes under the Diocese of Kigoma.
- Keep each parish's members, families, sacraments, contributions, communities, transfers and audit records private.
- Give diocesan officers explicitly authorised views across assigned parishes.
- Preserve existing Kazuramimba document IDs and history.
- Allow staff replacement without deleting historical records or sharing logins.

## Proposed identity model
- Diocese: `dioceses/{dioceseId}`
- Parish: `parishes/{parishId}` with `dioceseId`, name and active status.
- Staff identity: `staff/{authUid}` with name, email and active status.
- Staff assignments: `staff/{authUid}/assignments/{assignmentId}` with `dioceseId`, optional `parishId`, role and active status.
- Every parish-owned record must contain immutable `dioceseId` and `parishId` fields. Display names are not security boundaries.

## Collections to scope
All existing parish collections must become tenant-scoped: members, families, sacraments, contributions, communities, transfers, contributionCategories and auditLogs. Staff and assignment records must be governed separately.

## Roles
- Parish roles: parish_priest, assistant_priest, secretary, parish_brother, finance_officer and read_only.
- Diocesan roles must be explicitly defined and assigned; no diocesan-wide access by default.
- A parish role never grants access to another parish automatically.
- Diocesan users see only parishes in their active assignments.
- Individual accounts only. Deactivation revokes access but retains audit history.

## Security requirements
1. Firestore rules verify signed-in UID, active staff, active assignment, role and matching parish/diocese IDs for every read and write.
2. Never trust a client-supplied parish ID alone.
3. Queries include the tenant filters required by Firestore rules.
4. Public directory data, if approved, lives in a separate allowlisted collection with only approved public fields such as display name and Jumuiya. Never expose private registers, sacramental or contribution documents.
5. Deny all access by default.
6. Emulator tests prove Parish A cannot read or write Parish B data, finance users cannot read private registers, inactive staff cannot access data, and diocesan users access only assigned parishes.

## Migration plan
1. Download and verify a Kazuramimba backup before migration.
2. Create Diocese and Kazuramimba parish identity records.
3. Assign a stable parish ID to Kazuramimba.
4. Add `dioceseId` and `parishId` to every existing parish-owned document in a controlled, repeatable migration, preserving IDs and fields.
5. Update application reads and writes to use tenant IDs.
6. Test new Firestore rules in the Firebase Emulator Suite before production deployment.
7. Test with fictional Parish A and Parish B records and accounts.
8. Only after all tests pass, schedule production migration with a verified backup and rollback plan.
9. Onboard parishes one at a time with their own IDs, assignments and administrators.

## Not included in current release
- This plan does not implement multi-parish access.
- It does not change the live Firestore schema or rules.
- It does not migrate or alter Kazuramimba records.
- It does not grant diocesan access until roles and assignments are defined and tested.

## Current operational boundary
Until implementation and security tests are complete, use this Firebase project for Kazuramimba only. Do not enter other parishes' confidential records into this instance.
