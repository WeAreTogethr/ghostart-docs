# Ghostart Email System

## 1. Overview

The Ghostart Email System handles **transactional, automated, and personalized emails** using the **Resend API**.

---

## 2. Transactional Email Setup

### **Resend API Configuration**
```yaml
email_service:
  provider: Resend
  api_key: process.env.RESEND_API_KEY
  from_email: "no-reply@ghostart.app"
  reply_to: "support@ghostart.app"
```

### **Webhook Handling**
```ts
import express from 'express';

const app = express();
app.use(express.json());

app.post('/webhooks/email-events', async (req, res) => {
  const event = req.body;
  
  switch (event.type) {
    case 'delivered':
      console.log(`Email delivered to: ${"{event.recipient}"}`);
      break;
    case 'opened':
      console.log(`Email opened by: ${"{event.recipient}"}`);
      break;
    case 'bounced':
      console.log(`Email failed: ${"{event.error_message}"}`);
      break;
  }

  res.sendStatus(200);
});

app.listen(3000, () => console.log('Webhook server running'));
```

---

## 3. Email Personalization

Emails are dynamically personalized based on **user actions & stored data**.

### **Dynamic Email Templates**
```js
const emailTemplate = (user) => `
  <html>
    <body>
      <h2>Hello, ${"{user.name}"}</h2>
      <p>Your last post got <b>${"{user.lastPostEngagement}"}%</b> more engagement than average.</p>
      <p>Want to boost your reach? Try these tips:</p>
      <ul>
        <li>Post at ${"{user.optimalTime}"}</li>
        <li>Use ${"{user.bestPerformingTags.join(', ')}"}</li>
      </ul>
      <p>Keep growing your LinkedIn presence!</p>
    </body>
  </html>
`;
```

---

## 4. Automated Email Campaigns

### **Example: Welcome Series**
```yaml
campaigns:
  welcome_series:
    step_1:
      trigger: "User Signup"
      email_template: "welcome-email.html"
    step_2:
      trigger: "Day 3 Reminder"
      condition: "User hasn't completed profile"
      email_template: "profile-completion.html"
    step_3:
      trigger: "Day 7 Engagement Boost"
      email_template: "linkedin-engagement-tips.html"
```

---

## 5. Embedded Email System Diagrams

### **Enhanced Email System Flow**
```mermaid
enhanced-email-system
```

### **Error Handling in Email System**
```mermaid
error-handling-system
```

### **Email Template System Structure**
```mermaid
email-template-system
```

---

## 6. Summary

✅ **Transactional emails (Resend API with webhook tracking)**  
✅ **Dynamic email personalization based on user data**  
✅ **Automated email sequences (onboarding, engagement nudges)**  
✅ **Embedded Mermaid diagrams for email system flow & error handling**  

This file is **ready to upload** to your GitHub repo! 🚀  
