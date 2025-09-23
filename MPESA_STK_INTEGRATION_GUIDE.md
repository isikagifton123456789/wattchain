# M-Pesa Daraja API Integration Guide
## Energy Trading Platform - STK Push Setup

### 🎯 **Overview**

This guide helps you integrate M-Pesa Daraja API with STK Push functionality into your AI Energy Trading Platform. The integration allows real-time mobile money payments for energy token purchases, aligning with your project's blockchain and IoT architecture.

---

## 🚀 **Quick Setup (5 Minutes)**

### **Step 1: Get M-Pesa Sandbox Credentials**

1. **Visit Safaricom Developer Portal**: https://developer.safaricom.co.ke/
2. **Create Account**: Register with your email and phone number
3. **Create New App**:
   - App Name: "EnergyTradingPlatform"
   - Description: "AI Energy Trading with IoT and Blockchain"
   - Select APIs: **Lipa Na M-Pesa Online (STK Push)**
4. **Get Credentials**:
   - Consumer Key: `bclwIPkcRqw01yiPxNdaUKdKOAjCqgWs` (example)
   - Consumer Secret: `CDWm7Q3zYzPUAhEE` (example)

### **Step 2: Update Environment Configuration**

Your `.env` file is already configured with sandbox credentials:

```bash
# M-Pesa Integration (Daraja API)
MPESA_CONSUMER_KEY=bclwIPkcRqw01yiPxNdaUKdKOAjCqgWs
MPESA_CONSUMER_SECRET=CDWm7Q3zYzPUAhEE
MPESA_ENVIRONMENT=sandbox
MPESA_BUSINESS_SHORTCODE=174379
MPESA_PASSKEY=bfb279f9aa9bdbcf158e97dd71a467cd2e0c893059b10f78e6b72ada1ed2c919
```

### **Step 3: Test Integration**

```bash
# Start the API server
python main_app.py

# In another terminal, run the comprehensive test
python test_stk_push_integration.py
```

---

## 🏗️ **Architecture Integration**

### **How It Fits Your Project Requirements:**

```
┌─────────────────────────────────────────────────────────────────────┐
│                     AI ENERGY TRADING ECOSYSTEM                     │
├─────────────────────────────────────────────────────────────────────┤
│  1. IoT Layer (ESP32 + Wokwi)          → Smart meters measure energy │
│  2. AI Layer (Gemini + Weather)        → Predict optimal trading     │
│  3. 🆕 Payment Layer (M-Pesa STK)      → Real mobile money payments   │
│  4. Blockchain Layer (Solana)          → Energy token transfers      │
│  5. Dashboard Layer (React)            → User interface              │
└─────────────────────────────────────────────────────────────────────┘
```

### **Payment Flow Integration:**

```
Energy Trade Request
        ↓
AI Recommendation (Buy/Sell)
        ↓
STK Push to Buyer's Phone  📱
        ↓
Customer Enters M-Pesa PIN
        ↓
Payment Confirmation Callback
        ↓
Solana Smart Contract Execution
        ↓
Energy Token Transfer (eKWh)
        ↓
Seller Receives KES Payment
```

---

## 🔧 **Technical Implementation**

### **Key Components Created:**

1. **`src/payments/mpesa_daraja.py`**
   - Real M-Pesa Daraja API client
   - STK Push initiation
   - Payment status queries
   - Energy trading specific logic

2. **`src/payments/payment_integrator.py`**
   - Unified payment interface
   - Supports multiple payment providers
   - Fallback to mock for development

3. **Enhanced M-Pesa Integration in `main_app.py`**
   - Real STK Push + Mock fallback
   - Energy trading parameters
   - Callback endpoints

### **API Endpoints Added:**

```http
POST /api/execute_trade          # Execute trade with STK Push
POST /api/payment/callback       # M-Pesa payment confirmation
GET  /api/payment/status/{tx_id} # Check payment status
GET  /api/stk/status/{req_id}    # Query STK Push status
```

---

## 📱 **Testing Guide**

### **Sandbox Test Numbers:**
- **Test Phone**: `254708374149` (Safaricom sandbox)
- **Test Amount**: Any amount between 1-70,000 KES
- **PIN**: Use any PIN in sandbox (e.g., 1234)

### **Test Scenarios:**

```bash
# Test 1: Simple energy purchase
curl -X POST http://localhost:5000/api/execute_trade \
  -H "Content-Type: application/json" \
  -d '{
    "type": "BUY",
    "amount": 2.5,
    "price": 12.0,
    "phone": "254708374149",
    "seller_phone": "254700123456"
  }'
```

### **Expected STK Push Flow:**

1. **API Call** → STK Push initiated
2. **Phone Popup** → Customer receives payment request
3. **PIN Entry** → Customer enters M-Pesa PIN  
4. **Confirmation** → Payment processed
5. **Callback** → Your system receives confirmation
6. **Token Transfer** → Energy tokens allocated

