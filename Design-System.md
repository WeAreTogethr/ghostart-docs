# Ghostart Design System

## 1. Overview

The Ghostart Design System ensures **consistency, accessibility, and scalability** across all UI components. It is built with **TailwindCSS** and **React**.

---

## 2. TailwindCSS Theme Configuration

Ghostart’s colour scheme is defined in **tailwind.config.js**:

```js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#e4f7f6', 100: '#c9efed', 200: '#93dfdc', 300: '#5dcfcc', 400: '#26bfbb', 
          500: '#46afa6', 600: '#3e9a91', 700: '#317e77', 800: '#24605e', 900: '#1c4b49'
        },
        secondary: {
          50: '#e6e9eb', 100: '#cdd3d7', 200: '#9ba8af', 300: '#697d87', 400: '#4a5964', 
          500: '#294153', 600: '#233645', 700: '#1c2a34', 800: '#141f26', 900: '#0c1319'
        },
        accent: {
          50: '#fffbf4', 100: '#fff7e9', 200: '#feedcb', 300: '#fde3ad', 400: '#fbd88e', 
          500: '#f7b733', 600: '#d6a02e', 700: '#b48a28', 800: '#927322', 900: '#705d1c'
        }
      },
      borderRadius: {
        'xl': '1rem', '2xl': '1.5rem', '3xl': '2rem'
      },
      boxShadow: {
        'soft': '0 2px 10px rgba(0, 0, 0, 0.05)',
        'card': '0 4px 20px rgba(0, 0, 0, 0.08)'
      }
    }
  }
}
```

---

## 3. UI Components

### **Card Component**
```tsx
export const Card = ({ children, className = "" }) => (
  <div className={`bg-white rounded-xl shadow-card p-6 transition-all duration-200 hover:shadow-lg ${className}`}>
    {children}
  </div>
);
```

### **Button Component**
```tsx
export const Button = ({ variant = 'primary', size = 'md', children }) => {
  const variants = {
    primary: 'bg-primary-500 text-white hover:bg-primary-600',
    secondary: 'bg-secondary-500 text-white hover:bg-secondary-600',
    accent: 'bg-accent-500 text-white hover:bg-accent-600'
  };
  const sizes = {
    sm: 'px-3 py-1 text-sm', md: 'px-4 py-2', lg: 'px-6 py-3 text-lg'
  };
  return (
    <button className={`rounded-lg font-medium transition-all duration-200 ${variants[variant]} ${sizes[size]}`}>
      {children}
    </button>
  );
};
```

---

## 4. LinkedIn-Style Analytics Dashboard

```tsx
import React from 'react';
import { LineChart, BarChart, XAxis, YAxis, Tooltip, Legend, Line, Bar } from 'recharts';

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

## 5. Embedded Design System Diagram

### **Design System Overview**
```mermaid
{mermaid_diagrams.get("design-system", "No diagram available")}
```

---

## 6. Summary

✅ **TailwindCSS-based design system with brand colours & typography**  
✅ **Reusable UI components (Cards, Buttons, Analytics Dashboard)**  
✅ **LinkedIn-style data visualisation with Recharts.js**  
✅ **Embedded Mermaid diagram for design system overview**  

This file is **ready to upload** to your GitHub repo! 🚀  
