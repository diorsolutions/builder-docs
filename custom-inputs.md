# Creating Custom Inputs and Dynamic Fields

## 🔧 Adding Custom Input Types

### Step 1: Create Custom Input Component

Create `components/ui/custom-input.tsx`:

\`\`\`tsx
import { useState } from 'react'
import { Input } from './input'
import { Label } from './label'
import { Button } from './button'
import { Plus, X } from 'lucide-react'

interface DynamicListInputProps {
  label: string
  value: string[]
  onChange: (value: string[]) => void
  placeholder?: string
}

export function DynamicListInput({ 
  label, 
  value, 
  onChange, 
  placeholder 
}: DynamicListInputProps) {
  const [newItem, setNewItem] = useState('')

  const addItem = () => {
    if (newItem.trim()) {
      onChange([...value, newItem.trim()])
      setNewItem('')
    }
  }

  const removeItem = (index: number) => {
    onChange(value.filter((_, i) => i !== index))
  }

  return (
    <div className="space-y-2">
      <Label>{label}</Label>
      
      {/* Display existing items */}
      {value.map((item, index) => (
        <div key={index} className="flex items-center gap-2">
          <Input value={item} readOnly className="flex-1" />
          <Button
            type="button"
            variant="outline"
            size="sm"
            onClick={() => removeItem(index)}
          >
            <X className="h-4 w-4" />
          </Button>
        </div>
      ))}
      
      {/* Add new item */}
      <div className="flex items-center gap-2">
        <Input
          value={newItem}
          onChange={(e) => setNewItem(e.target.value)}
          placeholder={placeholder}
          className="flex-1"
          onKeyPress={(e) => e.key === 'Enter' && addItem()}
        />
        <Button type="button" variant="outline" size="sm" onClick={addItem}>
          <Plus className="h-4 w-4" />
        </Button>
      </div>
    </div>
  )
}
\`\`\`

### Step 2: Add to Profile Schema

Update user profile type in `lib/auth.ts`:

\`\`\`tsx
export interface UserProfile {
  // ... existing fields
  customSkills: string[]        // New dynamic field
  achievements: string[]        // New dynamic field
  languages: string[]          // New dynamic field
}
\`\`\`

### Step 3: Update Database Schema

Add to signup route `app/api/auth/signup/route.ts`:

\`\`\`tsx
const newUser = {
  // ... existing fields
  customSkills: [],
  achievements: [],
  languages: []
}
\`\`\`

### Step 4: Add to Customizer

Update `components/portfolio/portfolio-customizer.tsx`:

\`\`\`tsx
import { DynamicListInput } from '../ui/custom-input'

// Add to form
<DynamicListInput
  label="Custom Skills"
  value={formData.customSkills || []}
  onChange={(value) => setFormData(prev => ({ ...prev, customSkills: value }))}
  placeholder="Add a skill..."
/>

<DynamicListInput
  label="Achievements"
  value={formData.achievements || []}
  onChange={(value) => setFormData(prev => ({ ...prev, achievements: value }))}
  placeholder="Add an achievement..."
/>
\`\`\`

## 🎨 Advanced Custom Inputs

### Color Picker Input

\`\`\`tsx
interface ColorPickerProps {
  label: string
  value: string
  onChange: (color: string) => void
}

export function ColorPicker({ label, value, onChange }: ColorPickerProps) {
  const colors = [
    '#3B82F6', '#EF4444', '#10B981', '#F59E0B',
    '#8B5CF6', '#EC4899', '#06B6D4', '#84CC16'
  ]

  return (
    <div className="space-y-2">
      <Label>{label}</Label>
      <div className="flex flex-wrap gap-2">
        {colors.map((color) => (
          <button
            key={color}
            type="button"
            className={`w-8 h-8 rounded-full border-2 ${
              value === color ? 'border-gray-900' : 'border-gray-300'
            }`}
            style={{ backgroundColor: color }}
            onClick={() => onChange(color)}
          />
        ))}
      </div>
    </div>
  )
}
\`\`\`

### Rich Text Editor

\`\`\`tsx
import { useState } from 'react'
import { Button } from './button'
import { Bold, Italic, List } from 'lucide-react'

interface RichTextEditorProps {
  label: string
  value: string
  onChange: (value: string) => void
}

export function RichTextEditor({ label, value, onChange }: RichTextEditorProps) {
  const [isBold, setIsBold] = useState(false)
  const [isItalic, setIsItalic] = useState(false)

  const toggleBold = () => {
    setIsBold(!isBold)
    // Add bold formatting logic
  }

  return (
    <div className="space-y-2">
      <Label>{label}</Label>
      <div className="border rounded-md">
        <div className="flex gap-1 p-2 border-b">
          <Button
            type="button"
            variant={isBold ? "default" : "outline"}
            size="sm"
            onClick={toggleBold}
          >
            <Bold className="h-4 w-4" />
          </Button>
          <Button
            type="button"
            variant={isItalic ? "default" : "outline"}
            size="sm"
            onClick={() => setIsItalic(!isItalic)}
          >
            <Italic className="h-4 w-4" />
          </Button>
        </div>
        <textarea
          value={value}
          onChange={(e) => onChange(e.target.value)}
          className="w-full p-3 min-h-[120px] resize-none focus:outline-none"
          style={{
            fontWeight: isBold ? 'bold' : 'normal',
            fontStyle: isItalic ? 'italic' : 'normal'
          }}
        />
      </div>
    </div>
  )
}
\`\`\`

## 🔄 Making Fields Dynamic

### Step 1: Create Field Configuration

\`\`\`tsx
// lib/field-config.ts
export interface FieldConfig {
  id: string
  type: 'text' | 'textarea' | 'list' | 'color' | 'rich-text'
  label: string
  placeholder?: string
  required?: boolean
  section: 'personal' | 'professional' | 'creative'
}

export const customFields: FieldConfig[] = [
  {
    id: 'motto',
    type: 'text',
    label: 'Personal Motto',
    placeholder: 'Your life motto...',
    section: 'personal'
  },
  {
    id: 'favoriteTools',
    type: 'list',
    label: 'Favorite Tools',
    placeholder: 'Add a tool...',
    section: 'professional'
  }
]
\`\`\`

### Step 2: Dynamic Field Renderer

\`\`\`tsx
// components/ui/dynamic-field.tsx
import { FieldConfig } from '../../lib/field-config'
import { Input } from './input'
import { Textarea } from './textarea'
import { DynamicListInput } from './custom-input'
import { ColorPicker } from './color-picker'

interface DynamicFieldProps {
  config: FieldConfig
  value: any
  onChange: (value: any) => void
}

export function DynamicField({ config, value, onChange }: DynamicFieldProps) {
  switch (config.type) {
    case 'text':
      return (
        <Input
          placeholder={config.placeholder}
          value={value || ''}
          onChange={(e) => onChange(e.target.value)}
        />
      )
    
    case 'textarea':
      return (
        <Textarea
          placeholder={config.placeholder}
          value={value || ''}
          onChange={(e) => onChange(e.target.value)}
        />
      )
    
    case 'list':
      return (
        <DynamicListInput
          label={config.label}
          value={value || []}
          onChange={onChange}
          placeholder={config.placeholder}
        />
      )
    
    case 'color':
      return (
        <ColorPicker
          label={config.label}
          value={value || '#3B82F6'}
          onChange={onChange}
        />
      )
    
    default:
      return <Input value={value || ''} onChange={(e) => onChange(e.target.value)} />
  }
}
\`\`\`

## 🎯 Integration Tips

1. **Always update the database schema** when adding new fields
2. **Update TypeScript interfaces** for type safety
3. **Test with existing templates** to ensure compatibility
4. **Add validation** for required fields
5. **Consider mobile UX** for complex inputs
