# Incomplete salary transfer report

### Friday 27 March 2020

AV = Antony Vallee, PA = Pavan Avinash, WM = Wojciech Maciejak, KD = Kieran Denshi

## Summary
Correct amount recovered by Monergie, however payment back of the remaining salary was not completed. All active Everpress employees were affected (2 users).

## Timeline
- 11:37 AV noticed that 2x Everpress employees had not received the remainder of their salary
- 11:49 PA Monterail contacted for support
- 12:15 PA Modulr support contacted (via email)
- 12:20 WM found Modulr API error response in logs
- 12:57 PA Modulr support advise that the Everpress account has been disabled on our side. Moterail note that we do not have that capability. Modulr compliance team are looped in.
- 12:58 AV Modulr support update that the account was blocked by their compliance team
- 13:24 PA is advised account is unblocked
- 14:00 (est) WM re-triggers payment events
- 14:25 AV confirms payment success via Modulr console 

## Root cause
Modulr compliance had silently blocked the account.
Payments to the Everpress account (Modulr acc `A213VDZ7`) could not complete. The Modulr API response was
`[{“field”:“paymentOutMessage”,“code”:“BUSINESSRULE”,“message”:“Destination account not found.“}]`

## Resolution and recovery
Once Modulr had unblocked the affected account, the payment events that had faild were retriggered.

## Corrective and preventative measures
- Understand the cause of the account blocking - i.e. was this an error on the part of Modulr (and therefore not something we caused or could have avoided) or is there a future threat of breaching a compliance rule due to how we consume Modulr services?
- Due to circumstance, there was a delay between the discovery of the issue and the diagnosis of the cause. KD should ensure that diagnosis via production log and database query is something that can be handled in-house.
- Due to circumstance, there was a delay between the account being unblocked and the payment events retrigger. KD should ensure that event and task queue operation is something that can be handled in-house.
