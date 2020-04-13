# Incomplete pay period calculation report

### Friday 3 April 2020

AV = Antony Vallee, PA = Pavan Avinash, WM = Wojciech Maciejak, KD = Kieran Denshi, GHB = Gipsy Hill Brewery

## Summary
GHB payed March salaries late on 31/03. 

1 GHB employee withdrew advances 01/04 and 02/04 without issue. Today, employee is seeing in app:
- £0 available to withdraw
- Next payday 30 May, 57 days to go
- Pay advanced this month £0


## Timeline
- 05:44 AV noticed that April is missing from the admin dashboard reports by employer
- 05:49 AV updates that April report is now visible
- 05:49 AV updates that there have been 2 withdrawals for GHB this month
- 09:00 AV updates that although April report for GHB is visible, no withdrawals are showing
- 11:25 WM discovers issue with pay cycle calculation algorithm 
- 11:42 WM confirms this code is not used when producing statements, so this should not be affected
- 12:22 AV confirms that available amount is now visible as expected
- 16:30 AV reports GHB that users are unable to witdraw advances. Generic error message displayed in the app client. 
- 16:37 AV reports that an Everpress user made a withdrawal @ 15:02
- 16:45 AV reports feedback from another GHB employee. Reports that generic error message is displayed after the confirm (this will cost £3) step.
- 16:53 WM reports that the issue is showing up in the log **detail required**
- 17:25 AV reports employee has been able to complete withdrawal

## Root cause
The pay cycle detection algorithm was based on (date first of month) to (date last of month + 1 day). As GHB made salary payments on 03/31, the algorithm broke (03/31 + 1 day = 04/01).  

## Resolution and recovery
Fix algorithm with [hotfix to production](https://github.com/PayforeLtd/Payfore_backend/commit/ac4fdbc4b82e1ba29b8556a65d4b7331f7a3a569). This corrected the incorrect display of available to withdraw and next payday. The bad code was not detected until now as salary payments have not been made this late before.

Fix for inability to withdraw **To be documented**


## Corrective and preventative measures
- KD to organise a code audit
- KD to implement production ghost users functionality to enable testing under live conditions (with individual client settings and payment dates) without leaving traces.
- KD to create planning for clients that may make outside monthly date period salary payments (e.g. paying March salary in April)
