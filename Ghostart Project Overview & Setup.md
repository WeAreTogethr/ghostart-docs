\# Ghostart Project Overview & Setup

\#\# \*\*What is Ghostart?\*\*  
Ghostart is an \*\*AI-powered LinkedIn content platform\*\* that helps users \*\*create, optimize, and automate LinkedIn posts\*\* while tracking engagement and enhancing thought leadership. It integrates \*\*AI exercises, content generation, scheduling, and analytics tracking\*\* for individuals and brand advocacy programs.

\#\# \*\*Platform Structure\*\*  
\#\#\# \*\*1. Post Writing (Landing Page)\*\*  
The post-writing section is the primary interface users see when they log in. It allows them to draft, generate, and preview LinkedIn posts while pulling in audience insights and brand content.

\#\#\#\# \*\*User Interface\*\*  
\- \*\*Right Side (Preview Panel)\*\*  
  \- Displays a live preview of how the LinkedIn post will appear.  
  \- If the user hasn’t linked their LinkedIn profile, a prompt appears to connect their account.  
  \- Actions available: Post to LinkedIn, schedule the post, save as a draft.

\- \*\*Left Side (Post Drafting)\*\*  
  \- Users draft their post in a text box.  
  \- AI-powered dropdown options to customize content:  
    \- \*\*Choose Audience\*\* → Filters based on Primary, Secondary, Tertiary, All, or General audiences.  
    \- \*\*Choose Post Type\*\* → Sales post, engagement post, etc. (defined by the superadmin).  
    \- \*\*Choose Template\*\* → Users select predefined content structures (e.g., “Lessons Learned”, “Top 10”).  
    \- \*\*Brainstorm Hooks\*\* → AI generates 10 different hooks; users can select one to insert at the top of the post.

\#\#\#\# \*\*Sandpit Column (Left Sidebar \- Collapsible)\*\*  
\- \*\*Brand Content\*\* → Users in brand teams can view brand-approved posts/assets and create AI-personalized drafts.  
\- \*\*Ideas Bank\*\* → Users can save and retrieve post ideas (notes, screenshots, website snippets).  
\- \*\*Generate Ideas\*\* → AI suggests post ideas based on user input or past engagement data.  
\- \*\*Content Calendar\*\* → Users can schedule drafted posts directly.

\#\#\# \*\*2. AI-Driven Enhancements\*\*  
\- \*\*Post Personalization\*\* → AI generates posts based on stored user data (target audience, goals, industry, role).  
\- \*\*Post Resharing (Brand Content & Assets)\*\* → AI drafts a personalized commentary when resharing LinkedIn posts.  
\- \*\*“Beige-ometer” Score\*\* → Rates posts from “Beige” (generic) to “Green” (engaging & personalized).  
\- \*\*Improvement Suggestions\*\* → Users can request AI to rewrite the post with improvements.

\#\#\# \*\*3. Content Calendar\*\*  
\- Monthly/Weekly/Daily View → Users can see scheduled content at a glance.  
\- Drag & Drop Scheduling → Easily move posts to different dates/times.  
\- Edit & Delete → Users can modify, reschedule, or remove drafts.  
\- Post History & Analytics → Previously published posts appear with performance metrics (Red \= Poor, Orange \= Average, Green \= High Engagement).

\#\#\# \*\*4. Training Hub\*\*  
Ghostart features a Thinkific-style training bank with courses on content strategy, LinkedIn engagement, and personal branding.  
\- \*\*Courses Include:\*\*  
  \- Video modules  
  \- Text guides  
  \- AI-powered exercises  
\- \*\*Smart Course Progression:\*\*  
  \- Exercises completed in training are recognized system-wide.  
  \- Users are prompted to continue unfinished training modules.  
  \- If a user hasn’t been posting regularly, the system suggests training on consistency.

\#\#\# \*\*5. AI-Personalized Content\*\*  
\- \*\*AI Content Packs\*\* → Users can purchase AI-generated LinkedIn posts, email templates, or social media prompts.  
\- \*\*Monetization Model\*\* → One-time purchases (€49–€199 per pack) and subscription options (€49–€299/month).  
\- \*\*Workflow:\*\*  
  1\. User selects a content pack.  
  2\. System checks stored exercise data.  
  3\. AI generates personalized content (or generic if no exercises are completed).  
  4\. AI delivers content via dashboard & email.

\#\#\# \*\*6. Dashboard\*\*  
\- \*\*User Progress Overview\*\* → Training completed, exercises done, LinkedIn engagement stats.  
\- \*\*30-Day Post Performance Summary\*\* → Impressions, engagement, comments.  
\- \*\*Content Insights\*\* → AI recommendations based on successful past posts.

\#\#\# \*\*7. User Settings\*\*  
\- Profile customization → Name, email, profile image.  
\- LinkedIn integration → Connect/disconnect LinkedIn account.  
\- Company & job role settings.

\---

\#\# \*\*Backend & Technical Overview\*\*

\#\#\# \*\*Tech Stack:\*\*  
\- \*\*Frontend:\*\* Next.js (Vercel)  
\- \*\*Backend:\*\* Supabase (DB & Auth)  
\- \*\*AI Tools:\*\* OpenAI API  
\- \*\*Payments:\*\* Stripe  
\- \*\*Hosting & Security:\*\*  
  \- Cloudflare (DDoS protection)  
  \- Environment Variables for secrets

\#\#\# \*\*Key Integrations:\*\*  
\- \*\*LinkedIn API:\*\* OAuth-based login, post-sharing, and analytics retrieval.  
\- \*\*Supabase:\*\*  
  \- Row-Level Security (RLS) to ensure users only see their data.  
  \- Automated database updates with AI tools.  
\- \*\*OpenAI:\*\* AI-driven content generation, hooks, and improvements.  
\- \*\*Stripe:\*\* Payment processing for AI tools & subscriptions.

\---

\#\# \*\*Deployment & Setup Notes\*\*  
\- \*\*GitHub Repository:\*\* \[Ghostart Repo\](https://github.com/WeAreTogethr/ghostart-docs)  
\- \*\*Production Deployment:\*\* \`ghostart.vercel.app\` (Currently not live \- 404\)  
\- \*\*Navigation Design Decision Needed:\*\* Likely a \*\*horizontal menu\*\* for the user section.

\---

\#\# \*\*Next Steps\*\*  
1\. \*\*Finalize UX/UI decisions\*\* for the post-writing interface.  
2\. \*\*Develop the AI-personalization flow\*\* based on exercise data.  
3\. \*\*Integrate LinkedIn API fully\*\* for post scheduling & engagement tracking.  
4\. \*\*Beta testing & iteration\*\* with early users before launch.

\---

This document serves as the foundational \*\*Project Overview & Setup\*\* for Ghostart. Further sections (e.g., Admin System, Security, LinkedIn Integration) will expand upon this core functionality.

\---

