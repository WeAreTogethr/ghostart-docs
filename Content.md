# **Content Generation System Documentation**

## **Table of Contents**

1. System Overview & Business Context  
2. Technical Architecture  
3. Content Generation Workflows  
4. Post Types & Content Guidelines  
5. Scoring & Quality Systems  
6. Database Schemas  
7. State Management

## **System Overview & Business Context**

### **Project Overview**

Goal: Build a scalable, AI-powered digital product business that generates €1M+ in 12 months.

### **Technical Roadmap**

Objective: To integrate AI-personalised content generation into the existing Employee Advocacy Accelerator platform, leveraging the exercises framework to store user preferences and personalise content templates.

Key Components:

* Expanding the Exercises Framework for AI Personalisation  
* AI-Powered Content Personalisation Workflow  
* Database & Backend Adjustments (Supabase)  
* Frontend Adjustments (Next.js & Vercel)  
* Payment & Subscription Handling (Stripe API)  
* Automating AI Workflows  
* Deployment & Testing Plan

### **Development Strategy**

Core Focus Areas:

* Business Model Implementation  
* Technical Stack Integration  
* Subscription Model Development  
* E-commerce Integration

## **Technical Architecture**

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

    classDef tables fill:\#e3f2fd,stroke:\#1565c0,stroke-width:2px  
    classDef config fill:\#f3e5f5,stroke:\#4a148c,stroke-width:2px  
    classDef templates fill:\#e8f5e9,stroke:\#2e7d32,stroke-width:2px  
    classDef weights fill:\#fff3e0,stroke:\#e65100,stroke-width:2px

    class A,B,C,D tables  
    class A1,A2,A3 config  
    class B1,B2,B3 templates  
    class C1,C2,C3 weights

### **Brand Content Management**

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

    subgraph Content Types  
        G\[Templates\]  
        H\[Guidelines\]  
        I\[Assets\]  
    end

    subgraph Visibility Rules  
        J\[Organization-Wide\]  
        K\[Team-Specific\]  
        L\[Role-Based\]  
    end

    A \--\> B  
    B \--\> C  
      
    D \--\>|Full Control| A  
    D \--\>|Full Control| B  
    D \--\>|Full Control| C  
      
    E \--\>|Create & Edit| A  
    E \--\>|Review| B  
      
    F \--\>|View & Use| G  
    F \--\>|View Only| H  
    F \--\>|Download| I  
      
    G \--\> J  
    H \--\> K  
    I \--\> L

    classDef creation fill:\#e3f2fd,stroke:\#1565c0,stroke-width:2px  
    classDef access fill:\#f3e5f5,stroke:\#4a148c,stroke-width:2px  
    classDef types fill:\#e8f5e9,stroke:\#2e7d32,stroke-width:2px  
    classDef rules fill:\#fff3e0,stroke:\#e65100,stroke-width:2px

    class A,B,C creation  
    class D,E,F access  
    class G,H,I types  
    class J,K,L rules

## **Content Generation Workflows**

### **Main Generation Flow**

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
        U-\>\>S: Request Content Generation  
        S-\>\>S: Check Exercise Completion  
        alt No Exercises Done  
            S--\>\>U: Show Exercise Prompt  
            S-\>\>AI: Generate Generic Template  
        else Partial Exercises  
            S--\>\>U: Show Completion Reminder  
            S-\>\>AI: Generate Semi-Personalized  
        else All Exercises Done  
            S-\>\>AI: Generate Fully Personalized  
        end  
    end

    rect rgb(255, 240, 245\)  
        Note over U,M: AI Generation  
        alt AI Success  
            AI--\>\>S: Return Content  
            S-\>\>S: Calculate Beige Score  
            S--\>\>U: Display Content \+ Score  
        else AI Timeout  
            AI--\>\>F: Trigger Fallback  
            F-\>\>S: Use Generic Template  
            S--\>\>U: Show Limited Features Notice  
        else AI Error  
            AI--\>\>M: Log Error  
            F-\>\>S: Use Emergency Template  
            S--\>\>U: Show Error Message  
        end  
    end

    rect rgb(240, 255, 240\)  
        Note over U,M: Quality Check  
        S-\>\>S: Check Beige Score  
        alt Score \< 40  
            S--\>\>U: Show Improvement Tips  
        else Score \> 80  
            S--\>\>U: Show Success Badge  
        end  
    end

