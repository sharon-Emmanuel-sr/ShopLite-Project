# ShopLite: Environment-Aware Builds & Secure Secrets Management

## 📌 Project Overview
This project demonstrates the implementation of environment segregation and secure secrets management in a modern CI/CD pipeline. Using a simulated e-commerce platform, **ShopLite**, we analyze how to prevent critical production failures caused by credential mismanagement.

---

## 🛡️ Analysis: The "ShopLite" Case Study
### What Went Wrong?
In the case study, a developer accidentally used **staging database credentials** in the **production environment**. This happened because:
1. **Manual Configuration:** Credentials were likely managed manually or through a single unprotected `.env` file.
2. **Lack of Segregation:** There was no automated "wall" preventing staging secrets from being injected into the production build.
3. **Data Corruption:** This led to test data overwriting live customer data, causing downtime and a loss of customer trust.

### The Solution
By implementing **Environment-Aware Builds** and **Secure Secrets Management**, we ensure that:
* Production credentials are never handled by developers locally.
* The CI/CD pipeline (GitHub Actions) automatically selects the correct credentials based on the deployment target (Main/Production vs. Development/Staging).

---

## 🚀 Environment Segregation
In this project, we have separated the environments into three distinct stages:

| Environment | Purpose | Configuration File |
| :--- | :--- | :--- |
| **Development** | Local coding and unit testing. | `.env.development` |
| **Staging** | Pre-production testing with mock data. | `.env.staging` |
| **Production** | Live environment serving real customers. | `.env.production` (Secrets Injected) |

### Why is this essential?
Segregation creates a **blast zone**. If a database is accidentally wiped in "Development," the "Production" data remains untouched. It ensures stability, security, and performance isolation.

---

## 🔐 Secure Secrets Management
Instead of hardcoding sensitive information, this project utilizes:
1. **GitHub Secrets:** All production API keys and Database URLs are stored as encrypted secrets in the GitHub repository settings.
2. **Environment Masking:** Secrets are masked in GitHub Action logs (appearing as `***`) to prevent accidental exposure.
3. **Injected Variables:** Secrets are injected at runtime using the following logic:
   ```javascript
   require('dotenv').config({
     path: process.env.NODE_ENV === 'production' ? '.env.production' : '.env.development'
   });