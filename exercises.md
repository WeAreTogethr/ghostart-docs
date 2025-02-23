# **Exercises Documentation**

## **Table of Contents**

1. Overview  
2. Exercise Categories  
3. Dependency Map  
4. Exercise Details  
5. Technical Integration  
6. Templates & Formatting

## **Overview**

This system provides a series of exercises using OpenAI API to help users improve their LinkedIn presence. Exercise results are stored in Supabase to:

* Contribute to user's 'beige-ometer'  
* Personalize post creation  
* Populate target audience dropdown  
* Guide content strategy

## **Exercise Categories**

1. **Overcoming Barriers to Writing**  
   * Why am I doing this? (7 Levels Deep)  
2. **Deciding What to Write About**  
   * Who do you work for?  
   * Time travel: everyone is an expert  
   * Only I (find your thing)  
   * Personal brand vs company vision  
   * Finding your niche  
   * Discover your 5 LinkedIn content pillars

# **LinkedIn Exercises Documentation \[continued\]**

## **Dependency Map**

mermaid  
Copy  
graph TD  
    A\[Why: 7 Levels Deep\] \--\> B\[Finding Your Niche\]  
    C\[Time Travel Exercise\] \--\> B  
    D\[Only I Exercise\] \--\> B  
    E\[Who Do You Work For\] \--\> F\[Personal Brand vs Company\]  
    G\[Understanding Audience Basic\] \--\> H\[Understanding Audience Advanced\]  
    I\[Content Theme Helper\] \--\> J\[Content Pillars\]  
      
    classDef required fill:\#f9f,stroke:\#333,stroke-width:2px

    class B,F,H required

## **Exercise Details**

### **1\. Why: 7 Levels Deep**

Purpose: Uncover users' true LinkedIn motivation through 7 progressive "why" questions.  
Implementation:  
markdown  
Copy  
1\. Initial Question: "Why do I want to be on LinkedIn?"  
2\. Follow-up Analysis after each response  
3\. Maximum 7 "why" questions  
4\. Final Output:  
   \- Summary paragraph

   \- One-line motivational statement

Storage Requirements:  
sql  
Copy  
CREATE TABLE user\_motivation (  
    user\_id UUID REFERENCES auth.users,  
    why\_summary TEXT,  
    one\_liner TEXT,  
    created\_at TIMESTAMPTZ DEFAULT NOW()

);

**2\. Time Travel Exercise**

**Purpose:** Help users uncover expertise and experiences from past 2 years for LinkedIn content.

**Implementation:**

markdown  
Copy  
6 Sequential Questions:  
1\. How have you changed over these 2 years?  
2\. What do I do in my job \- how has my expertise grown?  
3\. What skills have I built?  
4\. What struggles have I overcome?  
5\. What topics have I learnt about?  
6\. What's my work backstory \- how have I changed?

After each response:  
\- Analyze learning points  
\- Identify potential LinkedIn content angles  
\- Provide insight into shareable learnings

### **3\. Only I (Find Your Thing)**

**Purpose:** Identify unique perspectives for LinkedIn content.

**Questions Flow:**

markdown  
Copy  
1\. What can only you do?  
2\. What do you know about best?  
3\. What are you the best at?  
4\. What gives you a different perspective?  
5\. What advice do colleagues seek from you?  
6\. Where do you have unique expertise?

Output:  
\- Unique angles for content creation  
\- Personal/professional crossover points  
\- Content differentiation opportunities

### **4\. Writing Style Guide**

**Purpose:** Establish user's unique LinkedIn writing voice.

**Components:**

markdown  
Copy  
1\. Style Analysis:  
   \- Tone preferences  
   \- Vocabulary choices  
   \- Structure preferences  
   \- Format preferences

2\. Output Format:  
   \- Style Summary  
   \- Key Writing Traits  
   \- Prompt Snippet for AI generation

3\. Technical Storage:  
   \- Style preferences in Supabase  
   \- Integration with post writing

### **5\. Understanding Your Audience (Who/Why Framework)**

**Purpose:** Help users identify their LinkedIn audience and content objectives.

**Implementation:**

markdown  
Copy  
Initial Data Collection:  
\- Job role  
\- Company/expertise area  
\- Primary target audience

Format Output:  
Who: \[Target Audience 1\]  
Why: \[Up to 3 objectives\]  
\- Objective 1  
\- Objective 2  
\- Objective 3

