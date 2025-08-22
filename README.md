# Portfolio Builder - Complete Documentation

## 🚀 Project Overview

This is a dynamic portfolio builder application that allows users to create, customize, and export professional portfolios using multiple templates.

## 📁 Project Structure

```
portfolio-builder/
├── app/                          # Next.js App Router pages
│   ├── api/                      # API routes
│   │   ├── auth/                 # Authentication endpoints
│   │   ├── profile/              # User profile management
│   │   └── upload/               # Image upload handling
│   ├── auth/                     # Authentication pages
│   ├── dashboard/                # User dashboard
│   ├── portfolio/                # Portfolio management
│   ├── profile/                  # Profile editing
│   └── templates/                # Template selection
├── components/                   # Reusable React components
│   ├── auth/                     # Authentication components
│   ├── dashboard/                # Dashboard components
│   ├── portfolio/                # Portfolio-related components
│   ├── profile/                  # Profile management components
│   ├── templates/                # Template components
│   └── ui/                       # Base UI components
├── lib/                          # Utility functions and configurations
├── docs/                         # Documentation files
└── public/                       # Static assets
```

## 🔧 How It Works

### 1. Authentication System
- Users sign up/login using NextAuth.js
- User data stored in MongoDB Atlas
- Session management with JWT tokens

### 2. Profile Management
- Users edit their information in `/profile`
- Data saved to MongoDB via API routes
- Real-time updates to portfolio previews

### 3. Template System
- Templates located in `components/templates/`
- Each template is a React component
- Templates receive user data as props
- Dynamic rendering based on selected template

### 4. Live Customization
- Split-screen interface: editor + preview
- Changes in customizer instantly update preview
- Real-time data binding between forms and templates

### 5. Export System
- Generates complete project files
- Supports multiple formats (Next.js, Static HTML, React)
- Downloads as ZIP file ready for deployment

## 📖 Detailed Guides

See individual guide files for specific topics:
- [Adding Custom Themes](./custom-themes.md)
- [Creating Custom Inputs](./custom-inputs.md)
- [Adding New Templates](./new-templates.md)
- [Extending the Customizer](./extending-customizer.md)
- [API Reference](./api-reference.md)
"# builder-docs" 
