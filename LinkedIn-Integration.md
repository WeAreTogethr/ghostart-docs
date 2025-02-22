# Ghostart LinkedIn Integration

## 1. Overview

The LinkedIn Integration in Ghostart allows users to **create, track, and optimize** their LinkedIn posts using:  
- **OAuth authentication** for posting and analytics retrieval  
- **Automated performance tracking** (impressions, reactions, shares)  
- **In-app & email notifications** for post updates  
- **Engagement insights & visualizations**  

---

## 2. LinkedIn API Integration

### **OAuth Authentication**
```yaml
linkedin_api:
  client_id: process.env.LINKEDIN_CLIENT_ID
  client_secret: process.env.LINKEDIN_CLIENT_SECRET
  redirect_uri: "https://ghostart.app/auth/linkedin/callback"
  scopes: "w_member_social r_organization_social w_organization_social r_analytics"
```

### **Post Creation & Tracking**
```ts
class LinkedInPostManager {
  async createPost(content: string): Promise<LinkedInPost> {
    const linkedInResponse = await this.linkedInAPI.createPost(content);

    const post = await this.db.from('linkedin_posts').insert({
      linkedin_post_id: linkedInResponse.id,
      post_url: linkedInResponse.postUrl,
      post_content: content
    });

    return post;
  }

  async updatePerformance(postId: string, metrics: PostPerformance): Promise<void> {
    await this.db.from('post_performance').insert({
      post_id: postId,
      ...metrics,
      measured_at: new Date()
    });
  }

  async getPostAnalytics(postId: string): Promise<PostPerformance[]> {
    return await this.db.from('post_performance').select('*').where('post_id', postId).orderBy('measured_at', 'asc');
  }
}
```

---

## 3. Database Schema for Post Tracking

```sql
-- Store LinkedIn posts
CREATE TABLE linkedin_posts (
    id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
    user_id UUID REFERENCES auth.users NOT NULL,
    linkedin_post_id TEXT NOT NULL,
    post_url TEXT NOT NULL,
    post_content TEXT,
    posted_at TIMESTAMPTZ DEFAULT NOW(),
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Store performance updates
CREATE TABLE post_performance (
    id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
    post_id UUID REFERENCES linkedin_posts NOT NULL,
    impressions INTEGER,
    reactions INTEGER,
    comments INTEGER,
    shares INTEGER,
    clicks INTEGER,
    measured_at TIMESTAMPTZ NOT NULL,
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Create view for latest post performance
CREATE VIEW latest_post_performance AS
SELECT 
    p.*, pp.impressions, pp.reactions, pp.comments, pp.shares, pp.clicks, pp.measured_at
FROM linkedin_posts p
LEFT JOIN LATERAL (
    SELECT * FROM post_performance WHERE post_id = p.id ORDER BY measured_at DESC LIMIT 1
) pp ON true;
```

---

## 4. Notification System for Post Updates

### **Notification Preferences**
```ts
interface NotificationPreferences {
  emailEnabled: boolean;
  inAppEnabled: boolean;
  reminderSchedule: 'default' | 'custom';
  customSchedule?: number[]; // Days after posting
  excludedTypes?: string[]; // Types of notifications to exclude
}

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
}
```

---

## 5. Analytics & Visualizations

### **LinkedIn Analytics Dashboard**
```tsx
const AnalyticsDashboard = ({ posts }) => {
  return (
    <div className="space-y-6">
      <div className="grid grid-cols-4 gap-4">
        <MetricCard title="Total Impressions" value={calculateTotalImpressions(posts)} />
        <MetricCard title="Avg. Engagement Rate" value={calculateEngagementRate(posts)} />
        <MetricCard title="Best Performing Post" value={findBestPost(posts)} />
        <MetricCard title="Weekly Growth" value={calculateGrowthRate(posts)} />
      </div>
      <div className="grid grid-cols-2 gap-4">
        <Card>
          <CardHeader><CardTitle>Performance Over Time</CardTitle></CardHeader>
          <CardContent>
            <LineChart data={prepareTimeSeriesData(posts)}>
              <XAxis dataKey="date" />
              <YAxis />
              <Tooltip />
              <Legend />
              <Line type="monotone" dataKey="impressions" stroke="#8884d8" />
              <Line type="monotone" dataKey="reactions" stroke="#82ca9d" />
            </LineChart>
          </CardContent>
        </Card>
        <Card>
          <CardHeader><CardTitle>Engagement Distribution</CardTitle></CardHeader>
          <CardContent>
            <BarChart data={prepareEngagementData(posts)}>
              <XAxis dataKey="type" />
              <YAxis />
              <Tooltip />
              <Legend />
              <Bar dataKey="value" fill="#8884d8" />
            </BarChart>
          </CardContent>
        </Card>
      </div>
    </div>
  );
};
```

---

## 6. Embedded LinkedIn Integration Diagrams

### **Performance Tracking UI**
```mermaid
performance-update-ui
```

### **Performance Tracking Flow**
```mermaid
performance-tracking-flow
```

### **LinkedIn API Retry Logic**
```mermaid
linkedin-retry-flow
```

### **LinkedIn Data Model**
```mermaid
linkedin-actual-data
```

### **Analytics Visualization System**
```mermaid
analytics-visualization
```

---

## 7. Summary

✅ **LinkedIn API OAuth authentication & post tracking**  
✅ **Automated performance updates (impressions, reactions, shares)**  
✅ **Email & in-app notifications for engagement tracking**  
✅ **Analytics dashboard with visualizations for engagement trends**  
✅ **Embedded Mermaid diagrams for tracking, retry logic & UI**  

This file is **ready to upload** to your GitHub repo! 🚀  