Storage:  
\- Audience profiles  
\- Content objectives  
\- Engagement goals

### **6\. Content Theme Helper**

**Purpose:** Generate personalized content themes based on user's role and expertise.

**Process Flow:**

markdown  
Copy  
1\. Information Gathering:  
   \- Job role  
   \- Company details  
   \- Expertise areas  
   \- Experience level  
   \- Personal interests  
   \- Social media goals

2\. Theme Development:  
   \- 3 top-level content themes  
   \- Creative mixing of interests/expertise  
   \- Example posts for each theme

3\. Output Format:  
Theme:  
Overview: \[Relevance explanation\]  
Example Posts:  
\- Post 1: \[Title \+ Overview\]  
\- Post 2: \[Title \+ Overview\]  
\- Post 3: \[Title \+ Overview\]

## **Technical Integration**

### **Database Schema**

sql  
Copy  
\-- Exercise Completion Tracking  
CREATE TABLE exercise\_completion (  
    user\_id UUID REFERENCES auth.users,  
    exercise\_id TEXT,  
    completed\_at TIMESTAMPTZ DEFAULT NOW(),  
    results JSONB,  
    PRIMARY KEY (user\_id, exercise\_id)  
);

\-- Writing Style Preferences  
CREATE TABLE user\_style (  
    user\_id UUID REFERENCES auth.users,  
    tone TEXT,  
    vocabulary\_level TEXT,  
    structure\_preferences JSONB,  
    formatting\_preferences JSONB,  
    updated\_at TIMESTAMPTZ DEFAULT NOW()  
);

\-- Content Themes  
CREATE TABLE content\_themes (  
    user\_id UUID REFERENCES auth.users,  
    themes JSONB,  
    examples JSONB,  
    created\_at TIMESTAMPTZ DEFAULT NOW()  
);

### **Exercise Data Flow**

mermaid  
Copy  
sequenceDiagram  
    participant U as User  
    participant E as Exercise System  
    participant AI as OpenAI  
    participant DB as Supabase  
      
    U-\>\>E: Start Exercise  
    E-\>\>DB: Check Prerequisites  
    alt Prerequisites Not Met  
        DB--\>\>E: Missing Exercises  
        E--\>\>U: Prompt to Complete  
    else Prerequisites Met  
        E-\>\>AI: Generate Questions  
        AI--\>\>U: Present Question  
        U-\>\>AI: Provide Answer  
        AI-\>\>DB: Store Response  
    end

**Templates & Formatting**

### **Default Writing Style**

markdown  
Copy  
Base Style:  
\- Professional yet conversational  
\- Clear and concise  
\- Business casual writing  
\- Thoughtful and intentional  
\- Balanced depth and accessibility

### **Post Templates**

markdown  
Copy  
1\. Contrary Template  
\- Contrary statement  
\- Statement expansion  
\- Solution teaser  
\- Benefits explanation  
\- Solution expansion  
\- Additional detail pointer

2\. Actionable Template  
\- Problem citation  
\- Old method list  
\- New method list  
\- Comparative conclusion

\[Additional templates...\]

### **Exercise Output Formats**

#### **Why Exercise Output**

markdown  
Copy  
Summary of your 'why': \[Motivation paragraph\]  
One-liner (your WHY\!): \[Focused statement\]  
Action Reminder: \[Display prompt\]

#### **Content Themes Output**

markdown  
Copy  
Theme: \[Title\]  
Overview: \[Relevance explanation\]  
Example Posts:  
1\. \[Title \+ Overview\]  
2\. \[Title \+ Overview\]  
3\. \[Title \+ Overview\]

**System Integration & Implementation Guidelines**

### **Exercise Prerequisites Checking**

javascript  
Copy  
async function checkPrerequisites(userId, exerciseId) {  
    const prerequisites \= {  
        'finding-your-niche': \['time-travel', 'only-i'\],  
        'audience-advanced': \['audience-basic'\],  
        'content-pillars': \['content-theme-helper'\]  
    };

    const requiredExercises \= prerequisites\[exerciseId\] || \[\];  
    // Check completion status in Supabase  
}

### **OpenAI Integration**

javascript  
Copy  
// Exercise Question Generation  
const exercisePrompts \= {  
    'why-seven-levels': {  
        system\_role: "Expert in helping people build professional brand",  
        initial\_question: "Why do you want to be on LinkedIn?",  
        max\_questions: 7  
    },  
    'time-travel': {  
        system\_role: "Expert in guiding reflective journeys",  
        questions: \[  
            "How have you changed over these 2 years?",  
            "What do I do in my job \- how has my expertise grown?",  
            // Additional questions...  
        \]  
    }  
};

