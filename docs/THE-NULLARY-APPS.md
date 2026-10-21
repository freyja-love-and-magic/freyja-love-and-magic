# The Nullary - Client Applications

The Nullary is a collection of minimal social media applications built on the allyabase infrastructure, providing user-facing interfaces for the Planet Nine ecosystem.

## Application Overview

**Location**: `/the-nullary/`

Collection of cross-platform desktop/mobile applications built with Tauri, featuring:
- **SVG-First Design**: Vector graphics for crisp, scalable interfaces
- **JSON Configuration**: All components configured via simple objects
- **Cross-App Sharing**: Shared component system across all applications
- **Real Service Integration**: No mock data - all apps connect to real allyabase services

## Core Applications

### **rhapsold** - Minimalist Blogging Platform ✅
**Location**: `/the-nullary/rhapsold/`

Flagship reference implementation showcasing complete SVG-first architecture.

**Features**:
- **Four-Screen Architecture**: Main (blog list), New Post (creation), Reading (consumption), Base (server management)
- **Layered UI System**: Multi-layer interface with HUD overlays and transparent scrolling zones
- **Sanora Integration**: Full blog management with file uploads and external URL support
- **Advanced UI Patterns**: Virtual scrolling, progress tracking, keyboard shortcuts, responsive design
- **Production-Ready Features**: Error handling, loading states, accessibility, theming

**Key Screens**:
```
Main Screen: Blog list with virtual scrolling and preview cards
New Post Screen: Dual-mode creation (hosted content vs external URLs)
Reading Screen: Immersive reading with auto-hiding controls
Base Screen: Universal server management (reusable across all apps)
```

### **ninefy** - Digital Goods Marketplace ✅
**Location**: `/the-nullary/ninefy/`

Complete marketplace for digital products with type-specific forms.

**Features**:
- **Product Types**: Ebooks, courses, tickets, shippable products
- **Dynamic Forms**: Type-specific creation forms with validation
- **Real Sanora Integration**: Live product management and discovery
- **Base Discovery**: BDO-based base discovery with intelligent caching
- **Environment Switching**: Support for dev/test/local environments

**Product Form Types**:
```javascript
const productTypes = {
    ebook: { title, author, description, price, file },
    course: { title, instructor, modules, duration, price },
    ticket: { event, date, venue, price, quantity },
    shippable: { title, description, price, weight, dimensions }
};
```

### **screenary** - Multi-Purpose Social App
**Location**: `/the-nullary/screenary/`

Comprehensive social media platform combining all content forms.

**Features**:
- **Universal Content Creation**: Text, images, videos, links
- **Feed Aggregation**: Content from multiple sources
- **Real-Time Updates**: Live content streaming
- **Base Management**: Complete base discovery and switching

### **mybase** - Personal Base Management
**Location**: `/the-nullary/mybase/`

Personal dashboard for managing Planet Nine base connections.

**Features**:
- **Base Discovery**: Find and connect to Planet Nine bases
- **Service Monitoring**: Health checks and status monitoring
- **Content Aggregation**: Pull content from multiple connected bases
- **Personal Dashboard**: Unified view of all your Planet Nine activity

### **stackchat** - Peer-to-Peer Messaging ✅
**Location**: `/the-nullary/stackchat/`

Secure messaging with cross-base communication.

**Features**:
- **P2P Messaging**: Direct peer-to-peer communication
- **Cross-Base Support**: Message users on different Planet Nine bases
- **Cryptographic Security**: End-to-end encryption using sessionless
- **Real-Time Delivery**: Instant message delivery and receipt confirmation

### **nexus** - Web-Based Ecosystem Portal ✅
**Location**: `/the-nullary/nexus/`

Central web portal demonstrating the complete Planet Nine ecosystem.

**Features**:
- **Four Main Portals**: Content & Social, Communications, Shopping, Base Discovery
- **SVG-First Design**: Following Nullary principles but for web browsers
- **Service Integration**: Real-time health monitoring of all Planet Nine services
- **Responsive Design**: Works on desktop and mobile
- **Express.js Backend**: Simple Node.js server with API proxy functionality

### Additional Applications

#### **idothis** - Task Management
Personal task management with Planet Nine integration.

#### **grocary** - Shopping Lists
Collaborative shopping lists with real-time updates.

#### **viewary** - Image Sharing
Photo sharing platform with privacy controls.

#### **lexary** - Text-Based Social
Minimalist text-focused social platform.

#### **photary** - Photo Management
Professional photo organization and sharing.

#### **blogary** - Simple Blogging
Basic blogging platform for quick thoughts.

#### **viewaris** - Short-Form Video
TikTok-like short video platform.

#### **eventary** - Event Management
Event creation and management platform.

#### **magicard** - Interactive Cards
Card-based interface for Planet Nine content.

## SVG Component Architecture ✅

**Location**: `/the-nullary/shared/`