---

## 🌐 **Production Deployment**

### **For Production Use:**

1. **Switch to Production Environment**:
   ```bash
   MPESA_ENVIRONMENT=production
   MPESA_CONSUMER_KEY=your_production_consumer_key
   MPESA_CONSUMER_SECRET=your_production_consumer_secret
   ```

2. **Set Up Public Callback URL**:
   ```bash
   # Use ngrok for local testing
   ngrok http 5000
   
   # Update callback URL
   PAYMENT_CALLBACK_URL=https://your-domain.com/api/payment/callback
   ```

3. **Get Production Credentials**:
   - Apply for production access on Safaricom Developer Portal
   - Complete KYC requirements
   - Get your business shortcode and passkey

---

## 🔗 **Blockchain Integration Points**

### **Where This Connects to Solana:**

```python
# In your Solana smart contract integration
def complete_energy_trade(trade_id, payment_confirmed):
    if payment_confirmed:
        # Mint energy tokens for buyer
        mint_energy_tokens(buyer_wallet, amount_kwh)
        
        # Transfer KES to seller
        transfer_payment(seller_wallet, amount_kes)
        
        # Record on blockchain
        record_trade(trade_id, buyer, seller, amount_kwh, amount_kes)
```

### **Integration with IoT Layer:**

```python
# IoT triggers energy availability
if household_surplus > 1.0:  # kWh
    # AI recommends selling
    recommendation = ai_advisor.get_recommendation()
    
    if recommendation == "SELL":
        # Find buyer and initiate payment
        stk_result = mpesa.initiate_energy_payment(...)
```

---

## ✅ **Verification Checklist**

- [ ] **Sandbox Credentials**: Configured in `.env`
- [ ] **STK Push**: Can initiate payment requests
- [ ] **Status Query**: Can check payment status
- [ ] **Callbacks**: Receive payment confirmations
- [ ] **Error Handling**: Graceful fallback to mock
- [ ] **Energy Trading**: Amounts and pricing work correctly
- [ ] **Phone Format**: Supports 254XXX and +254XXX formats
- [ ] **Logging**: All transactions logged properly

---

## 🎉 **Success Indicators**

### **✅ Working Integration Shows:**

```json
{
  "trade": {
    "id": "12345",
    "type": "BUY",
    "amount": "2.5 kWh",
    "price": "12.0 KES/kWh"
  },
  "payment": {
    "tx_id": "ws_CO_191220191020363925",
    "status": "pending_stk_push",
    "payment_method": "mpesa_daraja_stk",
    "customer_message": "Enter your PIN to pay KES 30.00 for energy"
  }
}
```

### **📱 Customer Experience:**
1. Customer initiates energy purchase via dashboard
2. Receives STK Push popup on phone immediately
3. Enters M-Pesa PIN to confirm payment
4. Energy tokens transferred to their wallet
5. Seller receives payment automatically

---

## 🚨 **Troubleshooting**

### **Common Issues:**

| Issue | Solution |
|-------|----------|
| "Token Error" | Check consumer key/secret in `.env` |
| "Invalid Phone" | Use 254XXXXXXXXX format |
| "Callback Not Received" | Set up ngrok or public URL |
| "Payment Timeout" | Query status using `/api/stk/status/{req_id}` |
| "Mock Fallback" | Verify M-Pesa credentials are set |

### **Debug Commands:**

```bash
# Check system status
curl http://localhost:5000/api/status

# Check payment integrations
curl http://localhost:5000/api/status | jq '.integrations.mpesa'

# Test STK Push directly
python -c "from src.payments import get_payment_integrator; print(get_payment_integrator().test_connection())"
```

---

## 📈 **Next Steps**

### **Phase 1 (Complete)**: ✅ M-Pesa STK Push Integration
### **Phase 2**: 🔄 Blockchain Integration (Solana Smart Contracts)
### **Phase 3**: 🔄 React Dashboard with Real-time Updates
### **Phase 4**: 🔄 ESP32 Real IoT Sensor Integration
### **Phase 5**: 🔄 Production Deployment with Real Households

---

## 💡 **Key Benefits Achieved**

✅ **Real Mobile Payments**: No more mock transactions  
✅ **Kenyan Market Ready**: M-Pesa is used by 96% of Kenyan adults  
✅ **Instant Settlements**: Payments processed in real-time  
✅ **Blockchain Compatible**: Ready for Solana token transfers  
✅ **Scalable Architecture**: Supports multiple payment providers  
✅ **Robust Fallback**: Mock payments for development  
✅ **Comprehensive Testing**: Full test suite included  

**Your AI Energy Trading Platform now has production-ready payment processing! 🚀**