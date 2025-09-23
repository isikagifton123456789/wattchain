# Ngrok Setup Guide for M-Pesa STK Push Testing

## 🔧 **Step-by-Step Ngrok Setup**

### **Step 1: Install Ngrok (if not already installed)**

Download from: https://ngrok.com/download
Or install via PowerShell:
```powershell
# Using Chocolatey (if you have it)
choco install ngrok

# Or using Scoop
scoop install ngrok
```

### **Step 2: Configure Ngrok with Your API Token**

```powershell
# Navigate to your project directory
cd D:\Solarenergyconsumption

# Configure ngrok with your API token
ngrok config add-authtoken 333FNwRH7Cp4svFWQ2DuC8HrjBf_6qXXv6viYbGFy7P4tvjbr
```

### **Step 3: Start Ngrok Tunnel**

```powershell
# Start ngrok tunnel for port 5000 (where your Flask app runs)
ngrok http 5000
```

### **Step 4: Copy the HTTPS URL**

Ngrok will show output like:
```
Session Status                online
Account                       Your Account (Plan: Free)
Version                       3.x.x
Region                        United States (us)
Latency                       -
Web Interface                 http://127.0.0.1:4040
Forwarding                    https://abc123-456-789.ngrok-free.app -> http://localhost:5000
```

**Copy the HTTPS URL** (e.g., `https://abc123-456-789.ngrok-free.app`)

### **Step 5: Update Environment Variables**

Add the ngrok URL to your `.env` file:
```
PAYMENT_CALLBACK_URL=https://your-ngrok-url.ngrok-free.app/api/payment/callback
```

## 🎯 **Quick Setup Commands**

Run these commands in PowerShell:

```powershell
# 1. Configure ngrok
ngrok config add-authtoken 333FNwRH7Cp4svFWQ2DuC8HrjBf_6qXXv6viYbGFy7P4tvjbr

# 2. Start tunnel (keep this running)
ngrok http 5000 --log=stdout
```

## 🔍 **Testing the Setup**

1. **Start Ngrok** (keep terminal open)
2. **Copy the HTTPS URL** from ngrok output
3. **Update .env file** with callback URL
4. **Start Flask app** in another terminal
5. **Test STK Push** with real payments

## ⚠️ **Important Notes**

- Keep ngrok running while testing
- Use HTTPS URL (not HTTP) for M-Pesa callbacks
- Free ngrok URLs change on restart
- Test with Safaricom sandbox numbers: 254708374149