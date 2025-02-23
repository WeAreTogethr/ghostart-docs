# **Ghostart: Project Overview**

## **Table of Contents**

1. Project Overview & Purpose  
2. System Architecture  
3. Platform Structure  
4. Frontend Layout

## **Project Overview & Purpose**

Ghostart is an evolution of the Employee Advocacy Accelerator project, designed to help users create, optimize, and share LinkedIn content while tracking engagement and improving personal branding. The platform integrates:

* AI-driven exercises  
* Content creation tools  
* Brand content management  
* LinkedIn analytics  
* Training modules

### **Core Features**

* AI-Enhanced Post Writing  
* Live LinkedIn Preview  
* Content Calendar & Scheduling  
* The Beige-ometer (Post Quality Scoring)  
* AI-Powered Insights  
* Training Hub  
* Brand Content & Team Collaboration  
* Automated Email System  
* LinkedIn Analytics Dashboard  
* Monetized AI Content Packs

**Ghostart: Project Overview \[continued\]**

## **System Architecture**

### **Tech Stack**

markdown  
Copy  
\- Frontend: Next.js (App Router) hosted on Vercel  
\- Styling: TailwindCSS  
\- Backend: Supabase (PostgreSQL, RLS, Edge Functions) \+ Redis  
\- AI Services: Python backend on Render (OpenAI-powered content)  
\- External APIs: LinkedIn API, Stripe  
\- Email System: Resend API

\- Security & Monitoring: Sentry, APM

### **Content Configuration Storage**

mermaid  
Copy  
graph TD  
    subgraph Supabase Tables  
        A\[content\_scoring\_thresholds\]  
        B\[generic\_templates\]  
        C\[exercise\_weights\]  
        D\[fallback\_content\]  
    end

    subgraph Threshold Config  
        A1\[Score Ranges\]  
        A2\[Category Weights\]  
        A3\[Update History\]  
    end

    A \--\> A1  
    A \--\> A2

    A \--\> A3

## **Platform Structure**

### **1\. Post Writing (Landing Page)**

* Right Side: Live LinkedIn post preview  
* Left Side: Post drafting interface  
* Sandpit Column: Content tools and resources

### **Content Generation Flow**

mermaid  
Copy  
sequenceDiagram  
    participant U as User  
    participant S as System  
    participant AI as AI Service  
    participant F as Fallback System  
      
    rect rgb(240, 248, 255\)  
        Note over U,F: Exercise Check  
        U\-\>\>S: Request Content Generation  
        S\-\>\>S: Check Exercise Completion  
        alt No Exercises Done  
            S\--\>\>U: Show Exercise Prompt  
            S\-\>\>AI: Generate Generic Template  
        else Partial Exercises  
            S\--\>\>U: Show Completion Reminder  
            S\-\>\>AI: Generate Semi-Personalized  
        else All Exercises Done  
            S\-\>\>AI: Generate Fully Personalized  
        end

    end

### **2\. Content Calendar**

* Monthly/Weekly/Daily view  
* Drag & drop scheduling  
* Performance tracking  
* Analytics integration

### **3\. Training Hub**

* Thinkific-style learning environment  
* Video modules  
* Interactive exercises  
* Progress tracking

### **4\. AI-Powered Exercises**

* Personalization questionnaires  
* Style analysis  
* Content theme discovery  
* Audience definition

### **5\. Dashboard & Settings**

markdown  
Copy  
Dashboard Features:  
\- Training progress  
\- Exercise completion  
\- Post performance metrics  
\- 30-day engagement overview  
\- LinkedIn analytics integration

Settings:  
\- Profile customization  
\- LinkedIn integration  
\- Company & role settings

\- Email preferences

## **Implementation Details**

### **Database Schema**

sql  
Copy  
CREATE TABLE content\_scoring\_thresholds (  
    id uuid DEFAULT uuid\_generate\_v4() PRIMARY KEY,  
    name text NOT NULL,  
    min\_score integer NOT NULL,  
    max\_score integer NOT NULL,  
    color text NOT NULL,  
    created\_at timestamptz DEFAULT now(),  
    updated\_at timestamptz DEFAULT now()

);

### **State Management**

typescript  
Copy  
interface IdeasStore {  
    ideas: Idea\[\];  
    folders: Folder\[\];  
    tags: Tag\[\];  
    filters: {  
        type?: IdeaType;  
        folder?: string;  
        tags?: string\[\];  
        dateRange?: DateRange;  
    };

}

### **Monetization Model**

markdown  
Copy  
One-Time Purchases:  
\- Content Packs (€49-€199)  
\- Template Collections  
\- Custom AI Tools

Subscriptions:  
\- Basic: €49/month  
\- Pro: €99/month

\- Elite: €299/month

## **Deployment Strategy**

1. Infrastructure Setup (Week 1\)  
2. Core Features Development (Weeks 2-3)  
3. Testing & Optimization (Week 4\)  
4. Beta Launch & Feedback (Week 5\)  
5. Full Production Release (Week 6\)

## **Security & Compliance**

* Row-Level Security (RLS)  
* OAuth Authentication  
* Data Encryption  
* GDPR Compliance  
* Regular Security Audits