### Core Principles
- **JSON Configuration First**: All components accept JSON config objects
- **SVG-Based UI Elements**: Vector graphics for crisp, scalable interfaces
- **Cross-App Compatibility**: Components work in all Nullary applications
- **Theme Integration**: Built-in responsive and accessibility features

### Component Structure

```
shared/
├── components/
│   ├── text.js          # Text rendering with typography
│   ├── forms.js         # SVG forms with HTML inputs
│   ├── layout.js        # Containers & positioning
│   ├── buttons.js       # Interactive buttons
│   ├── media.js         # Images, videos
│   ├── feed.js          # Scrollable feed component
│   ├── hud.js           # SVG HUD overlay system
│   └── decorative.js    # Borders, backgrounds
├── utils/
│   ├── svg-utils.js     # SVG creation helpers
│   ├── form-utils.js    # Form creation and handling
│   ├── post-creator.js  # Form-to-post conversion
│   ├── layered-ui.js    # Multi-layer UI management
│   ├── colors.js        # Color utilities
│   └── typography.js    # Font utilities
├── themes/
│   ├── default.js       # Default theme
│   ├── dark.js          # Dark theme
│   └── light.js         # Light theme
├── screens/
│   └── base-screen.js   # Universal base management
└── scaffolding/
    └── app-scaffolding.js # Complete app templates
```

### Component Usage Pattern

```javascript
import { createTextComponent } from './shared/components/text.js';

const config = {
    text: "Hello World",
    fontSize: 24,
    color: "#333",
    fontFamily: "Arial",
    width: 200,
    height: 60
};

const svgElement = createTextComponent(config);
document.body.appendChild(svgElement);
```

### Layered UI System ✅

Complex applications use a sophisticated layered architecture:

```javascript
// Create layered interface
const layeredUI = createLayeredUI({
    layers: [
        { id: 'background', type: 'div', zIndex: 1 },
        { id: 'feed', type: 'feed', zIndex: 100 },
        { id: 'hud', type: 'hud', zIndex: 1000 }
    ]
});

// Transparent scrolling zones
const hudConfig = {
    transparentZones: [{
        x: 20, y: 80,
        width: 'calc(100% - 40px)',
        height: 'calc(100% - 140px)',
        shape: 'rect'
    }]
};
```

### Screen Components

#### Base Screen ✅
**Purpose**: Universal screen component for managing base server connections.

```javascript
import { createBaseScreen } from '/shared/screens/base-screen.js';

const baseScreen = createBaseScreen({
    title: 'My Base Servers',
    theme: customTheme
});

await baseScreen.initialize(baseCommand);
```

**Features**:
- Interactive SVG feed showing connected bases
- Expandable rows with detailed information
- Join/Leave functionality with visual state changes
- Real-time updates and animated transitions

## Forms and Posts System ✅

### Form Components
Uses SVG `<foreignObject>` to embed HTML inputs:

```javascript
import { createBlogPostForm } from './shared/components/forms.js';

const blogForm = createBlogPostForm({
    width: 600,
    colors: theme.colors,
    fontFamily: theme.typography.fontFamily
});

// Handle form submission
blogForm.onSubmit(async (postConfig) => {
    const result = await createAndSavePost(blogForm, POST_TYPES.BLOG, theme, allyabaseClient);
});
```

### Form-to-Post Conversion
**Location**: `/shared/utils/post-creator.js`

```javascript
// Convert form data to SVG post components
const post = createPostFromForm(formData, POST_TYPES.BLOG, theme);

// Save to allyabase with offline fallbacks
const result = await savePostToAllyabase(post, allyabaseClient);

// Complete workflow automation
const result = await createAndSavePost(blogForm, POST_TYPES.BLOG, theme, allyabaseClient);
```

### Post Types Supported
- **TEXT**: Simple single-line text posts
- **MULTILINE**: Multi-line text with word wrapping
- **BLOG**: Title + content blog posts with custom typography

## Environment Configuration System ✅

### Three Environment Support
All applications support unified environment switching:

