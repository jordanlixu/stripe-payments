# stripe-payments

## Project Overview

`stripe-payments` is a sample payment service built on **Stripe**, providing:

* **Payments (PaymentIntent)**
* **Payouts (Transfer + Payout)**
* **Webhook event handling**

Currently, it only supports **Stripe Test Mode**.

---

## Features

* 💰 **Payments (Deposit/Charge)**: Users can pay orders or top up wallets via Stripe
* 💸 **Payouts (Withdrawal/Transfer)**: Transfer funds to Connected Accounts and payout to bank accounts
* 🔔 **Webhook Handling**: Handle Stripe events like `payment_intent.succeeded` and `payout.paid`
* 🧪 **Test Mode Support**: Full flow can be simulated safely in Stripe Test Mode

---

## Suggested Project Structure

```
/controller
    PaymentController.java       # Create PaymentIntent, query status
/webhook
    StripeWebhookController.java # Handle Stripe webhook events
/service
    PaymentService.java          # Core business logic
/config
    StripeConfig.java            # Stripe API key configuration
```

---

## Environment Configuration

In `application.yml`:

```yaml
stripe:
  secret-key: sk_test_xxx      # Stripe Test Secret Key
  webhook-secret: whsec_xxx    # Stripe Webhook Secret
```

> Note: In Test Mode, no real charges will occur.

---

## Usage

1. Start the Spring Boot application
2. Create a payment (PaymentIntent):

   * POST `/api/payments/create-intent`
   * Returns `clientSecret` for frontend confirmation
3. Receive payment events:

   * Stripe posts to `/api/payments/webhook`
   * Parse events and update orders/balances
4. Payout (Withdrawal):

   * Create a test Connected Account
   * Transfer platform balance to Connected Account
   * Payout to test bank account (in Test Mode, status will show as paid; no real funds are transferred)

---

## Notes

* Test Mode is free and allows full simulation of deposits and payouts
* Live Mode will process real payments and Stripe fees apply
* Future work: abstract `PaymentGateway` for multi-vendor support
