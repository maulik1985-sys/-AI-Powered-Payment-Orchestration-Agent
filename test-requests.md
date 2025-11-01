# Test Requests for AI Payment Agent

## 🧪 Testing the Workflow

Use these example requests to test different payment scenarios.

### Base URL
 webhook URL:
https://maulikjpatel.app.n8n.cloud/workflow/sh0v1lahVOV2Ul3g

---

## 1️⃣ Basic Payment Intent

**Request:**
```bash
curl -X POST https://maulikjpatel.app.n8n.cloud/webhook/chatbot-payment \
  -H "Content-Type: application/json" \
  -d '{
    "message": "I want to pay $50 to John Doe"
  }'
Expected Response:

{
  "status": "success",
  "message": "Payment processed",
  "paymentMethod": "stripe",
  "paymentStatus": "completed"
}
2️⃣ UPI Payment Request
Request:

curl -X POST https://maulikjpatel.app.n8n.cloud/webhook/chatbot-payment \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Pay 500 rupees via UPI to merchant@upi"
  }'
Expected Response:

{
  "status": "success",
  "message": "Payment processed",
  "paymentMethod": "upi",
  "paymentStatus": "completed"
}
3️⃣ Open Banking Payment
Request:

curl -X POST https://maulikjpatel.app.n8n.cloud/webhook/chatbot-payment \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Transfer $100 from my bank account to vendor XYZ"
  }'
Expected Response:

{
  "status": "success",
  "message": "Payment processed",
  "paymentMethod": "open_banking",
  "paymentStatus": "completed"
}
4️⃣ Non-Payment Intent (Should Not Trigger Payment)
Request:

curl -X POST https://maulikjpatel.app.n8n.cloud/webhook/chatbot-payment \
  -H "Content-Type: application/json" \
  -d '{
    "message": "What is my account balance?"
  }'
Expected Behavior:

Intent Parser should NOT classify this as "payment"
Workflow should not proceed to payment options
🔗 Webhook Testing (Stripe Confirmation)
Stripe Webhook Simulation
Endpoint:

https://maulikjpatel.app.n8n.cloud/webhook/stripe-confirmation
Request:

curl -X POST https://maulikjpatel.app.n8n.cloud/webhook/stripe-confirmation \
  -H "Content-Type: application/json" \
  -d '{
    "type": "payment_intent.succeeded",
    "data": {
      "object": {
        "id": "pi_test_123456",
        "amount": 5000,
        "currency": "usd",
        "status": "succeeded"
      }
    }
  }'
🔗 Webhook Testing (Razorpay UPI Confirmation)
Razorpay Webhook Simulation
Endpoint:

https://maulikjpatel.app.n8n.cloud/webhook/razorpay-upi-confirm
Request:

curl -X POST https://maulikjpatel.app.n8n.cloud/webhook/razorpay-upi-confirm \
  -H "Content-Type: application/json" \
  -d '{
    "event": "payment.authorized",
    "payload": {
      "payment": {
        "entity": {
          "id": "pay_test_789",
          "amount": 50000,
          "currency": "INR",
          "method": "upi",
          "vpa": "user@upi",
          "status": "authorized"
        }
      }
    }
  }'
📊 Testing Checklist
[ ] Basic payment intent with Stripe
[ ] UPI payment via Razorpay
[ ] Open Banking payment
[ ] Non-payment intent (should not trigger payment)
[ ] Stripe webhook confirmation
[ ] Razorpay webhook confirmation
[ ] Error handling (invalid amount, missing fields)