### **Exercise State Management**

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
      
    state PartialExercises {  
        \[\*\] \--\> SemiPersonalized  
        SemiPersonalized \--\> GenerateContent2: Use Partial Data  
        SemiPersonalized \--\> ContinueExercise: Complete More  
        GenerateContent2 \--\> MediumScore: Beige-ometer 40-80  
        MediumScore \--\> ContinueExercise: Suggest Completion  
    }  
      
    state AllExercises {  
        \[\*\] \--\> FullyPersonalized  
        FullyPersonalized \--\> GenerateContent3: Use Full Data  
        GenerateContent3 \--\> HighScore: Beige-ometer \> 80  
        HighScore \--\> Optimize: Suggest Optimizations  
    }

## **Post Types & Content Guidelines**

### **Default Writing Style**

The system's default writing style bridges professional and conversational tones. Content should be:

* Clear and concise while maintaining appropriate formality  
* Informative and well-structured yet approachable  
* Balanced between complexity and accessibility  
* Characterized by intentionality, thoughtfulness, and depth

### **Post Type Categories**

1. Sales Post  
2. Engagement Post  
3. Thought Leadership Post  
4. Storytelling / Personal Experience Post  
5. Event or Webinar Promotion  
6. Behind-the-Scenes / Company Culture Post  
7. Customer Success Story / Testimonial Post  
8. Industry News or Trend Discussion  
9. Poll or Survey Post  
10. Product Launch / Feature Announcement

\[Detailed descriptions for each post type are available in the system\]

## **Scoring & Quality Systems**

### **Beige-ometer Scoring System**

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

    subgraph Engagement Factors  
        C1\[Hook Strength 15%\]  
        C2\[Trending Topics 10%\]  
        C3\[Unique Insights 10%\]  
    end

    subgraph Tone Analysis  
        D1\[Voice Consistency 15%\]  
        D2\[Language Quality 10%\]  
    end

    subgraph Score Thresholds  
        E\[0-20: Beige\]  
        F\[21-40: Yellow\]  
        G\[41-60: Light Green\]  
        H\[61-80: Green\]  
        I\[81-100: Ultra Green\]  
    end

    A \--\> B  
    A \--\> C  
    A \--\> D  
    B \--\> B1  
    B \--\> B2  
    B \--\> B3  
    C \--\> C1  
    C \--\> C2  
    C \--\> C3  
    D \--\> D1  
    D \--\> D2

## **Database Schemas**

### **Content Generation Schema**

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

\-- Fallback Templates  
CREATE TABLE fallback\_templates (  
    id uuid DEFAULT uuid\_generate\_v4() PRIMARY KEY,  
    type text NOT NULL,  
    content text NOT NULL,  
    use\_case text NOT NULL,  
    is\_active boolean DEFAULT true,  
    created\_at timestamptz DEFAULT now(),  
    updated\_at timestamptz DEFAULT now()  
);

\-- Content Generation Logs  
CREATE TABLE content\_generation\_logs (  
    id uuid DEFAULT uuid\_generate\_v4() PRIMARY KEY,  
    user\_id uuid REFERENCES auth.users,  
    request\_type text NOT NULL,  
    beige\_score decimal,  
    used\_fallback boolean DEFAULT false,  
    error\_message text,  
    created\_at timestamptz DEFAULT now()  
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
    }\> {  
        // Implementation details...  
    }

    private async calculateBeigeScore(content: string, exercises: any): Promise\<BeigeScore\> {  
        // Implementation details...  
    }  
}  
