# Ghostart System Architecture

## 1. Core Database Schema
Ghostart's database is built on **Supabase (PostgreSQL)** with **Row-Level Security (RLS)** policies to ensure data isolation.

### **Organizations Table**
```sql
CREATE TABLE organizations (
    id uuid DEFAULT uuid_generate_v4() PRIMARY KEY,
    name text NOT NULL,
    settings jsonb DEFAULT '{}'::jsonb,
    created_at timestamptz DEFAULT now()
);
```

### **User Organizations (Junction Table)**
```sql
CREATE TABLE user_organizations (
    user_id uuid REFERENCES auth.users,
    organization_id uuid REFERENCES organizations,
    role text NOT NULL CHECK (role IN ('admin', 'brand_creator', 'employee')),
    PRIMARY KEY (user_id, organization_id)
);
```

### **Exercise Completion Table**
```sql
CREATE TABLE exercise_completion (
    user_id uuid REFERENCES auth.users,
    exercise_type text NOT NULL,
    completion_status text NOT NULL,
    data jsonb,
    completed_at timestamptz,
    PRIMARY KEY (user_id, exercise_type)
);
```

### **Example RLS Policies**
```sql
CREATE POLICY "org_member_read_access" ON organizations
    FOR SELECT
    USING (
        EXISTS (
            SELECT 1 FROM user_organizations
            WHERE user_id = auth.uid()
            AND organization_id = id
        )
    );

CREATE POLICY "admin_full_access" ON organizations
    FOR ALL
    USING (
        EXISTS (
            SELECT 1 FROM user_organizations
            WHERE user_id = auth.uid()
            AND organization_id = id
            AND role = 'admin'
        )
    );
```

---

## 2. System Architecture

### **Frontend (Vercel - Next.js App Router)**
- **Protected & Public Routes**
- **Edge Caching** for Static Content & API Responses
- **Live Post Preview System** mimicking LinkedIn UI

### **Backend (Supabase)**
- **Authentication System (OAuth + JWT)**
- **PostgreSQL Database**
- **Row-Level Security (RLS)**
- **Automated Database Updates via AI Tools**

### **Caching Layer (Redis)**
- **User Data Cache** (profile information, preferences, exercise responses)
- **Content Cache** (generated posts, templates, training materials)
- **Analytics Cache** (performance metrics, engagement stats, system health data)

### **Queue System**
- **Message Queue** for content generation tasks, analytics processing, email notifications
- **Task Scheduler** for scheduled posts, data aggregation, cache invalidation

### **AI Services (Render - Python)**
- **Background Workers**
- **Retry Mechanisms & Fallback Systems**
- **AI-powered Content Generation via OpenAI API**

---

## 3. External Services Integration
- **LinkedIn API** → OAuth-based login, Post-sharing & analytics retrieval
- **Stripe API** → Payment processing for AI-generated content
- **Resend API** → Email automation for reminders, reports, and alerts

---

## 4. Security & Performance
- **JWT Authentication & OAuth Token Storage**
- **API Key Management & Rate Limiting**
- **DDoS Protection via Cloudflare**
- **SSL/TLS Encryption**
- **Database Backups & Disaster Recovery Plans**

---

## 5. Embedded Architecture Diagrams

### **Email System Architecture**
```mermaid
graph TD
    subgraph Frontend
        A[User Settings Page]
        B[Email Preferences UI]
    end

    subgraph Email System
        C[Resend API]
        D[Email Templates]
        E[Email Queue Manager]
    end

    subgraph Queue Layer
        F[Redis Queue]
        G[Scheduled Jobs]
    end

    subgraph Database
        H[Supabase DB]
        I[Email Logs]
        J[User Preferences]
    end

    subgraph Background Tasks
        K[Next.js API Routes]
        L[Email Workers]
    end

    A --> B
    B --> J
    K --> E
    E --> C
    E --> D
    E --> F
    F --> L
    L --> C
    L --> I
    L --> J
    G --> F
    
    classDef frontend fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    classDef email fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef queue fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    classDef db fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef tasks fill:#fce4ec,stroke:#880e4f,stroke-width:2px
    
    class A,B frontend
    class C,D,E email
    class F,G queue
    class H,I,J db
    class K,L tasks

```

