# 🚀 RedTube Account Automation – QA Portfolio Project

This project is part of my **QA Automation Engineering portfolio**.  
It showcases my ability to build **complex, end-to-end automated flows** using **Playwright**, including:

- UI automation  
- Email verification  
- Modal handling  
- Stress testing  
- Captcha observation  
- Repeated workflow execution  

The automation interacts with **https://www.redtube.com/** because its multi-step registration flow provides a realistic example of a dynamic, real-world UI.

⚠️ **Disclaimer:**  
This project is for **portfolio and educational purposes only**.  
It is **not** intended to bypass security, create real accounts, or violate Terms of Service.

---

## 🧭 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [QA Skills Demonstrated](#qa-skills-demonstrated)
- [Project Structure](#project-structure)
- [How It Works](#how-it-works)
- [Example Code Snippet](#example-code-snippet)
- [Installation](#installation)
- [Running the Script](#running-the-script)
- [Ethical Notice](#ethical-notice)
- [Future Enhancements](#future-enhancements)

---

## 📌 Overview

This script automates the entire **signup + verification + login** workflow on RedTube.com.  
Key capabilities:

- Fills forms  
- Handles modals and overlays  
- Retrieves real email verification codes  
- Inputs 6-digit codes into UI fields  
- Completes TOS prompts  
- Takes final screenshots  
- Repeats workflow for stress testing (up to 1000 iterations)

---

## ✨ Features

- **Automated sign-up workflow**  
- **Email verification handling**  
- **TOS modal automation**  
- **Sign-in workflow**  
- **Screenshot capture**  
- **Stress-testing loop**  
- **Captcha trigger observation**  
- **Async Playwright browser management**

---

## 🧪 QA Skills Demonstrated

- End-to-end UI automation  
- Testing verification-based flows  
- Dynamic locator handling  
- Working with modal logic  
- Error recovery + exception handling  
- High-volume workflow execution  
- Asynchronous programming  
- Real-world debugging using screenshots  
- Selector strategy for unstable UIs  
- Handling repeated executions until captcha triggers

---

## 📂 Project Structure

/project

├─ browser_manager.py

├─ utils.py

├─ main.py

├─ screenshots/

└─ README.md

---

## 📝 How It Works

### **1. Sign-Up Flow**
- Navigate to Pornhub  
- Handle age and cookie banners  
- Open sign-up dialog  
- Fill email and password  
- Wait for email verification modal  
- Retrieve verification code via `get_verification_code()`  
- Input digits into UI fields  
- Handle "Date of Birth" modal if shown  
- Accept "Terms of Service" modal  
- Confirm success through profile menu  

### **2. Sign-In Flow**
- Navigate to homepage  
- Handle modals  
- Enter credentials  
- Complete DOB/TOS if shown  
- Confirm login  

### **3. Runner Loop**
Loops the automation up to **1000 times**:

```
async def runner():
    for _ in range(1000):
        await main()
```

## 💻 Example Code Snippet

```
async def sign_up(page, username, password):
    await page.goto("https://www.redtube.com/register")

    # Fill credentials
    await page.locator("input[name='email']").type(username, delay=100)
    await page.locator("input[name='password']").type(password, delay=100)
    await page.locator("#signup-btn").click()

    # Handle TOS
    tos_btn = page.locator("#tos_got_it_btn")
    try:
        await tos_btn.wait_for(state="visible", timeout=10000)
        await tos_btn.click()
    except Exception:
        print("TOS not found or already accepted.")

    # Email verification
    verification_wrapper = page.locator(".email-verification-form")
    await verification_wrapper.wait_for(state="visible", timeout=15000)
    code = await get_verification_code(username.split("@")[0])
    await page.evaluate(f"""
        (() => {{
            const code = '{code}';
            for (let i = 0; i < code.length; i++) {{
                const input = document.querySelector('#emailCode_' + (i+1));
                if (input) {{
                    input.value = code[i];
                    input.dispatchEvent(new Event('input', {{ bubbles: true }}));
                    input.dispatchEvent(new Event('change', {{ bubbles: true }}));
                }}
            }}
        }})()
    """)
    return username
```

### ⚙️ Installation

```
pip install playwright
playwright install
```

### ▶️ Running the Script

```
python main.py
```

Screenshots will be available in:

```
/screenshots/<username>_screenshot.png
```

## ⚠️ Ethical Notice

This repository is created exclusively to showcase QA automation skills.
All accounts used were test accounts, and no real user data is involved.
Do not use this automation to bypass security or create real accounts.

## 🚀 Future Enhancements

- Structured logging and JSON output
- HTML/Allure test reporting
- Selector abstraction for maintainability
- Parallel execution of workflows
- Integration with multiple mailbox providers
- UI flow visualization and reporting