- **`dev`**: Production dev server (https://dev.*.allyabase.com)
- **`test`**: Local 3-base test ecosystem (localhost:5111-5122)
- **`local`**: Standard local development (localhost:3000-3008)

### Quick Environment Switching
Browser console commands for instant switching:

```javascript
// Switch any app to test ecosystem
ninefyEnv.switch('test')
rhapsoldEnv.switch('test')
screenaryEnv.switch('test')
lexaryEnv.switch('test')

// Check current environment
ninefyEnv.current()

// List available environments
ninefyEnv.list()
```

### Package Scripts
All apps include environment-specific npm scripts:

```bash
npm run dev:dev      # Dev server
npm run dev:test     # 3-base test ecosystem
npm run dev:local    # Local development

npm run build:dev    # Build for dev server
npm run build:test   # Build for test ecosystem
npm run build:local  # Build for local
```

### Programming API
```javascript
// Get current environment configuration
const config = getEnvironmentConfig();
console.log(config.env);        // 'dev', 'test', or 'local'
console.log(config.services);   // Service URLs object

// Get specific service URL
const sanoraUrl = getServiceUrl('sanora');
const bdoUrl = getServiceUrl('bdo');
```

## Development Patterns

### Tauri Development

#### JavaScript Module Loading
Direct file system module loading in Tauri:

```javascript
// ✅ Correct - Import from shared directory
import { createTextComponent } from '../../shared/components/text.js';
import { createLayeredUI } from '../../shared/utils/layered-ui.js';

// ✅ Correct - Local app modules
import { initializeApp } from './components/app.js';
```

#### HTML Setup for Tauri
```html
<!DOCTYPE html>
<html>
<head>
    <script type="module" src="main.js" defer></script>
</head>
<body>
    <div id="app"></div>
</body>
</html>
```

#### Development Workflow
1. **Run Tauri Dev**: `npm run tauri dev` (never open HTML directly)
2. **Import Paths**: Use `../../shared/` for shared components
3. **No HTTP Server**: Tauri handles file loading automatically
4. **Module Loading**: ES6 modules work with relative imports

### Component Development Guidelines

#### 1. JSON Configuration First
```javascript
export function createComponentName(config = {}) {
    const finalConfig = { ...DEFAULTS, ...config };
    // Component implementation
}
```

#### 2. SVG-Based UI Elements
```javascript
const svg = createSVGContainer(finalConfig);
const element = createSVGElement('elementType', attributes);
svg.appendChild(element);

// Add embedded styles
const css = generateCSS(finalConfig);
addSVGStyles(svg, css);
```

#### 3. Theme Integration
```javascript
// Support theme-based styling through JSON
const themedComponent = createComponent({
    ...baseConfig,
    colors: theme.colors,
    typography: theme.typography
});
```

### Shared Code First Policy ✅

**CRITICAL**: Before adding functionality to individual apps, always check shared code:

1. **Check Shared Components**: Look in `/the-nullary/shared/` first
2. **Update Master Files**: Enhance shared version before app-specific changes
3. **Sync to Apps**: Use `/shared/scripts/sync-shared-code.js` to distribute
4. **Avoid Duplication**: Never duplicate functionality that exists in shared code
5. **Consolidate First**: Move similar functionality into shared code

### Universal Spell System Integration ✅

All 16 nullary applications include universal spell casting:

```javascript
// loadSpellSystems() function in all app index.html files
function loadSpellSystems() {
    setTimeout(() => {
        if (typeof getServiceUrl === 'function') {
            // Load castSpell.js from fount service
            const fountUrl = getServiceUrl('fount');
            loadScript(`${fountUrl}/public/castSpell.js`, 'Spell system');

        }
    }, 100);
}
```

**Apps with Spell Integration**:
- rhapsold, ninefy, screenary, mybase, stackchat, nexus
- idothis, grocary, viewary, lexary, photary
- blogary, viewaris, eventary, magicard

## Production Readiness Patterns

### NO MOCK DATA Policy ✅
- **NEVER use mock, sample, or placeholder data** in any application
- All applications use real backend integration or graceful degradation
- Replace unavailable data with professional "no data yet" messages
- Use patterns like: "📦 No products yet - Products will appear here when added"

### Base Discovery Architecture ✅
- Use `dev.bdo.allyabase.com` as home base for discovery in development
- Follow screenary's base-command.js pattern for BDO-based base discovery
- Implement intelligent caching (10 minutes for bases, 5 minutes for products)
- Always provide graceful degradation when backend services unavailable

### API Evolution Best Practices ✅
- Update README documentation first with full examples
- Follow RESTful patterns (`/products/base` for all products from base)
- Provide comprehensive examples including cURL commands
- Document response formats clearly with JSON examples

## File Structure Pattern

```
app-name/
├── index.html           # Main HTML with Tauri setup
├── main.js             # Application entry point
├── package.json        # Dependencies and scripts
├── src-tauri/          # Tauri configuration
├── components/         # App-specific components
├── styles/            # App-specific styling
├── utils/             # App-specific utilities
└── README.md          # App documentation
```

## Testing Patterns

### Environment-Based Testing
```bash
# Test against 3-base ecosystem
cd the-nullary/ninefy/ninefy
npm run dev:test

# Test against dev servers
npm run dev:dev

# Test with local services
npm run dev:local
```

### Cross-App Testing
```bash
# Test shared component updates
cd the-nullary/shared/scripts
node sync-shared-code.js

# Test environment switching
# In browser console: appEnv.switch('test'); location.reload()
```

## Build and Deployment

### Build Scripts
```bash
# Build all applications
cd the-nullary
./build-em-all.sh

# Build specific app
cd app-name
npm run tauri build
```

### Cross-Platform Deployment
- **Desktop**: Windows, macOS, Linux via Tauri
- **Mobile**: iOS and Android via Tauri mobile
- **Web**: Browser deployment for apps like Nexus

The Nullary provides a comprehensive suite of applications demonstrating the full potential of the Planet Nine ecosystem, with shared architectural patterns enabling rapid development while maintaining consistency and quality across all platforms.