### **Error Handling System**
```mermaid
sequenceDiagram
    participant U as User
    participant S as System
    participant L as LinkedIn API
    participant E as Error Handler
    participant M as Monitoring
    
    rect rgb(240, 248, 255)
        Note over U,M: API Errors
        S->>L: API Request
        L-->>S: Error Response
        S->>E: Handle Error
        E->>E: Classify Error
        E->>S: Retry Strategy
        E->>M: Log Error
    end
    
    rect rgb(255, 240, 245)
        Note over U,M: System Errors
        S->>S: Internal Operation
        S->>E: Operation Failed
        E->>M: Alert Admin
        E->>U: User Friendly Message
    end
    
    rect rgb(240, 255, 240)
        Note over U,M: Recovery
        E->>S: Retry Operation
        S->>L: Retry Request
        L-->>S: Success
        S-->>U: Operation Complete
    end

```

### **Ghostart Data Flows**
```mermaid
sequenceDiagram
    participant U as User
    participant F as Frontend
    participant C as Redis Cache
    participant Q as Message Queue
    participant S as Supabase
    participant AI as AI Service
    participant M as Monitoring
    
    rect rgb(240, 248, 255)
        Note over U,M: Content Generation Flow
        U->>F: Request Content
        F->>C: Check Cache
        alt Cache Hit
            C-->>F: Return Cached Content
        else Cache Miss
            F->>Q: Queue Generation Task
            Q->>AI: Process Task
            AI->>S: Store Result
            AI->>C: Cache Result
            C-->>F: Return Content
        end
    end
    
    rect rgb(255, 240, 245)
        Note over U,M: Error Handling Flow
        AI->>M: Log Error
        M->>Q: Trigger Retry
        Q->>AI: Retry Task
        alt Retry Success
            AI-->>F: Return Content
        else Retry Failed
            AI->>F: Return Fallback
            M->>S: Log Incident
        end
    end
    
    rect rgb(240, 255, 240)
        Note over U,M: Performance Monitoring
        F->>M: Log User Action
        AI->>M: Log Processing Time
        S->>M: Log DB Metrics
        M->>S: Store Analytics
    end

```

### **Ghostart Monitoring System**
```mermaid
graph TD
    subgraph User Actions
        A1[Page Views]
        A2[API Calls]
        A3[Content Generation]
    end

    subgraph APM Monitoring
        B1[Response Times]
        B2[Error Rates]
        B3[Resource Usage]
    end

    subgraph Error Tracking
        C1[Frontend Errors]
        C2[Backend Errors]
        C3[API Errors]
    end

    subgraph Analytics Dashboard
        D1[User Metrics]
        D2[System Health]
        D3[Performance Graphs]
    end

    subgraph Alerts
        E1[Error Alerts]
        E2[Performance Alerts]
        E3[Usage Alerts]
    end

    A1 --> B1
    A2 --> B1
    A3 --> B1
    
    B1 --> D2
    B2 --> D2
    B3 --> D2
    
    C1 --> E1
    C2 --> E1
    C3 --> E1
    
    B1 --> E2
    B2 --> E2
    B3 --> E2
    
    D1 --> E3
    D2 --> E3
    
    classDef actions fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    classDef apm fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef errors fill:#ffebee,stroke:#b71c1c,stroke-width:2px
    classDef dashboard fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    classDef alerts fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class A1,A2,A3 actions
    class B1,B2,B3 apm
    class C1,C2,C3 errors
    class D1,D2,D3 dashboard
    class E1,E2,E3 alerts

```

### **Security Architecture**
```mermaid
graph TD
    subgraph Authentication
        A[OAuth Flow]
        B[Token Management]
        C[RLS Policies]
    end

    subgraph Token Storage
        D[HTTP-Only Cookies]
        E[Encrypted DB Fields]
        F[Refresh Tokens]
    end

    subgraph RLS Implementation
        G[User Data Policies]
        H[Brand Content Policies]
        I[Content Access Rules]
    end

    subgraph Organization Control
        J[Org-Based Access]
        K[Role Permissions]
        L[Brand Manager Rights]
    end

    A --> D
    A --> E
    B --> F
    C --> G
    C --> H
    C --> I
    G --> J
    H --> K
    I --> L

    classDef auth fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    classDef storage fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef rls fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    classDef org fill:#fff3e0,stroke:#e65100,stroke-width:2px

    class A,B,C auth
    class D,E,F storage
    class G,H,I rls
    class J,K,L org

```