### **Result Storage & Retrieval**

typescript  
Copy  
interface ExerciseResult {  
    userId: string;  
    exerciseId: string;  
    responses: {  
        question: string;  
        answer: string;  
        analysis: string;  
    }\[\];  
    summary: string;  
    createdAt: Date;  
}

### **Exercise Progression**

mermaid  
Copy  
graph TD  
    A\[Start Exercise\] \--\> B{Prerequisites Met?}  
    B \--\>|No| C\[Show Required Exercises\]  
    B \--\>|Yes| D\[Begin Exercise\]  
    D \--\> E\[Ask Single Question\]  
    E \--\> F\[Process Response\]  
    F \--\> G\[Provide Analysis\]  
    G \--\> H{More Questions?}  
    H \--\>|Yes| E  
    H \--\>|No| I\[Generate Summary\]  
    I \--\> J\[Store Results\]

### **Exercise Personalization**

#### **Data Sources**

markdown  
Copy  
1\. User Profile Data  
   \- Job role  
   \- Company  
   \- Industry  
   \- Experience level

2\. Previous Exercise Results  
   \- Writing style preferences  
   \- Content themes  
   \- Audience definition  
   \- Core motivation

3\. System Configuration  
   \- Default templates  
   \- Industry-specific prompts  
   \- Role-based suggestions

### **Exercise Integration with Post Writing**

markdown  
Copy  
1\. Style Application  
   \- Apply user's writing style  
   \- Use identified themes  
   \- Target defined audience

2\. Content Suggestions  
   \- Based on completed exercises  
   \- Aligned with identified niche  
   \- Matching personal brand

3\. Template Selection  
   \- Informed by writing style  
   \- Adapted to content type  
   \- Personalized structure

**Implementation Checklist & Testing Guidelines**

### **Pre-Exercise Setup**

markdown  
Copy  
1\. Database Preparation  
   \- Create required tables  
   \- Set up relationships  
   \- Configure access permissions

2\. OpenAI Integration  
   \- Configure API access  
   \- Set up error handling  
   \- Implement retry logic

3\. User State Management  
   \- Track exercise progress  
   \- Store interim results  
   \- Handle session persistence

### **Testing Requirements**

markdown  
Copy  
1\. Exercise Flow Testing  
   \- Prerequisite checking  
   \- Question sequencing  
   \- Response handling  
   \- Result storage

2\. Data Integration Testing  
   \- Exercise results storage  
   \- Post writing integration  
   \- Style application  
   \- Template matching

3\. User Experience Testing  
   \- Clear instructions  
   \- Helpful feedback  
   \- Error handling  
   \- Progress indication

### **Quality Assurance**

markdown  
Copy  
1\. Response Quality  
   \- Relevance checking  
   \- Consistency verification  
   \- Style adherence  
   \- Personalization accuracy

2\. System Performance  
   \- Response times  
   \- Data retrieval speed  
   \- Storage efficiency  
   \- API reliability

## **Maintenance Guidelines & System Updates**

### **Regular Maintenance Tasks**

markdown  
Copy  
1\. Exercise Content Updates  
   \- Review exercise questions  
   \- Update templates  
   \- Refresh example content  
   \- Validate dependencies

2\. Database Maintenance  
   \- Clean old exercise data  
   \- Optimize queries  
   \- Update indexes  
   \- Archive unused data

3\. AI Integration Updates  
   \- Review OpenAI prompts  
   \- Update model parameters  
   \- Optimize response handling  
   \- Update error handling

### **Exercise Results Integration**

markdown  
Copy  
1\. Post Writing System  
   \- Style application  
   \- Theme integration  
   \- Audience targeting  
   \- Template selection

2\. Beige-ometer Integration  
   \- Score calculation  
   \- Quality assessment  
   \- Personalization level  
   \- Style matching

## **Final Implementation Notes**

### **Success Metrics**

markdown  
Copy  
1\. Exercise Completion Rates  
2\. User Engagement Levels  
3\. Content Quality Scores  
4\. System Performance Metrics

### **Best Practices**

markdown  
Copy  
1\. Always preserve user progress  
2\. Provide clear feedback  
3\. Maintain exercise dependencies  
4\. Regular data validation  
5\. Monitor AI response quality

