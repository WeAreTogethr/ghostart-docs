# **Ideas Bank Documentation**

## **Table of Contents**

1. **Overview & Architecture**  
2. **Interface Design & Layout**  
3. **Core Features & Implementation**  
4. **Technical Specifications**  
5. **Development Phases**

## **Overview & Architecture**

**The Ideas Bank is a central repository for collecting, organizing, and managing content ideas through multiple input methods:**

* **Browser extension**  
* **Email integration**  
* **Direct platform input**

### **Core System Flow**

**mermaid**  
**Copy**  
**graph TD**  
    **subgraph Core Features**  
        **A\[Ideas Bank Dashboard\]**  
        **B\[Note Creation\]**  
        **C\[Media Upload\]**  
        **D\[Idea Organization\]**  
    **end**

    **subgraph Note Components**  
        **B1\[Rich Text Editor\]**  
        **B2\[Title & Tags\]**  
        **B3\[Categories\]**  
        **B4\[Draft Generation\]**  
    **end**

    **subgraph Media Handling**  
        **C1\[Image Upload\]**  
        **C2\[Screenshot Storage\]**  
        **C3\[URL Embedding\]**  
        **C4\[PDF Storage\]**  
    **end**

    **subgraph Organization**  
        **D1\[Folders\]**  
        **D2\[Search\]**  
        **D3\[Filters\]**  
        **D4\[Tags\]**  
    **end**

    **A \--\> B & C & D**  
    **B \--\> B1 & B2 & B3 & B4**  
    **C \--\> C1 & C2 & C3 & C4**  
    **D \--\> D1 & D2 & D3 & D4**

**Ideas Bank Documentation \[continued\]**

## **Interface Design & Layout**

### **Main Layout Structure**

**mermaid**  
**Copy**  
**graph TD**  
    **subgraph Page Layout**  
        **A\[Top Navigation Bar\]**  
        **B\[Left Sidebar\]**  
        **C\[Main Content Area\]**  
        **D\[Right Panel\]**  
    **end**

    **subgraph Left Sidebar Components**  
        **B1\[All Ideas\]**  
        **B2\[Folders\]**  
        **B3\[Tags\]**  
        **B4\[Recent\]**  
        **B5\[Create New\]**  
    **end**

    **subgraph Main Content Options**  
        **C1\[Grid View\]**  
        **C2\[List View\]**  
        **C3\[Search Bar\]**  
        **C4\[Filter Options\]**  
    **end**

    **subgraph Right Panel Details**  
        **D1\[Idea Preview\]**  
        **D2\[Quick Edit\]**  
        **D3\[Actions Menu\]**  
    **end**

    **A \--\> B & C & D**  
    **B \--\> B1 & B2 & B3 & B4 & B5**  
    **C \--\> C1 & C2 & C3 & C4**

    **D \--\> D1 & D2 & D3**

### **Creation Interface**

**mermaid**  
**Copy**  
**graph TD**  
    **subgraph Creation Modal**  
        **A\[Header Bar\]**  
        **B\[Content Area\]**  
        **C\[Footer Actions\]**  
    **end**

    **subgraph Header Components**  
        **A1\[Title Input\]**  
        **A2\[Type Selector\]**  
        **A3\[Close Button\]**  
    **end**

    **subgraph Content Types**  
        **B1\[Rich Text Editor\]**  
        **B2\[Media Uploader\]**  
        **B3\[URL Input\]**  
        **B4\[Tags Input\]**  
    **end**

    **subgraph Footer Options**  
        **C1\[Save Draft\]**  
        **C2\[Create Post\]**  
        **C3\[Add to Calendar\]**  
    **end**

    **A \--\> A1 & A2 & A3**  
    **B \--\> B1 & B2 & B3 & B4**

    **C \--\> C1 & C2 & C3**

**Ideas Bank Documentation \[continued\]**

## **Core Features & Implementation**

### **Integration Flows**

**mermaid**  
**Copy**  
**sequenceDiagram**  
    **participant U as User**  
    **participant E as Browser Extension**  
    **participant M as Email Handler**  
    **participant A as API**  
    **participant S as Storage**  
      
    **rect rgb(240, 248, 255\)**  
        **Note over U,S: Browser Extension Flow**  
        **U\-\>\>E: Click Extension Button**  
        **E\-\>\>E: Capture Screenshot/Selection**  
        **E\-\>\>A: Send to API**  
        **A\-\>\>S: Store Content**  
        **S\--\>\>U: Confirm Save**  
    **end**  
      
    **rect rgb(255, 240, 245\)**  
        **Note over U,S: Email Integration Flow**  
        **U\-\>\>M: Send to ideas@ghostart.com**  
        **M\-\>\>A: Process Email**  
        **A\-\>\>A: Extract Content**  
        **A\-\>\>S: Store in Ideas Bank**  
        **S\--\>\>U: Email Confirmation**

    **end**

