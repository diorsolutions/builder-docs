# Project Architecture Deep Dive

## 🏗️ How Everything Connects

### Data Flow Diagram

\`\`\`
User Input → Customizer → State Management → Template Renderer → Live Preview
     ↓
Database ← API Routes ← Form Submission
\`\`\`

### Detailed Flow Explanation

1. **User Authentication**
   \`\`\`
   Login Form → NextAuth → MongoDB → Session Cookie → Protected Routes
   \`\`\`

2. **Profile Management**
   \`\`\`
   Profile Form → API Route (/api/profile) → MongoDB → Updated User Data
   \`\`\`

3. **Template Customization**
   \`\`\`
   Customizer Form → React State → Template Props → Live Preview Update
   \`\`\`

4. **Export Process**
   \`\`\`
   Export Button → Generate Files → Create ZIP → Download
   \`\`\`

## 🔄 State Management

### Local State (React useState)
- Form data in customizers
- UI states (loading, modals)
- Temporary preview data

### Server State (Database)
- User profiles
- Authentication sessions
- Saved portfolio configurations

### Example State Flow

\`\`\`tsx
// In portfolio-customizer.tsx
const [formData, setFormData] = useState(userData) // Local state

const handleSave = async () => {
  // Update server state
  await fetch('/api/profile', {
    method: 'PUT',
    body: JSON.stringify(formData)
  })
  
  // Update local state
  setUserData(formData)
}
\`\`\`

## 📁 File Relationships

### Template System
\`\`\`
components/templates/
├── minimal-dev-template.tsx     → Receives userData props
├── creative-artist-template.tsx → Receives userData props
└── business-pro-template.tsx    → Receives userData props

components/portfolio/
├── template-renderer.tsx        → Dynamically loads templates
└── portfolio-customizer.tsx     → Provides userData to renderer
\`\`\`

### API Structure
\`\`\`
app/api/
├── auth/
│   ├── [...nextauth]/route.ts   → Handles all auth
│   └── signup/route.ts          → Creates new users
├── profile/route.ts             → GET/PUT user profiles
└── upload/route.ts              → Handles image uploads
\`\`\`

## 🎨 Styling Architecture

### CSS Hierarchy
1. **Global Styles** (`app/globals.css`)
   - CSS variables for themes
   - Base Tailwind imports
   - Custom utility classes

2. **Component Styles**
   - Tailwind classes in JSX
   - Conditional styling based on props/state
   - Responsive design classes

3. **Theme System**
   - CSS variables for colors
   - Theme provider context
   - Dynamic class application

### Example Styling Pattern
\`\`\`tsx
// Theme-aware component
const buttonClasses = cn(
  "px-4 py-2 rounded-md transition-colors", // Base styles
  "bg-primary text-primary-foreground",     // Theme variables
  "hover:bg-primary/90",                    // Interactive states
  "md:px-6 md:py-3",                       // Responsive
  className                                 // Override prop
)
\`\`\`

## 🔧 Extension Points

### Adding New Features

1. **New Template**
   - Create component in `components/templates/`
   - Add to `lib/templates.ts`
   - Update template gallery

2. **New Input Type**
   - Create component in `components/ui/`
   - Add to customizer forms
   - Update database schema

3. **New API Endpoint**
   - Create route in `app/api/`
   - Add error handling
   - Update frontend calls

## 🚀 Performance Considerations

### Optimization Strategies
- **Image Optimization**: Next.js Image component + Cloudinary
- **Code Splitting**: Dynamic imports for templates
- **Caching**: API responses cached where appropriate
- **Bundle Size**: Tree-shaking unused components

### Example Optimization
\`\`\`tsx
// Dynamic template loading
const TemplateComponent = dynamic(
  () => import(`../templates/${templateId}-template`),
  { 
    loading: () => <LoadingSpinner />,
    ssr: false // Client-side only for customizer
  }
)
\`\`\`

## 🔒 Security Architecture

### Authentication Flow
\`\`\`
Client → NextAuth → JWT Token → API Middleware → Protected Resource
\`\`\`

### Data Validation
- **Frontend**: Form validation with react-hook-form
- **Backend**: API route validation
- **Database**: Schema validation in MongoDB

### Security Measures
- CSRF protection via NextAuth
- Input sanitization
- File upload restrictions
- Environment variable protection
