# EC2 WorkWeek Scheduler (AWS Lambda)

This project automates the start and stop of EC2 instances based on Indian Standard Time (IST).  
Instances run only during **9 AM – 9 PM (Monday to Friday)** and remain **stopped during off-hours and weekends**, helping reduce AWS cost.

---

## ⭐ Features
- Auto-start EC2 instances at **9:00 AM IST** (Mon–Fri)
- Auto-stop EC2 instances at **9:00 PM IST** or on weekends
- Powered by **AWS Lambda + EventBridge**
- Tag-based instance control
- Ideal for development/testing environments

---

## 🏷 Required EC2 Tags

Add these tags to the instances you want to manage:

| Key           | Value       | Purpose |
|---------------|-------------|---------|
| Environment   | Development | Grouping |
| AutoStart     | true        | Start at 9 AM |
| AutoStop      | true        | Stop at 9 PM |

---

## 🔧 How It Works
1. Lambda runs twice daily via EventBridge schedules.
2. Converts current time to IST.
3. Depending on day/time:
   - Starts stopped instances with `AutoStart=true`
   - Stops running instances with `AutoStop=true`

---

## 🖼 Architecture
![Architecture](https://github.com/user-attachments/assets/706083f0-ce7c-4168-89d2-debb15074612)