### **User Interactions**

**mermaid**  
**Copy**  
**sequenceDiagram**  
    **participant U as User**  
    **participant L as List View**  
    **participant E as Editor**  
    **participant P as Preview**  
      
    **rect rgb(240, 248, 255\)**  
        **Note over U,P: Create New Idea**  
        **U\-\>\>L: Click "Create New"**  
        **L\-\>\>E: Open Editor**  
        **U\-\>\>E: Enter Content**  
        **U\-\>\>E: Add Media**  
        **E\-\>\>L: Save Idea**  
        **L\-\>\>P: Show Preview**  
    **end**  
      
    **rect rgb(255, 240, 245\)**  
        **Note over U,P: Manage Existing**  
        **U\-\>\>L: Select Idea**  
        **L\-\>\>P: Show Details**  
        **U\-\>\>P: Edit/Tag**  
        **P\-\>\>L: Update List**

    **end**

**Ideas Bank Documentation \[continued\]**

## **Technical Specifications**

### **Database Schema**

**sql**  
**Copy**  
**CREATE TABLE ideas (**  
    **id UUID DEFAULT uuid\_generate\_v4() PRIMARY KEY,**  
    **user\_id UUID REFERENCES auth.users NOT NULL,**  
    **type TEXT NOT NULL,**  
    **title TEXT,**  
    **content JSONB,**  
    **metadata JSONB,**  
    **folders TEXT\[\],**  
    **created\_at TIMESTAMPTZ DEFAULT NOW(),**  
    **updated\_at TIMESTAMPTZ DEFAULT NOW()**  
**);**

**CREATE TABLE idea\_folders (**  
    **id UUID DEFAULT uuid\_generate\_v4() PRIMARY KEY,**  
    **user\_id UUID REFERENCES auth.users NOT NULL,**  
    **name TEXT NOT NULL,**  
    **parent\_id UUID REFERENCES idea\_folders(id),**  
    **created\_at TIMESTAMPTZ DEFAULT NOW()**

**);**

### **State Management Interface**

**typescript**  
**Copy**  
**interface IdeasStore {**  
    **ideas: Idea\[\];**  
    **folders: Folder\[\];**  
    **tags: Tag\[\];**  
    **filters: {**  
        **type?: IdeaType;**  
        **folder?: string;**  
        **tags?: string\[\];**  
        **dateRange?: DateRange;**  
    **};**  
    **actions: {**  
        **createIdea: (data: IdeaInput) \=\> Promise\<Idea\>;**  
        **updateIdea: (id: string, data: Partial\<IdeaInput\>) \=\> Promise\<void\>;**  
        **deleteIdea: (id: string) \=\> Promise\<void\>;**  
        **moveToFolder: (ideaId: string, folderId: string) \=\> Promise\<void\>;**  
    **};**

**}**

**Development Phases**

### **Phase 1: Core Functionality (MVP)**

* **Rich text editor for notes (using ProseMirror or TipTap)**  
* **Basic image/screenshot upload**  
* **Simple organization with tags**  
* **Search functionality**  
* **Direct integration with post creation**

### **Phase 2: Browser Extension**

**javascript**  
**Copy**  
**// Browser Extension Background Script**  
**chrome.runtime.onInstalled.addListener(() \=\> {**  
    **chrome.contextMenus.create({**  
        **id: 'save-to-ghostart',**  
        **title: 'Save to Ghostart Ideas Bank',**  
        **contexts: \['selection', 'image', 'link'\]**  
    **});**  
**});**

**// Screenshot Capture**  
**async function captureScreenshot() {**  
    **const screenshot \= await chrome.tabs.captureVisibleTab();**  
    **return screenshot;**  
**}**

### **Phase 3: Email Integration**

**javascript**  
**Copy**  
**// Email Handler (Next.js API Route)**  
**import { Resend } from 'resend';**

**export default async function handler(req, res) {**  
    **const { from, subject, text, attachments } \= req.body;**  
      
    **// Store in Ideas Bank**  
    **const idea \= {**  
        **type: 'email',**  
        **title: subject,**  
        **content: {**  
            **text,**  
            **attachments: attachments.map(processAttachment)**  
        **},**  
        **metadata: {**  
            **source: 'email',**  
            **created: new Date()**  
        **}**  
    **};**

    **await supabase.from('ideas').insert(idea);**  
**}**

### **Implementation Timeline**

1. **Core Features (2-3 weeks)**  
   * **Dashboard implementation**  
   * **Basic CRUD operations**  
   * **Search and filtering**  
2. **Browser Extension (2 weeks)**  
   * **Chrome/Firefox extension**  
   * **Screenshot functionality**  
   * **Context menu integration**  
3. **Email Integration (1-2 weeks)**  
   * **Email processing setup**  
   * **Attachment handling**  
   * **Confirmation system**

