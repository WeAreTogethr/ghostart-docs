# **Content Generation System Documentation**

## **Table of Contents**

1. [System Overview & Architecture](https://claude.ai/chat/a6b3743f-eb6a-4732-ae22-781a55768452#system-overview--architecture)  
2. [Content Categories & Templates](https://claude.ai/chat/a6b3743f-eb6a-4732-ae22-781a55768452#content-categories--templates)  
3. [Technical Implementation](https://claude.ai/chat/a6b3743f-eb6a-4732-ae22-781a55768452#technical-implementation)  
4. [User Workflows & Experiences](https://claude.ai/chat/a6b3743f-eb6a-4732-ae22-781a55768452#user-workflows--experiences)

## **System Overview & Architecture**

### **Project Overview**

Goal: Build a scalable, AI-powered digital product business that generates €1M+ in 12 months, leveraging AI to create high-value, instantly sellable digital assets.

### **Technical Stack**

* Frontend: Next.js (App Router), TailwindCSS  
* Backend: Supabase Edge Functions (serverless)  
* Database: Supabase (PostgreSQL)  
* AI Processing: OpenAI API (GPT)  
* Payments: Stripe API  
* Authentication: Supabase Auth  
* Hosting: Vercel (frontend), Supabase (backend)

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

    subgraph Templates  
        B1\[Emergency Templates\]  
        B2\[Generic Posts\]  
        B3\[Industry Templates\]  
    end

    subgraph Weights  
        C1\[Exercise Importance\]  
        C2\[Category Impact\]  
        C3\[Minimum Requirements\]  
    end

    A \--\> A1  
    A \--\> A2  
    A \--\> A3  
    B \--\> B1  
    B \--\> B2  
    B \--\> B3  
    C \--\> C1  
    C \--\> C2

    C \--\> C3

**Content Generation System Documentation \[continued\]**

## **Content Categories & Templates**

### **Category 1: LinkedIn Thought Leadership**

#### **Purpose**

Provides structured templates for generating high-engagement LinkedIn posts with AI-personalised prompts.

#### **Available Templates**

1. AI-Personalised LinkedIn Content Pack  
   * Hook Variations (3-5 AI-Generated Options)  
   * Core Post Message  
   * Engagement Boosters  
2. LinkedIn Storytelling Framework  
   * Story Opening Hooks  
   * Turning Point Structure  
   * Key Takeaway Format  
3. AI-Generated Poll & Engagement Strategy  
   * Poll Question Types  
   * Follow-Up Templates  
   * Engagement Tactics

### **Category 2: LinkedIn Content Strategy**

#### **Main Generation Flow**

mermaid  
Copy  
sequenceDiagram  
    participant U as User  
    participant S as System  
    participant AI as AI Service  
    participant F as Fallback System  
    participant M as Monitoring

    rect rgb(240, 248, 255\)  
        Note over U,M: Exercise Check  
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

**Category 3: Sales & Business Growth Content**

#### **Key Templates**

1. AI-Customised Cold Email & LinkedIn Sales Outreach  
   * First Contact Templates  
   * Follow-up Sequences  
   * Closing CTAs  
2. Elevator Pitch & About Me Template  
   * LinkedIn Bio Generator  
   * Authority Positioning  
   * Networking Scripts  
3. Lead Magnet & Freebie Content Pack  
   * Content Ideas Generator  
   * Promotional Post Templates  
   * Lead Nurture Sequences

### **Category 4: Employee Advocacy & Employer Branding**

#### **Templates Include**

1. Employee Advocacy LinkedIn Pack  
   * Company Update Posts  
   * Success Story Formats  
   * Culture & Values Content  
2. Personal Branding Guide  
   * LinkedIn Bio Templates  
   * Thought Leadership Framework  
   * Engagement Strategies

### **Category 5: Writing Efficiency Tools**

#### **Quick Writing Templates**

1. LinkedIn Post Accelerator  
   * Instant Post Generator  
   * Hook Variations  
   * Format Options  
2. Style Guide Generator  
   * Tone Analysis  
   * Voice Customization  
   * Content Adaptation

**Technical Implementation**

### **Database Schema**

sql  
Copy  
\-- Scoring Thresholds  
CREATE TABLE content\_scoring\_thresholds (  
    id uuid DEFAULT uuid\_generate\_v4() PRIMARY KEY,  
    name text NOT NULL,  
    min\_score integer NOT NULL,  
    max\_score integer NOT NULL,  
    color text NOT NULL,  
    created\_at timestamptz DEFAULT now(),  
    updated\_at timestamptz DEFAULT now()  
);

\-- Scoring Weights  
CREATE TABLE scoring\_weights (  
    id uuid DEFAULT uuid\_generate\_v4() PRIMARY KEY,  
    category text NOT NULL,  
    weight decimal NOT NULL,  
    subcategory text,  
    subcategory\_weight decimal,  
    created\_at timestamptz DEFAULT now(),  
    updated\_at timestamptz DEFAULT now()  
);

### **Content Generation Service Interface**

typescript  
Copy  
interface BeigeScore {  
    total: number;  
    personalization: number;  
    engagement: number;  
    tone: number;  
    details: {  
        userStories: number;  
        audienceMatch: number;  
        roleRelevance: number;  
        hookStrength: number;  
        trendingTopics: number;  
        uniqueInsights: number;  
        voiceConsistency: number;  
        languageQuality: number;  
    };  
}

class ContentGenerationService {  
    async generateContent(userId: string, type: string): Promise\<{  
        content: string;  
        score: BeigeScore;  
    }\>;  
}

### **State Management**

mermaid  
Copy  
stateDiagram-v2  
    \[\*\] \--\> NoExercises: New User  
    NoExercises \--\> PartialExercises: Complete Some Exercises  
    PartialExercises \--\> AllExercises: Complete All Exercises  
      
    state NoExercises {  
        \[\*\] \--\> Generic  
        Generic \--\> GenerateContent: Use Generic Templates  
        Generic \--\> StartExercise: Start Exercise  
        GenerateContent \--\> LowScore: Beige-ometer \< 40  
        LowScore \--\> StartExercise: Prompt to Complete  
    }

**Technical Implementation**

### **Database Schema**

sql  
Copy  
\-- Scoring Thresholds  
CREATE TABLE content\_scoring\_thresholds (  
    id uuid DEFAULT uuid\_generate\_v4() PRIMARY KEY,  
    name text NOT NULL,  
    min\_score integer NOT NULL,  
    max\_score integer NOT NULL,  
    color text NOT NULL,  
    created\_at timestamptz DEFAULT now(),  
    updated\_at timestamptz DEFAULT now()  
);

\-- Scoring Weights  
CREATE TABLE scoring\_weights (  
    id uuid DEFAULT uuid\_generate\_v4() PRIMARY KEY,  
    category text NOT NULL,  
    weight decimal NOT NULL,  
    subcategory text,  
    subcategory\_weight decimal,  
    created\_at timestamptz DEFAULT now(),  
    updated\_at timestamptz DEFAULT now()  
);

### **Content Generation Service Interface**

typescript  
Copy  
interface BeigeScore {  
    total: number;  
    personalization: number;  
    engagement: number;  
    tone: number;  
    details: {  
        userStories: number;  
        audienceMatch: number;  
        roleRelevance: number;  
        hookStrength: number;  
        trendingTopics: number;  
        uniqueInsights: number;  
        voiceConsistency: number;  
        languageQuality: number;  
    };  
}

class ContentGenerationService {  
    async generateContent(userId: string, type: string): Promise\<{  
        content: string;  
        score: BeigeScore;  
    }\>;  
}

### **State Management**

mermaid  
Copy  
stateDiagram-v2  
    \[\*\] \--\> NoExercises: New User  
    NoExercises \--\> PartialExercises: Complete Some Exercises  
    PartialExercises \--\> AllExercises: Complete All Exercises  
      
    state NoExercises {  
        \[\*\] \--\> Generic  
        Generic \--\> GenerateContent: Use Generic Templates  
        Generic \--\> StartExercise: Start Exercise  
        GenerateContent \--\> LowScore: Beige-ometer \< 40  
        LowScore \--\> StartExercise: Prompt to Complete  
    }

\[Would you like me to continue with the User Workflows section?\]

Retry  
N

Yes please

Edit

# **Content Generation System Documentation \[continued\]**

## **User Workflows & Experiences**

### **Content Generation Flow**

#### **Exercise-Based Personalization**

1. **No Exercises Completed**  
   * Generic template provided  
   * Prompts to complete exercises  
   * Basic personalization  
2. **Partial Exercises Completed**  
   * Semi-personalized content  
   * Suggestions for further customization  
   * Industry-specific adaptations  
3. **All Exercises Completed**  
   * Fully personalized content  
   * Advanced customization  
   * Complete brand alignment

### **Beige Score System**

mermaid  
Copy  
graph TD  
    subgraph Scoring Components  
        A\[Total Score Calculation\]  
        B\[Personalization 40%\]  
        C\[Engagement 35%\]  
        D\[Tone 25%\]  
    end

    subgraph Personalization Metrics  
        B1\[User Stories 15%\]  
        B2\[Audience Match 15%\]  
        B3\[Role Relevance 10%\]  
    end

    A \--\> B  
    A \--\> C  
    A \--\> D  
    B \--\> B1  
    B \--\> B2  
    B \--\> B3

### **Default Writing Style**

The system's default writing style bridges professional and conversational tones. Content should be:

* Clear and concise while maintaining appropriate formality  
* Informative and well-structured yet approachable  
* Balanced between complexity and accessibility  
* Characterized by intentionality, thoughtfulness, and depth

## **API Integration & Automation**

### **OpenAI API Integration**

javascript  
Copy  
const OpenAI\_API\_ENDPOINT \= 'https://api.openai.com/v1/chat/completions';

const requestBody \= {  
    model: "gpt-4o",  
    messages: \[  
        {  
            role: "system",  
            content: "You are a LinkedIn content strategist..."  
        },  
        {  
            role: "user",  
            content: "Generate a LinkedIn post for {job\_role} in {industry}..."  
        }  
    \],  
    temperature: 0.7,  
    max\_tokens: 600  
}

### **Personalization Levels**

#### **Handling Different User States**

mermaid  
Copy  
graph TD  
    subgraph Content Creation  
        A\[Brand Manager Creates Content\]  
        B\[Content Review\]  
        C\[Content Publishing\]  
    end

    subgraph Access Levels  
        D\[Admin Access\]  
        E\[Brand Creator Access\]  
        F\[Employee Access\]  
    end

    A \--\> B  
    B \--\> C

## **Pricing & Subscription Models**

### **Subscription Tiers**

| Plan | Price (€/month) | Features |
| ----- | ----- | ----- |
| Basic | 49 | 5 AI-personalised templates |
| Pro | 99 | 10 AI-personalised templates \+ priority AI tuning |
| Elite | 299 | Unlimited templates \+ VIP support |

## **Development Timeline**

1. Phase 1: Infrastructure Setup (1-2 weeks)  
2. Phase 2: Core Features Development (2-3 weeks)  
3. Phase 3: Testing & Optimization (1-2 weeks)  
4. Phase 4: Launch & Scale (Ongoing)

## **Final Implementation Notes**

* All content generation happens server-side  
* Results cached in Supabase for quick access  
* Fallback templates available for system outages  
* Regular performance monitoring and optimization  
* Continuous improvement based on user feedback

