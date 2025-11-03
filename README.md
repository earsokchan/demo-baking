🏦 Bakong API KHQR Integration
This project demonstrates how to generate KHQR codes and check payment statuses using the Bakong API.
It’s a simple Node.js-based demo for creating KHQR payment QR codes and verifying transactions automatically.
🚀 Features
✅ Generate KHQR code dynamically for any amount
🔁 Auto-check payment status (loop until paid or expired)
🔒 Secure MD5 hash for transaction validation
🕒 QR expiration time tracking
⚙️ Step 1 — Verify Your Bakong Account
Before you can use the Bakong API, ensure you have:
A Bakong ID (e.g., ear_sokchan2@aclb)
API credentials from Bakong (developer access)
Sandbox or production environment set up
You can verify your account at:
👉 https://bakong.nbc.gov.kh
🪙 Step 2 — Register Access Token
To interact with the Bakong API, you need an access token.
You can obtain it by registering with Bakong’s API service.
Save your token in .env:
BAKONG_ACCESS_TOKEN=your_token_here
BAKONG_DEV_BASE_API_URL=https://api-sandbox.bakong.nbc.gov.kh
BAKONG_PROD_BASE_API_URL=https://api.bakong.nbc.gov.kh
💳 Step 3 — Generate KHQR Code
Endpoint
POST /api/generate-khqr
Example Request
{
  "amount": 100,
  "currency": "KHR",
  "orderId": "ORDER_1762179271536"
}
Example Log Output
/api/generate-khqr called with: { amount: 100, currency: 'KHR', orderId: 'ORDER_1762179271536' }

✅ KHQR Generated: {
  orderId: 'ORDER_1762179271536',
  amount: 100,
  currency: 'KHR',
  qr: '00020101021229210017ear_sokchan2@aclb52045999530311654031005802KH5907Chandev6010Phnom Penh62520119ORDER_17621792715360307Chandev0714Online Payment9934001317621792715650113176217957156363040781',
  md5: '555d01de7baf6f6b6c8ac2818b2ed119',
  expiresAt: 1762179571563
}
⏳ Step 4 — Check Payment Status
Endpoint
POST /api/check-payment
Example Request
{
  "md5": "555d01de7baf6f6b6c8ac2818b2ed119"
}
Response: Pending
💰 Payment Check Result: {
  paid: false,
  hash: '555d01de7baf6f6b6c8ac2818b2ed119',
  amount: 0,
  from: 'unknown',
  to: 'unknown',
  timestamp: '11/3/2025, 9:14:34 PM'
}
⏳ Payment pending, retrying...
Response: Paid
💰 Payment Check Result: {
  paid: true,
  hash: '5b792fa172db22c8fc7752c97ceecc057c9fe8819ec81c2909c42aeab9959f9f',
  amount: 100,
  from: 'abaakhppxxx@abaa',
  to: 'ear_sokchan2@aclb',
  timestamp: '11/3/2025, 9:15:07 PM'
}
✅ Payment successful!
🧩 Folder Structure
bakong-khqr-demo/
├── .env
├── server.js
├── package.json
├── routes/
│   ├── khqr.js
├── public/
│   ├── index.html
🖥️ Run the Project
Install dependencies:
npm install
Run in development:
npm run dev
Start server:
node server.js
Then open:
👉 http://localhost:3000
📚 Notes
The QR code expires after a short period (typically 5 minutes).
Always verify the MD5 hash matches your stored transaction.
You can loop the /api/check-payment endpoint every few seconds to check if payment is complete.
🧑‍💻 Example Frontend (HTML)
<!DOCTYPE html>
<html>
  <head>
    <title>Bakong KHQR Demo</title>
  </head>
  <body>
    <h2>Bakong KHQR Payment</h2>
    <button onclick="generateQR()">Generate QR</button>
    <pre id="result"></pre>

    <script>
      async function generateQR() {
        const res = await fetch('/api/generate-khqr', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ amount: 100, currency: 'KHR', orderId: 'ORDER_' + Date.now() }),
        });
        const data = await res.json();
        document.getElementById('result').textContent = JSON.stringify(data, null, 2);
      }
    </script>
  </body>
</html>
🏁 Conclusion
With this setup, you can:
Generate KHQR payment codes instantly
Track payment status in real time
Integrate seamlessly with your Node.js or Express backend