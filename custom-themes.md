# Adding Custom Themes

## 🎨 Theme System Overview

The application uses a dual theme system:
1. **Site-wide themes** (light/dark mode for the platform)
2. **Template themes** (color schemes for portfolio templates)

## Adding Site-wide Themes

### Step 1: Update Theme Provider

Edit `components/theme-provider.tsx`:

\`\`\`tsx
// Add new theme option
const themes = ['light', 'dark', 'blue', 'purple'] as const
\`\`\`

### Step 2: Add CSS Variables

Edit `app/globals.css`:

\`\`\`css
/* Add your custom theme */
.blue {
  --background: 220 100% 97%;
  --foreground: 220 100% 10%;
  --primary: 220 100% 50%;
  --primary-foreground: 0 0% 100%;
  /* ... add all color variables */
}

.purple {
  --background: 270 100% 97%;
  --foreground: 270 100% 10%;
  --primary: 270 100% 50%;
  --primary-foreground: 0 0% 100%;
  /* ... add all color variables */
}
\`\`\`

### Step 3: Update Theme Toggle

Edit `components/ui/theme-toggle.tsx`:

\`\`\`tsx
// Add new theme options
<DropdownMenuRadioItem value="blue">
  <Palette className="mr-2 h-4 w-4" />
  Blue
</DropdownMenuRadioItem>
<DropdownMenuRadioItem value="purple">
  <Palette className="mr-2 h-4 w-4" />
  Purple
</DropdownMenuRadioItem>
\`\`\`

## Adding Template Color Schemes

### Step 1: Define Color Schemes

Create `lib/template-themes.ts`:

\`\`\`tsx
export const templateThemes = {
  default: {
    primary: 'bg-blue-600',
    secondary: 'bg-gray-100',
    accent: 'bg-blue-100',
    text: 'text-gray-900'
  },
  sunset: {
    primary: 'bg-orange-500',
    secondary: 'bg-orange-50',
    accent: 'bg-orange-100',
    text: 'text-orange-900'
  },
  forest: {
    primary: 'bg-green-600',
    secondary: 'bg-green-50',
    accent: 'bg-green-100',
    text: 'text-green-900'
  }
}
\`\`\`

### Step 2: Update Template Components

Modify template components to accept theme props:

\`\`\`tsx
interface TemplateProps {
  userData: UserProfile
  theme?: keyof typeof templateThemes
}

export function MinimalDevTemplate({ userData, theme = 'default' }: TemplateProps) {
  const colors = templateThemes[theme]
  
  return (
    <div className={`min-h-screen ${colors.secondary}`}>
      <header className={`${colors.primary} text-white`}>
        {/* Template content */}
      </header>
    </div>
  )
}
\`\`\`

### Step 3: Add Theme Selector to Customizer

Update `components/portfolio/portfolio-customizer.tsx`:

\`\`\`tsx
// Add theme selection
<div className="space-y-2">
  <Label>Template Theme</Label>
  <Select value={selectedTheme} onValueChange={setSelectedTheme}>
    <SelectTrigger>
      <SelectValue placeholder="Choose theme" />
    </SelectTrigger>
    <SelectContent>
      <SelectItem value="default">Default Blue</SelectItem>
      <SelectItem value="sunset">Sunset Orange</SelectItem>
      <SelectItem value="forest">Forest Green</SelectItem>
    </SelectContent>
  </Select>
</div>
\`\`\`

## 🎯 Pro Tips

1. **Use CSS Variables**: They make theme switching smooth
2. **Test Accessibility**: Ensure good contrast ratios
3. **Mobile First**: Test themes on all screen sizes
4. **Consistent Naming**: Use semantic color names
