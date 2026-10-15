**Model:** Claude Sonnet

## How I set the prompt up

I split the job into three stages (extract, map rules to risks, generate threats) because the model gives much better threats when it has first written down what the system actually does. Each threat has to point at a feature and a step from the extraction, so it can't drift into general advice.

The description never covers everything, so the extraction stage also asks for missing or unclear controls (how webhooks are checked, how balances are updated, how "once per user" is enforced). The `assumption` field on every threat makes the model say what it is guessing instead of passing it off as fact. I also told it to use a category only when it really fits. Without that it tends to stretch a threat to hit a category.

## Keeping threats specific and non-duplicate

- Every attack scenario has to use the system's own limits, roles and amounts.
- There's a "could this be pasted into another system?" test, and generic bugs like SQLi are excluded unless they break a business rule.
- Duplicates are judged by root cause, not by wording, and the model compares every pair before it answers.
- Mitigations must be concrete (a unique constraint, a row lock, signature verification), not "validate input".

## How I'd evaluate it

I'd write 15 to 20 scenarios of different types (ride hailing, checkout, SaaS billing, lending) and run each prompt version a few times. Automatic checks cover valid JSON, exactly 10 threats, at least 6 categories, allowed category names and features that exist in the extraction. Then I'd read a sample by hand and score specificity, whether the threat is believable, severity calibration, and how honest the assumptions are. Running the same scenario repeatedly shows how much the threats overlap between runs. Scoring every prompt change against the same set tells me whether an edit helped.

## What I saw when I ran it

I ran the prompt twice in separate chats. Both outputs were valid JSON with exactly 10 threats and at least 9 categories. About 8 of 10 threats matched by root cause, mostly the money-handling ones: webhook forgery and replay, the daily limit race, negative amounts, refund abuse and PIN brute force. The differences were at the edges. One run had a KYC tier bypass and the other a statement data leak, and the same threat sometimes got a different category or severity. To make it more stable I would lower the temperature and add a required coverage check, for example making sure confidentiality and KYC each get considered.
