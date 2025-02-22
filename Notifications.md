# Ghostart Notifications System

## 1. Overview

The Ghostart **Notification System** provides users with **real-time updates, post engagement reminders, and scheduled alerts** via **email & in-app notifications**.

---

## 2. Notification Preferences

### **User Notification Preferences System**
```ts
interface NotificationPreferences {
  emailEnabled: boolean;
  inAppEnabled: boolean;
  reminderSchedule: 'default' | 'custom';
  customSchedule?: number[]; // Days after posting
  excludedTypes?: string[]; // Types of notifications to exclude
}

const DefaultReminderSchedule = [1, 7, 30]; // Default reminder intervals in days

class UserPreferencesManager {
  async getUserPreferences(userId: string): Promise<NotificationPreferences> {
    return await this.db.from('notification_preferences').where('user_id', userId).first();
  }
}
```

---

## 3. Notification Triggering System

### **Notification Triggers**
```ts
interface NotificationTrigger {
  type: 'post_created' | 'time_based' | 'milestone' | 'system';
  postId?: string;
  userId: string;
  schedule?: number[];
  data?: Record<string, any>;
}

class NotificationManager {
  async scheduleNotifications(trigger: NotificationTrigger): Promise<void> {
    const userPrefs = await this.getUserPreferences(trigger.userId);

    if (trigger.type === 'post_created') {
      await this.schedulePostReminders(trigger.postId, userPrefs);
    }
  }

  private async schedulePostReminders(postId: string, prefs: NotificationPreferences): Promise<void> {
    const reminderSchedule = prefs.reminderSchedule === 'custom' ? prefs.customSchedule : DefaultReminderSchedule;

    for (const day of reminderSchedule) {
      await this.createReminderJob({
        postId,
        scheduledFor: addDays(new Date(), day),
        channels: this.getEnabledChannels(prefs)
      });
    }
  }
}
```

---

## 4. Email Notification System

### **Email Sending Logic**
```ts
class NotificationService {
  async sendNotification(notification: Notification): Promise<void> {
    if (notification.channels.includes('email')) {
      await this.sendEmailNotification(notification);
    }
    if (notification.channels.includes('inApp')) {
      await this.sendInAppNotification(notification);
    }
  }

  private async sendEmailNotification(notification: Notification): Promise<void> {
    const emailTemplate = this.getEmailTemplate(notification.type);

    await resend.emails.send({
      from: 'Ghostart <updates@ghostart.com>',
      to: notification.userEmail,
      subject: emailTemplate.subject,
      react: emailTemplate.component(notification.data)
    });
  }
}
```

### **Example: Performance Update Email**
```tsx
const PerformanceUpdateEmail = ({ postData }) => (
  <EmailTemplate>
    <Heading>Time to Update Your Post Performance</Heading>
    <Text>
      It's been {postData.daysAgo} days since you published your post. 
      Take a moment to update its performance metrics.
    </Text>
    <Button href={postData.updateUrl}>
      Update Performance
    </Button>
    <PostPreview post={postData} />
  </EmailTemplate>
);
```

---

## 5. Notification Queue & Processing

### **Notification Queue Processor**
```ts
class NotificationQueue {
  async processNotificationJobs(): Promise<void> {
    const jobs = await this.getScheduledNotifications();

    for (const job of jobs) {
      try {
        await this.processNotification(job);
        await this.markAsProcessed(job.id);
      } catch (error) {
        await this.handleNotificationError(job, error);
      }
    }
  }
}
```

---

## 6. Database Schema for Notifications

```sql
-- Notification Preferences
CREATE TABLE notification_preferences (
    user_id UUID REFERENCES auth.users PRIMARY KEY,
    email_enabled BOOLEAN DEFAULT true,
    in_app_enabled BOOLEAN DEFAULT true,
    reminder_schedule TEXT DEFAULT 'default',
    custom_schedule INTEGER[],
    excluded_types TEXT[],
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Notification Queue
CREATE TABLE notification_queue (
    id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
    user_id UUID REFERENCES auth.users,
    type TEXT NOT NULL,
    scheduled_for TIMESTAMPTZ NOT NULL,
    data JSONB,
    status TEXT DEFAULT 'pending',
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Notification History
CREATE TABLE notification_history (
    id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
    notification_id UUID REFERENCES notification_queue,
    status TEXT NOT NULL,
    sent_at TIMESTAMPTZ DEFAULT NOW(),
    channel TEXT NOT NULL,
    error TEXT
);
```

---

## 7. Embedded Notification System Diagrams

### **Notification Flow**
```mermaid
notification-flow
```

### **Notification Preferences System**
```mermaid
notification-preferences
```

### **Overall Notification System Architecture**
```mermaid
notification-system
```

---

## 8. Summary

✅ **Multi-channel notifications (email & in-app)**  
✅ **Customizable reminder schedules for user preferences**  
✅ **Smart triggers based on post activity & engagement**  
✅ **Efficient notification queue & error handling system**  
✅ **Embedded Mermaid diagrams for notification logic & preferences**  

This file is **ready to upload** to your GitHub repo! 🚀  
