# Imagenity - AI-Powered Text Behind Image Tool

## Table of Contents
1. [Project Overview](#project-overview)
2. [Architecture & Tech Stack](#architecture--tech-stack)
3. [Core Features](#core-features)
4. [Data Models & Types](#data-models--types)
5. [Component Architecture](#component-architecture)
6. [State Management](#state-management)
7. [Image Processing System](#image-processing-system)
8. [Text Effects System](#text-effects-system)
9. [Layer Management](#layer-management)
10. [Download & Export System](#download--export-system)
11. [Authentication System](#authentication-system)
12. [Font Management](#font-management)
13. [File Structure](#file-structure)
14. [API Integration](#api-integration)
15. [Styling & UI](#styling--ui)
16. [Error Handling](#error-handling)
17. [Performance Optimizations](#performance-optimizations)
18. [Deployment & Environment](#deployment--environment)

---

## Project Overview

**Imagenity** is a sophisticated web application that uses AI to intelligently place text behind image subjects. It's designed for creating stunning visuals for social media, marketing materials, and creative projects. The application leverages Azure Computer Vision API for background removal and provides a comprehensive text editing and effects system.

### Key Capabilities
- **AI Background Removal**: Automatic detection and removal of image backgrounds
- **Advanced Text Editing**: Rich text controls with multiple effects
- **Layer Management**: Intuitive drag-and-drop layer system
- **High-Quality Export**: Multiple resolution and quality options
- **User Authentication**: Secure user management with Supabase
- **Social Sharing**: Direct link sharing capabilities

---

## Architecture & Tech Stack

### Frontend Framework
- **React 18.3.1** with TypeScript for type safety
- **Vite 5.4.2** for fast development and building
- **React Router DOM 6.22.3** for client-side routing

### State Management
- **Zustand 4.5.2** for lightweight state management
- **React Hooks** for component-level state

### Styling & UI
- **Tailwind CSS 3.4.1** for utility-first styling
- **Framer Motion 11.0.8** for smooth animations
- **Lucide React 0.344.0** for consistent icons

### Backend & Services
- **Supabase** for authentication and database
- **Azure Computer Vision API** for AI image processing
- **Google Fonts API** for font management

### Image Processing
- **html2canvas 1.4.1** for canvas capture and export
- **WebFont Loader 1.6.28** for dynamic font loading

### Development Tools
- **TypeScript 5.5.3** for type safety
- **ESLint 9.9.1** for code quality
- **PostCSS 8.4.35** for CSS processing

---

## Core Features

### 1. AI Image Processing
- **Background Removal**: Uses Azure Computer Vision API to automatically detect and remove backgrounds
- **High-Quality Processing**: Supports images up to 4MB with multiple quality options
- **Real-time Preview**: Immediate visual feedback during processing

### 2. Text Editing System
- **Rich Text Controls**: Bold, italic, underline formatting
- **Font Management**: Google Fonts integration with 1000+ fonts
- **Color Control**: Advanced color picker with eyedropper tool
- **Position Control**: Precise positioning with arrow key controls

### 3. Text Effects
- **Shadow Effects**: Customizable blur, color, and position
- **Neon Glow**: Intensity control with customizable colors
- **Gradient Text**: Multi-color gradients with angle control
- **Text Outline**: Width and color customization
- **Echo Effect**: Multiple echo layers with opacity control

### 4. Layer Management
- **Drag & Drop**: Intuitive layer positioning
- **Layer Controls**: Visibility toggle, reordering, position above/below foreground
- **Layer Types**: Background, foreground, text, and image layers
- **Selection System**: Click to select and edit layers

### 5. Export & Sharing
- **Multiple Resolutions**: HD (720p), Full HD (1080p), 2K, 4K
- **Quality Control**: Adjustable quality settings
- **Social Sharing**: Direct link sharing with public/private options
- **Download Options**: Various format and size options

---

## Data Models & Types

### Core Type Definitions

Based on the codebase analysis, the main types are imported from `../types` but the actual definitions appear to be inferred from usage. Here are the inferred type structures:

#### TextElement
```typescript
interface TextElement {
  id: string;                    // Unique identifier
  text: string;                  // Text content
  x: number;                     // X position
  y: number;                     // Y position
  fontSize: number;              // Font size in pixels
  color: string;                 // Text color (hex)
  fontFamily: string;            // Font family name
  isBold: boolean;               // Bold formatting
  isItalic: boolean;             // Italic formatting
  isUnderline: boolean;          // Underline formatting
  isVisible: boolean;            // Visibility toggle
  effects: TextEffects;          // Text effects object
}
```

#### ImageElement
```typescript
interface ImageElement {
  id: string;                    // Unique identifier
  url: string;                   // Image URL
  width: number;                 // Image width
  height: number;                // Image height
  x: number;                     // X position
  y: number;                     // Y position
  isVisible: boolean;            // Visibility toggle
}
```

#### ProcessedImage
```typescript
interface ProcessedImage {
  original: string;              // Original image URL
  foreground: string | null;     // Foreground image URL (after background removal)
}
```

#### Layer
```typescript
interface Layer {
  id: string;                    // Layer identifier
  type: 'background' | 'foreground' | 'text' | 'image';
  isVisible: boolean;            // Visibility toggle
  isAboveForeground?: boolean;   // Position relative to foreground
  data?: TextElement | ImageElement; // Layer data
}
```

#### TextEffects
```typescript
interface TextEffects {
  shadow?: {
    color: string;
    blur: number;
    offsetX: number;
    offsetY: number;
  };
  neon?: {
    color: string;
    blur: number;
    intensity: number;
  };
  outline?: {
    color: string;
    width: number;
  };
  gradient?: {
    colors: string[];
    angle: number;
  };
  echo?: {
    count: number;
    distance: number;
    opacity: number;
  };
}
```

---

## Component Architecture

### Main Application Structure

```
App.tsx
├── AppRouter (routes/index.tsx)
    ├── LandingPage
    ├── Editor (Main application)
    ├── ProfilePage
    ├── SharedImage
    ├── AuthCallback
    └── ResetPassword
```

### Editor Component Hierarchy

```
Editor.tsx (Main container)
├── Header.tsx
├── Left Sidebar (Upload & Tools)
│   ├── ImageUploadButton
│   └── Add Text/Image buttons
├── Canvas.tsx (Main work area)
│   ├── DraggableText.tsx
│   ├── DraggableImage.tsx
│   └── ImageLayers.tsx
└── Right Panel (Properties)
    ├── TextEditor.tsx
    ├── ImageEditor.tsx
    └── LayersPanel.tsx
        ├── LayersList.tsx
        └── LayerItem.tsx
```

### Key Components

#### 1. Editor.tsx
- **Purpose**: Main application container
- **Responsibilities**: 
  - Orchestrates image processing
  - Manages text and image selection
  - Coordinates between canvas and panels
  - Handles file uploads

#### 2. Canvas.tsx
- **Purpose**: Main work area for image editing
- **Responsibilities**:
  - Renders all layers in correct order
  - Handles canvas sizing and scaling
  - Manages drag and drop interactions
  - Provides selection feedback

#### 3. TextEditor.tsx
- **Purpose**: Text properties editing panel
- **Responsibilities**:
  - Text content editing
  - Font family and size controls
  - Text formatting (bold, italic, underline)
  - Position controls with arrow keys
  - Color picker integration

#### 4. DraggableText.tsx
- **Purpose**: Draggable text element
- **Responsibilities**:
  - Drag and drop functionality
  - Text rendering with effects
  - Selection state management
  - Position constraints

#### 5. LayersPanel.tsx
- **Purpose**: Layer management interface
- **Responsibilities**:
  - Layer visibility toggles
  - Layer reordering
  - Layer selection
  - Delete functionality

---

## State Management

### Zustand Store Structure

The application uses Zustand for global state management with the following store:

#### CanvasStore (stores/canvasStore.ts)
```typescript
interface CanvasState {
  image: ProcessedImage | null;
  layers: Layer[];
  
  // Actions
  setImage: (image: ProcessedImage | null) => void;
  addText: (text: TextElement) => void;
  updateText: (text: TextElement) => void;
  removeText: (id: string) => void;
  addImage: (image: ImageElement) => void;
  updateImage: (image: ImageElement) => void;
  removeImage: (id: string) => void;
  reorderLayers: (fromIndex: number, toIndex: number) => void;
  toggleLayerVisibility: (id: string) => void;
  toggleLayerPosition: (id: string) => void;
}
```

### State Management Features

1. **Layer Management**: Automatic layer ordering with background/foreground positioning
2. **Selection State**: Tracks selected text and image elements
3. **Visibility Control**: Individual layer visibility toggles
4. **Position Control**: Above/below foreground positioning
5. **Immutable Updates**: All state updates are immutable

### Custom Hooks

#### useImageProcessing
- Manages image upload and background removal
- Handles processing states and errors
- Provides cleanup functionality

#### useGoogleFonts
- Loads and manages Google Fonts
- Handles font loading states
- Provides fallback fonts

#### useAuth
- Manages user authentication state
- Handles sign up, sign in, and sign out
- Integrates with Supabase auth

---

## Image Processing System

### Background Removal Pipeline

1. **File Validation** (`utils/imageProcessing.ts`)
   - Validates file type (image only)
   - Checks file size (max 4MB)
   - Creates object URL for original image

2. **Azure API Integration** (`utils/azure/segmentation.ts`)
   - Converts file to ArrayBuffer
   - Sends to Azure Computer Vision API
   - Endpoint: `/vision/v4.0/analyze?visualFeatures=BackgroundRemoval`

3. **Response Processing**
   - Receives foreground image as blob
   - Creates object URL for foreground
   - Updates state with both images

### Error Handling

```typescript
class ImageProcessingError extends Error {
  constructor(message: string, public readonly code: string) {
    super(message);
    this.name = 'ImageProcessingError';
  }
}
```

### Processing States

- **isProcessing**: Boolean flag for loading state
- **error**: Error message display
- **processedImage**: Contains original and foreground URLs

---

## Text Effects System

### Effects Implementation

The text effects system uses CSS properties and generates styles dynamically:

#### Shadow Effect
```typescript
// Generates CSS text-shadow
textShadows.push(`${offsetX}px ${offsetY}px ${blur}px ${color}`);
```

#### Neon Effect
```typescript
// Creates multiple shadow layers for glow effect
for (let i = 1; i <= intensity; i++) {
  textShadows.push(`0 0 ${i * blur}px ${color}`);
}
```

#### Outline Effect
```typescript
// Uses webkit-text-stroke for outline
styles.WebkitTextStroke = `${width}px ${color}`;
```

#### Gradient Effect
```typescript
// Creates linear gradient background with text clipping
styles.backgroundImage = `linear-gradient(${angle}deg, ${colors.join(', ')})`;
styles.WebkitBackgroundClip = 'text';
styles.WebkitTextFillColor = 'transparent';
```

### Effects Panel (TextEffectsPanel.tsx)

- **Toggle Controls**: Enable/disable each effect
- **Color Pickers**: Custom color selection
- **Range Sliders**: Numeric value controls
- **Real-time Preview**: Immediate visual feedback

---

## Layer Management

### Layer Types

1. **Background**: Original uploaded image
2. **Foreground**: AI-processed foreground (background removed)
3. **Text**: User-added text elements
4. **Image**: User-added image overlays

### Layer Operations

#### Reordering
- Drag and drop reordering using Framer Motion
- Background layer is locked (cannot be reordered)
- Automatic z-index management

#### Visibility
- Individual layer visibility toggles
- Eye icon indicators
- Maintains layer order when hidden

#### Positioning
- Above/below foreground positioning
- Automatic insertion at correct z-index
- Visual feedback for layer position

### Layer Rendering

Layers are rendered in order with proper z-indexing:

```typescript
{layers.map((layer, index) => {
  if (!layer.isVisible) return null;
  
  switch (layer.type) {
    case 'background': // Render original image
    case 'text': // Render draggable text
    case 'image': // Render draggable image
    case 'foreground': // Render foreground image
  }
})}
```

---

## Download & Export System

### Export Pipeline

1. **Preparation** (`utils/downloadUtils.ts`)
   - Waits for fonts to load
   - Clones canvas container
   - Applies all styles and effects

2. **Canvas Capture** (html2canvas)
   - Captures DOM elements as canvas
   - Maintains positioning and styling
   - Handles CORS and cross-origin images

3. **Download Generation**
   - Creates download link
   - Generates filename with timestamp
   - Triggers browser download

### Resolution Options

```typescript
const resolutionOptions = [
  { label: 'HD (720p)', width: 1280, height: 720, quality: 0.8 },
  { label: 'Full HD (1080p)', width: 1920, height: 1080, quality: 0.9 },
  { label: '2K', width: 2560, height: 1440, quality: 0.95 },
  { label: '4K', width: 3840, height: 2160, quality: 1.0 }
];
```

### Text Positioning Fix

The system includes a text positioning fix for accurate export:

```typescript
// Adjusts text position for export accuracy
export function adjustTextPosition(y: number): number {
  return y - 2; // Small adjustment for pixel-perfect export
}
```

---

## Authentication System

### Supabase Integration

The authentication system uses Supabase for:

1. **User Registration**
   - Email/password signup
   - Profile creation with additional fields
   - Username and mobile number support

2. **User Login**
   - Email or username login
   - Secure session management
   - Password recovery

3. **Session Management**
   - Automatic session restoration
   - Real-time auth state changes
   - Secure logout

### Auth Hook (useAuth.ts)

```typescript
export function useAuth() {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  
  // Methods
  const signUp = async (data: SignUpData) => Promise<AuthResult>;
  const signIn = async (data: SignInData) => Promise<AuthResult>;
  const signOut = async () => Promise<void>;
}
```

### Protected Routes

```typescript
// ProtectedRoute component ensures authentication
<ProtectedRoute>
  <ProfilePage />
</ProtectedRoute>
```

---

## Font Management

### Google Fonts Integration

1. **Font Fetching** (`utils/fonts/api.ts`)
   - Fetches font list from Google Fonts API
   - Filters for web fonts only
   - Handles API rate limiting

2. **Font Loading** (`utils/fonts/loader.ts`)
   - Dynamic font loading with WebFont Loader
   - Fallback font management
   - Loading state handling

3. **Font Selection** (TextEditor.tsx)
   - Dropdown with all available fonts
   - Font preview in dropdown
   - Automatic font loading on selection

### Font Loading States

```typescript
export function useGoogleFonts() {
  const [fonts, setFonts] = useState<string[]>(getFallbackFonts());
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
}
```

### Fallback Fonts

```typescript
export function getFallbackFonts(): string[] {
  return [
    'Arial', 'Helvetica', 'Times New Roman', 'Georgia',
    'Verdana', 'Tahoma', 'Trebuchet MS', 'Impact'
  ];
}
```

---

## File Structure

```
src/
├── components/           # React components
│   ├── auth/            # Authentication components
│   ├── canvas/          # Canvas-related components
│   ├── download/        # Download and export components
│   ├── effects/         # Text effects components
│   ├── header/          # Header components
│   ├── image/           # Image editing components
│   ├── landing/         # Landing page components
│   ├── layers/          # Layer management components
│   ├── profile/         # User profile components
│   ├── sharing/         # Sharing components
│   └── ui/              # Reusable UI components
├── config/              # Configuration files
├── hooks/               # Custom React hooks
├── lib/                 # External library configurations
├── pages/               # Page components
├── routes/              # Routing configuration
├── stores/              # State management
├── styles/              # Global styles
├── types/               # TypeScript type definitions
├── utils/               # Utility functions
│   ├── azure/           # Azure API utilities
│   ├── canvas/          # Canvas utilities
│   ├── download/        # Download utilities
│   └── fonts/           # Font utilities
├── App.tsx              # Main app component
├── main.tsx             # Application entry point
└── index.css            # Global styles
```

---

## API Integration

### Azure Computer Vision API

#### Configuration
```typescript
export const azureConfig = {
  endpoint: import.meta.env.VITE_AZURE_CV_ENDPOINT,
  apiKey: import.meta.env.VITE_AZURE_CV_KEY
};
```

#### Background Removal Request
```typescript
const response = await fetch(endpoint, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/octet-stream',
    'Ocp-Apim-Subscription-Key': azureConfig.apiKey
  },
  body: imageData
});
```

### Google Fonts API

#### Font Fetching
```typescript
export async function fetchGoogleFonts(apiKey: string): Promise<string[]> {
  const response = await fetch(
    `https://www.googleapis.com/webfonts/v1/webfonts?key=${apiKey}&sort=popularity`
  );
  const data = await response.json();
  return data.items.map((font: GoogleFont) => font.family);
}
```

### Supabase Integration

#### Client Configuration
```typescript
import { createClient } from '@supabase/supabase-js';

export const supabase = createClient(
  import.meta.env.VITE_SUPABASE_URL,
  import.meta.env.VITE_SUPABASE_ANON_KEY
);
```

---

## Styling & UI

### Design System

#### Color Palette
```css
/* Dark theme colors */
--dark-50: #0a0a0a;
--dark-100: #1a1a1a;
--dark-200: #2a2a2a;
--dark-300: #3a3a3a;
--accent-primary: #00ff88;
```

#### Typography
- **Primary Font**: System fonts with Google Fonts fallback
- **Font Sizes**: 12px to 200px range
- **Default Size**: 24px

#### Spacing
- **Container Padding**: 1rem (16px)
- **Component Spacing**: 0.5rem to 2rem
- **Border Radius**: 0.5rem (8px)

### Responsive Design

- **Mobile First**: Responsive design approach
- **Breakpoints**: Tailwind CSS breakpoints
- **Flexible Layout**: Adaptive canvas sizing

### Animations

#### Framer Motion Integration
```typescript
<motion.div
  initial={{ opacity: 0, y: 10 }}
  animate={{ opacity: 1, y: 0 }}
  exit={{ opacity: 0, y: 10 }}
>
```

#### CSS Animations
```css
@keyframes pulse {
  0%, 100% { opacity: 0.8; filter: brightness(1); }
  50% { opacity: 1; filter: brightness(1.2); }
}
```

---

## Error Handling

### Error Types

1. **ImageProcessingError**: Custom error for image processing failures
2. **AzureApiError**: Azure API specific errors
3. **ValidationError**: Input validation errors
4. **NetworkError**: Network request failures

### Error Display

#### ErrorMessage Component
```typescript
export function ErrorMessage({ message, onClose }: ErrorMessageProps) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 50 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: 50 }}
      className="fixed bottom-4 right-4 bg-red-500 text-white p-4 rounded-lg"
    >
      {message}
    </motion.div>
  );
}
```

### Error Recovery

1. **Graceful Degradation**: Fallback to basic functionality
2. **User Feedback**: Clear error messages
3. **Retry Mechanisms**: Automatic retry for transient errors
4. **State Recovery**: Restore previous state on error

---

## Performance Optimizations

### React Optimizations

1. **Memoization**: React.memo for expensive components
2. **Callback Optimization**: useCallback for event handlers
3. **State Batching**: Efficient state updates
4. **Lazy Loading**: Code splitting for routes

### Image Optimizations

1. **Lazy Loading**: Images load on demand
2. **Object URL Management**: Proper cleanup of blob URLs
3. **Canvas Optimization**: Efficient canvas rendering
4. **Memory Management**: Automatic garbage collection

### Font Optimizations

1. **Font Loading**: Asynchronous font loading
2. **Font Caching**: Browser font cache utilization
3. **Fallback Fonts**: Immediate fallback display
4. **Font Subsetting**: Load only required characters

### Bundle Optimization

1. **Tree Shaking**: Remove unused code
2. **Code Splitting**: Split by routes and features
3. **Asset Optimization**: Compressed images and fonts
4. **CDN Usage**: External CDN for large assets

---

## Deployment & Environment

### Environment Variables

```env
# Supabase Configuration
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key

# Google Fonts API
VITE_GOOGLE_FONTS_API_KEY=your_google_fonts_api_key

# Azure Computer Vision
VITE_AZURE_CV_ENDPOINT=your_azure_endpoint
VITE_AZURE_CV_KEY=your_azure_key

# Azure Face API (if used)
VITE_AZURE_FACE_ENDPOINT=your_face_endpoint
VITE_AZURE_FACE_KEY=your_face_key
```

### Build Configuration

#### Vite Configuration (vite.config.ts)
```typescript
export default defineConfig({
  plugins: [react()],
  build: {
    outDir: 'dist',
    sourcemap: true,
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
          utils: ['html2canvas', 'framer-motion']
        }
      }
    }
  }
});
```

### Deployment Considerations

1. **Static Hosting**: Can be deployed to Vercel, Netlify, or similar
2. **CORS Configuration**: Proper CORS setup for API calls
3. **Environment Variables**: Secure environment variable management
4. **CDN Integration**: Content delivery network for assets
5. **SSL Certificate**: HTTPS for secure API calls

### Database Schema

#### User Profiles Table
```sql
CREATE TABLE user_profiles (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES auth.users(id),
  first_name TEXT,
  last_name TEXT,
  username TEXT UNIQUE,
  mobile_number TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

#### Shared Images Table
```sql
CREATE TABLE shared_images (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES auth.users(id),
  image_url TEXT,
  public_url TEXT UNIQUE,
  is_public BOOLEAN DEFAULT false,
  created_at TIMESTAMP DEFAULT NOW()
);
```

---

## Conclusion

Imagenity is a sophisticated web application that demonstrates modern web development practices with:

- **Advanced AI Integration**: Azure Computer Vision for background removal
- **Rich Text Editing**: Comprehensive text effects and formatting
- **Modern Architecture**: React with TypeScript and Zustand
- **Professional UI/UX**: Dark theme with smooth animations
- **Scalable Design**: Modular component architecture
- **Performance Focus**: Optimized rendering and loading
- **Security**: Proper authentication and data handling

The application serves as an excellent example of how to build a feature-rich, production-ready web application with modern technologies and best practices. 