# Stripe

## Source

1. https://www.hellointerview.com/learn/system-design/problem-breakdowns/payment-system
2. https://newsletter.pragmaticengineer.com/p/designing-a-payment-system
3. https://www.designgurus.io/answers/detail/designing-payment-processing-system-for-e-commerce

## Area of focus

1. How do you design a system where merchant servers **never handle card details**?
2. How do you design a **reconciliation worker** to handle timeouts or unclear authorization states?
3. How will you **prevent duplicate charges** when client retries or network times out?
    1. https://youtu.be/m6DtqSb1BDM
    2. https://youtu.be/J2IcD9FZvZU
4. How do you design an **idempotent payment create API**?
5. Describe the full **state machine** for a payment: INITIATED → AUTHORIZED → CAPTURED → SETTLED
6. How do you design **retry-safe** flows end-to-end?
7. How do you resume a payment stuck in `IN_PROGRESS` state?
8. How do you design the refund flow?

## New Learning

1. Online Payment happens in 2 phase: Intend and Transaction
2. CC/DC/Payment inputs are always taken via a iframe that is owned by the js library of payment processor for compliance, this way user’s payment input never reached merchants servers.
3. DB data changes, if want to keep track of all the updates, use Change Data Capture(CDC)
4. Webhook usage with callback URL
5. Money trail in case of payment processor: Customer Card → Card Network → Issuing Bank → Stripe → Merchant’s Stripe Balance → Merchant's Bank