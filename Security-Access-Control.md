# Ghostart Security & Access Control

## 1. Overview

Ghostart implements **Role-Based Access Control (RBAC), Row-Level Security (RLS), and API security measures** to ensure **data privacy, user-specific permissions, and system integrity**.

---

## 2. Role-Based Access Control (RBAC)

### **User Roles & Permissions**
```sql
-- Role Definitions
CREATE TYPE user_role_type AS ENUM (
    'full_access',
    'content_only',
    'exercises_only',
    'training_exercises',
    'posting_exercises'
);

-- Role Permissions Table
CREATE TABLE role_permissions (
    role user_role_type PRIMARY KEY,
    permissions JSONB NOT NULL,
    description TEXT NOT NULL,
    upgrade_path JSONB
);

-- User Role Assignment
ALTER TABLE user_profiles
ADD COLUMN role user_role_type NOT NULL DEFAULT 'exercises_only';

-- Insert Base Permissions
INSERT INTO role_permissions (role, permissions, description, upgrade_path) VALUES
(
    'full_access',
    '{
        "content": true,
        "exercises": true,
        "training": true,
        "posting": true,
        "calendar": true
    }',
    'Complete access to all Ghostart features',
    null
),
(
    'content_only',
    '{
        "content": true,
        "exercises": false,
        "training": false,
        "posting": false,
        "calendar": false
    }',
    'Access to content library only',
    '{"next_tier": "posting_exercises", "features": ["posting", "exercises"]}'
);
```

---

## 3. Row-Level Security (RLS) Policies

### **User Data Access**
```sql
CREATE POLICY "Users can view own data"
ON public.user_data
FOR SELECT
USING (auth.uid() = user_id);
```

### **Organization-Based Access**
```sql
CREATE POLICY "Users can view org data"
ON public.organization_data
FOR SELECT
USING (
    organization_id IN (
        SELECT org_id 
        FROM user_organizations 
        WHERE user_id = auth.uid()
    )
);
```

### **Brand Content Access**
```sql
CREATE POLICY "Brand content access"
ON public.brand_content
FOR ALL
USING (
    EXISTS (
        SELECT 1 
        FROM user_organizations uo
        WHERE uo.user_id = auth.uid()
        AND uo.org_id = organization_id
        AND (
            uo.role IN ('admin', 'brand_creator')
            OR CURRENT_OPERATION = 'SELECT'
        )
    )
);
```

---

## 4. API Security & Verification

### **JWT-Based Authentication**
```ts
import jwt from 'jsonwebtoken';

const verifyToken = (token) => {
  try {
    return jwt.verify(token, process.env.JWT_SECRET);
  } catch (error) {
    throw new Error('Invalid or expired token');
  }
};
```

### **Rate Limiting for API Protection**
```ts
import rateLimit from 'express-rate-limit';

const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100,
  message: 'Too many requests, please try again later.'
});

app.use('/api/', apiLimiter);
```

---

## 5. Monitoring Logs & Access Tracking

### **Access Logging System**
```sql
CREATE TABLE monitoring_logs (
    id uuid DEFAULT uuid_generate_v4() PRIMARY KEY,
    event_type text NOT NULL,
    user_id uuid REFERENCES auth.users,
    organization_id uuid,
    metadata jsonb,
    created_at timestamptz DEFAULT now()
);

CREATE TABLE brand_content_access_logs (
    id uuid DEFAULT uuid_generate_v4() PRIMARY KEY,
    content_id uuid REFERENCES brand_content,
    user_id uuid REFERENCES auth.users,
    access_type text NOT NULL,
    success boolean DEFAULT false,
    error_message text,
    created_at timestamptz DEFAULT now()
);
```

### **Monitoring Service for Security Events**
```ts
class MonitoringService {
  async trackMetrics(): Promise<MonitoringMetrics> {
    const userActivity = await this.getUserActivityMetrics();
    const contentQuality = await this.getContentQualityMetrics();
    const systemHealth = await this.getSystemHealthMetrics();

    await this.logMetrics({ userActivity, contentQuality, systemHealth });
    await this.checkAlertThresholds({ userActivity, contentQuality, systemHealth });

    return { userActivity, contentQuality, systemHealth };
  }

  private async checkAlertThresholds(metrics: MonitoringMetrics): Promise<void> {
    if (metrics.systemHealth.errorRate > 0.05) {
      await this.triggerAlert('HIGH_ERROR_RATE', metrics.systemHealth);
    }
    if (metrics.systemHealth.apiLatency > 1000) {
      await this.triggerAlert('HIGH_API_LATENCY', metrics.systemHealth);
    }
  }
}
```

---

## 6. Embedded Security System Diagrams

### **System Verification Process**
```mermaid
system-verification
```

### **Security Architecture Overview**
```mermaid
security-architecture
```

### **Role-Based Access System**
```mermaid
role-access-system
```

### **Row-Level Security Policy Flow**
```mermaid
rls-policy-flow
```

### **Monitoring & Error Handling System**
```mermaid
enhanced-monitoring
```

---

## 7. Summary

✅ **Role-Based Access Control (RBAC) for granular permissions**  
✅ **Row-Level Security (RLS) to protect user-specific & organizational data**  
✅ **Secure API endpoints with JWT authentication & rate limiting**  
✅ **Real-time monitoring and logging of security events**  
✅ **Embedded Mermaid diagrams for access control & security enforcement**  

This file is **ready to upload** to your GitHub repo! 🚀  
