# Quick Start Guide for Developers

## 🚀 Getting Started (5 minutes)

### 1. Clone and Setup
\`\`\`bash
git clone <repository>
cd portfolio-builder
npm install
\`\`\`

### 2. Environment Variables
Copy `.env.example` to `.env` and fill in your values:
\`\`\`env
MONGODB_URI=your_mongodb_connection_string
NEXTAUTH_SECRET=your_secret_key
CLOUDINARY_CLOUD_NAME=your_cloudinary_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
\`\`\`

### 3. Run Development Server
\`\`\`bash
npm run dev
\`\`\`

Visit `http://localhost:3000` and you're ready!

## 🎯 Common Tasks

### Adding a New Skill Input
1. Open `components/portfolio/portfolio-customizer.tsx`
2. Find the skills section
3. Add your new input:
\`\`\`tsx
<div className="space-y-2">
  <Label>New Skill Category</Label>
  <Input
    value={formData.newSkill || ''}
    onChange={(e) => setFormData(prev => ({ 
      ...prev, 
      newSkill: e.target.value 
    }))}
    placeholder="Enter skill..."
  />
</div>
\`\`\`

### Changing Template Colors
1. Open the template file (e.g., `components/templates/minimal-dev-template.tsx`)
2. Find color classes like `bg-blue-600`
3. Replace with your preferred color:
\`\`\`tsx
// Change from
<div className="bg-blue-600 text-white">

// Change to
<div className="bg-purple-600 text-white">
\`\`\`

### Adding Loading States
\`\`\`tsx
const [isLoading, setIsLoading] = useState(false)

const handleSubmit = async () => {
  setIsLoading(true)
  try {
    // Your async operation
    await saveData()
  } finally {
    setIsLoading(false)
  }
}

return (
  <Button disabled={isLoading}>
    {isLoading ? (
      <>
        <LoadingSpinner className="mr-2" />
        Please wait...
      </>
    ) : (
      'Save Changes'
    )}
  </Button>
)
\`\`\`

## 🎨 Customization Examples

### Custom Theme Colors
Edit `app/globals.css`:
\`\`\`css
:root {
  --primary: 142 69 173;        /* Purple */
  --secondary: 251 146 60;      /* Orange */
  --accent: 34 197 94;          /* Green */
}
\`\`\`

### New Template Section
\`\`\`tsx
// Add to any template
<section className="py-16 bg-gray-50">
  <div className="max-w-4xl mx-auto px-4">
    <h2 className="text-3xl font-bold mb-8">New Section</h2>
    <div className="grid md:grid-cols-2 gap-8">
      {/* Your content */}
    </div>
  </div>
</section>
\`\`\`

## 🐛 Troubleshooting

### Common Issues

**Templates not updating?**
- Check if you're passing the correct props
- Verify the template renderer is importing correctly

**Styles not applying?**
- Make sure Tailwind classes are correct
- Check if CSS variables are defined

**API errors?**
- Verify environment variables
- Check MongoDB connection
- Look at browser console for details

### Debug Mode
Add this to any component for debugging:
\`\`\`tsx
console.log('[v0] Current data:', formData)
console.log('[v0] User profile:', userData)
\`\`\`

## 📚 Next Steps

1. **Read the full documentation** in `/docs/`
2. **Explore example templates** in `/components/templates/`
3. **Check the API routes** in `/app/api/`
4. **Customize the theme** in `/app/globals.css`
5. **Add your own features** following the patterns shown

## 🤝 Need Help?

- Check the documentation files in `/docs/`
- Look at existing components for patterns
- Use browser dev tools for debugging
- Add console.log statements to trace data flow
