# **Ghostart Admin System Documentation**

## **Table of Contents**

1. [Role-Based Access Control](https://claude.ai/chat/a6b3743f-eb6a-4732-ae22-781a55768452#role-based-access-control)  
2. [User Management](https://claude.ai/chat/a6b3743f-eb6a-4732-ae22-781a55768452#user-management)  
3. [System Monitoring](https://claude.ai/chat/a6b3743f-eb6a-4732-ae22-781a55768452#system-monitoring)  
4. [Feature Access Control](https://claude.ai/chat/a6b3743f-eb6a-4732-ae22-781a55768452#feature-access-control)  
5. [Database Schema](https://claude.ai/chat/a6b3743f-eb6a-4732-ae22-781a55768452#database-schema)

## **Role-Based Access Control**

### **Role Hierarchy**

mermaid  
Copy  
graph TD  
    subgraph Access Levels  
        A\[Full Access\]  
        B\[Content Only\]  
        C\[Exercises Only\]  
        D\[Training \+ Exercises\]  
        E\[Posting \+ Exercises\]  
    end

    subgraph Features  
        F\[Content Library\]  
        G\[Exercise System\]  
        H\[Training Courses\]  
        I\[Post Creation\]  
        J\[Content Calendar\]  
    end

    A \--\> F & G & H & I & J  
    B \--\> F  
    C \--\> G  
    D \--\> G & H

    E \--\> G & I & J

### **Access Control Middleware**

typescript  
Copy  
const checkFeatureAccess \= (  
    requiredFeature: keyof FeatureAccess  
) \=\> async (req: Request, res: Response, next: NextFunction) \=\> {  
    const userRole \= await getUserRole(req.user.id);  
    const permissions \= await getPermissions(userRole);  
      
    if (permissions\[requiredFeature\]) {  
        next();  
    } else {  
        res.status(403).json({  
            error: 'Access Denied',  
            upgrade: await getUpgradePath(userRole, requiredFeature)  
        });  
    }

};

## **User Management**

### **Role Assignment Schema**

sql  
Copy  
\-- Role Definitions  
CREATE TYPE user\_role\_type AS ENUM (  
    'full\_access',  
    'content\_only',  
    'exercises\_only',  
    'training\_exercises',  
    'posting\_exercises'  
);

\-- Role Permissions Table  
CREATE TABLE role\_permissions (  
    role user\_role\_type PRIMARY KEY,  
    permissions JSONB NOT NULL,  
    description TEXT NOT NULL,  
    upgrade\_path JSONB

);

### **Dashboard Components**

typescript  
Copy  
interface SystemMetrics {  
    api: {  
        health: number;  
        responseTime: number;  
        errorRate: number;  
        requestVolume: number;  
    };  
    users: {  
        active: number;  
        newSignups: number;  
        postVolume: number;  
        exerciseCompletions: number;  
    };  
    content: {  
        queueSize: number;  
        successRate: number;  
        failureRate: number;  
        retryCount: number;  
    };

}

## **System Monitoring**

### **Role-Based Dashboard Flow**

mermaid  
Copy  
sequenceDiagram  
    participant U as User  
    participant D as Dashboard  
    participant F as Features  
    participant UP as Upgrade Prompts  
      
    rect rgb(240, 248, 255\)  
        Note over U,UP: Full Access View  
        U\-\>\>D: Load Dashboard  
        D\-\>\>F: Load All Features  
        F\--\>\>U: Show Complete Dashboard  
    end  
      
    rect rgb(255, 240, 245\)  
        Note over U,UP: Limited Access  
        U\-\>\>D: Load Dashboard  
        D\-\>\>F: Load Available Features  
        D\-\>\>UP: Load Upgrade Options  
        F\--\>\>U: Show Available Features  
        UP\--\>\>U: Show Upgrade Paths

    end

## **Feature Access Control**

### **User Role Layout Component**

typescript  
Copy  
const RoleBasedLayout: React.FC\<{ userRole: UserRole }\> \= ({ userRole, children }) \=\> {  
    const permissions \= usePermissions(userRole);  
      
    return (  
        \<div className\="app-layout"\>  
            \<Sidebar permissions\={permissions} /\>  
            \<main className\="content-area"\>  
                {children}  
                {\!isFullAccess(permissions) && (  
                    \<UpgradePrompts userRole\={userRole} /\>  
                )}  
            \</main\>  
        \</div\>  
    );

};

### **Notification Preferences**

typescript  
Copy  
interface NotificationPreferences {  
    channels: {  
        email: boolean;  
        inApp: boolean;  
        push: boolean;  
    };  
    types: {  
        performance: boolean;  
        system: boolean;  
        exercises: boolean;  
        suggestions: boolean;  
    };  
    frequency: {  
        type: 'immediate' | 'daily' | 'weekly' | 'custom';  
        customSchedule?: {  
            days: number\[\];  
            times: string\[\];  
        };  
    };  
    smart: {  
        aiTiming: boolean;  
        priorityLevel: 1 | 2 | 3;  
        contextAware: boolean;  
    };

}

## **Implementation Notes**

* Role-based access control implemented at middleware level  
* Feature access determined by user role permissions  
* Upgrade paths configured in database  
* Monitoring dashboard with real-time metrics  
* Smart notification system with AI-powered timing  
* Comprehensive audit logging system  
* Automated role assignment based on subscription level  
* Granular permission control for admin users

All components integrate with Supabase for data persistence and Redis for caching frequently accessed permission data.  
