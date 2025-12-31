# Baby Name Swiper - Quick Start Guide

Get up and running locally in **under 10 minutes**!

---

## Prerequisites

Ensure you have the following installed:
- **Node.js** 18+ ([download](https://nodejs.org/))
- **npm** or **pnpm** (comes with Node.js)
- **Git** ([download](https://git-scm.com/))
- **VS Code** (recommended editor)

Verify installation:
```bash
node --version   # Should show v18.x.x or higher
npm --version    # Should show 9.x.x or higher
```

---

## Step 1: Create Svelte + Vite Project

```bash
# Navigate to your workspace
cd "c:\dev\baby-swiper\baby-name-swiper"

# Create Svelte project with Vite
npm create vite@latest . -- --template svelte

# Install dependencies
npm install
```

**Choose these options when prompted:**
- Project name: `.` (current directory)
- Framework: `Svelte`
- Variant: `JavaScript` (or `TypeScript` if you prefer)

---

## Step 2: Install Tailwind CSS

```bash
# Install Tailwind and dependencies
npm install -D tailwindcss postcss autoprefixer

# Initialize Tailwind config
npx tailwindcss init -p
```

**Configure Tailwind** - Update `tailwind.config.js`:
```js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{svelte,js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

**Add Tailwind directives** - Create `src/app.css`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

**Import in main file** - Update `src/main.js`:
```js
import './app.css'
import App from './App.svelte'

const app = new App({
  target: document.getElementById('app'),
})

export default app
```

---

## Step 3: Set Up Project Structure

```bash
# Create directories
mkdir -p src/lib/components
mkdir -p src/lib/stores
mkdir -p src/lib/data
mkdir -p src/routes
```

**Your structure should look like:**
```
src/
├── lib/
│   ├── components/     # Svelte components
│   ├── stores/         # Svelte stores (state)
│   └── data/           # Name dataset JSON
├── routes/             # (Optional) Pages if using SvelteKit
├── app.css             # Tailwind imports
├── App.svelte          # Root component
└── main.js             # Entry point
```

---

## Step 4: Create Name Dataset

Create `src/lib/data/names.json` with sample data:

```json
{
  "names": [
    {
      "id": 1,
      "name": "Emma",
      "gender": "Girl",
      "origin": "German",
      "meaning": "Universal, whole. A classic name that has remained popular for decades."
    },
    {
      "id": 2,
      "name": "Liam",
      "gender": "Boy",
      "origin": "Irish",
      "meaning": "Strong-willed warrior and protector. Short form of William."
    },
    {
      "id": 3,
      "name": "Olivia",
      "gender": "Girl",
      "origin": "Latin",
      "meaning": "Olive tree, symbolizing peace and beauty."
    },
    {
      "id": 4,
      "name": "Noah",
      "gender": "Boy",
      "origin": "Hebrew",
      "meaning": "Rest and comfort. Biblical name with timeless appeal."
    },
    {
      "id": 5,
      "name": "Ava",
      "gender": "Girl",
      "origin": "Latin",
      "meaning": "Life or bird. Simple yet elegant."
    }
  ]
}
```

**Note:** Start with 5-10 names for testing, then expand to 200+ for production.

---

## Step 5: Start Development Server

```bash
# Run dev server
npm run dev
```

**You should see:**
```
  VITE v5.x.x  ready in xxx ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

Open **http://localhost:5173/** in your browser!

---

## Step 6: Build Your First Component

Replace `src/App.svelte` with a basic test:

```svelte
<script>
  import namesData from './lib/data/names.json';
  
  let names = namesData.names;
  let currentIndex = 0;
  let currentName = names[currentIndex];
  
  function handleLike() {
    console.log('Liked:', currentName.name);
    nextName();
  }
  
  function handleDismiss() {
    console.log('Dismissed:', currentName.name);
    nextName();
  }
  
  function nextName() {
    currentIndex++;
    if (currentIndex < names.length) {
      currentName = names[currentIndex];
    } else {
      currentName = null; // All names done
    }
  }
</script>

<main class="min-h-screen bg-gray-50 flex items-center justify-center p-4">
  <div class="max-w-md w-full">
    <h1 class="text-3xl font-bold text-center mb-8">Baby Name Swiper</h1>
    
    {#if currentName}
      <div class="bg-white rounded-2xl shadow-xl p-8 text-center">
        <h2 class="text-4xl font-bold mb-2">{currentName.name}</h2>
        <p class="text-gray-600 mb-1">{currentName.gender} • {currentName.origin}</p>
        <p class="text-gray-700 mt-4">{currentName.meaning}</p>
        
        <div class="flex gap-4 mt-8">
          <button 
            class="flex-1 bg-red-500 hover:bg-red-600 text-white py-3 px-6 rounded-lg font-semibold"
            on:click={handleDismiss}
          >
            ✕ Pass
          </button>
          <button 
            class="flex-1 bg-green-500 hover:bg-green-600 text-white py-3 px-6 rounded-lg font-semibold"
            on:click={handleLike}
          >
            ♥ Like
          </button>
        </div>
      </div>
      
      <p class="text-center text-gray-600 mt-4">
        Name {currentIndex + 1} of {names.length}
      </p>
    {:else}
      <div class="bg-white rounded-2xl shadow-xl p-8 text-center">
        <h2 class="text-2xl font-bold mb-4">All Done! 🎉</h2>
        <p class="text-gray-700">You've seen all the names.</p>
      </div>
    {/if}
  </div>
</main>
```

**Save and refresh** - you should see a working name card!

---

## Step 7: Test It Works

1. **Click "Like"** - Name should advance
2. **Click "Pass"** - Name should advance
3. **Check console** - Should see "Liked: Emma" messages
4. **Go through all names** - Should see "All Done" screen
5. **Test on mobile** - Open http://YOUR_IP:5173 on phone (same WiFi)

---

## Development Workflow

### Common Commands
```bash
# Start dev server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Format code (if using Prettier)
npm run format
```

### Hot Tips
- **Hot reload** works automatically - save files to see changes instantly
- **VS Code extensions**: Install "Svelte for VS Code" for syntax highlighting
- **Browser DevTools**: Press F12 to debug, check console for errors
- **Mobile testing**: Use `npm run dev -- --host` to access from phone

---

## Next Steps

### Implement Core Features (in order):
1. **Add swipe gestures** - Use pointer events for drag detection
2. **Create Svelte stores** - Manage favorites and dismissed names
3. **Add localStorage** - Persist state across sessions
4. **Build favorites view** - Show liked names in a grid
5. **Add animations** - Card transitions and visual feedback
6. **Make responsive** - Optimize for mobile screens

### File Roadmap
- `src/lib/stores/nameStore.js` - Current name state
- `src/lib/stores/favoritesStore.js` - Liked names
- `src/lib/components/NameCard.svelte` - Card UI
- `src/lib/components/SwipeContainer.svelte` - Gesture handling
- `src/lib/components/FavoritesList.svelte` - Favorites grid
- `src/lib/utils/storage.js` - localStorage helpers

---

## Troubleshooting

### Port already in use
```bash
# Kill process on port 5173
npx kill-port 5173
# Or use a different port
npm run dev -- --port 3000
```

### Tailwind styles not working
- Check `src/app.css` has Tailwind directives
- Verify `src/main.js` imports `./app.css`
- Check `tailwind.config.js` content paths

### JSON import not working
Add to `vite.config.js`:
```js
export default {
  assetsInclude: ['**/*.json'],
}
```

### Hot reload not working
- Hard refresh: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)
- Restart dev server: Ctrl+C then `npm run dev`

---

## Deployment (Future)

When ready to deploy to Azure Static Web Apps:

```bash
# Build production bundle
npm run build

# Output will be in dist/ folder
# Upload to Azure Static Web Apps via:
# - Azure Portal
# - GitHub Actions
# - Azure CLI
```

**Custom domain setup** will be configured in Azure portal after deployment.

---

## Resources

- **Svelte Docs**: https://svelte.dev/docs
- **Vite Guide**: https://vitejs.dev/guide/
- **Tailwind CSS**: https://tailwindcss.com/docs
- **Azure Static Web Apps**: https://docs.microsoft.com/azure/static-web-apps/

---

## Need Help?

Common issues and solutions are in the PRD.md troubleshooting section. Check there first!

**You're all set!** 🚀 Start building in `src/App.svelte`.