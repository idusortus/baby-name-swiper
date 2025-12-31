# Copilot Instructions: Baby Name Swiper

## Project Overview
Baby Name Swiper is a Tinder-style swipe interface for discovering and saving baby names. Users can swipe right to like names and left to dismiss them.

**POC Constraints:**
- Static website only (no backend, no auth, no database)
- Client-side only with localStorage for persistence
- Future deployment: Azure Static Web Apps with custom domain

## Project Setup
- **Framework**: Svelte 4 + Vite
- **Styling**: Tailwind CSS
- **Gestures**: Native touch/pointer events (no external library)
- **Storage**: Browser localStorage API
- **Data Source**: Static JSON file at `src/lib/data/names.json`

## Architectural Patterns (To Be Established)
When building this application, follow these patterns:

### State Management
- Use Svelte stores for global state (liked names, dismissed names, user preferences)
- Keep swipe position and animation state local to components
- Implement persistent storage sync for liked/dismissed lists

### Component Structure
```
src/
├── routes/              # SvelteKit pages
│   ├── +page.svelte    # Main swipe interface
│   ├── favorites/      # Saved names view
│   └── settings/       # User preferences
├── lib/
│   ├── components/     # Reusable UI components
│   │   ├── NameCard.svelte    # Individual name display
│   │   └── SwipeContainer.svelte  # Swipe gesture handler
│   ├── stores/         # Svelte stores
│   └── data/           # Name database and utilities
```

### Swipe Mechanics
- Implement touch/mouse drag with velocity tracking
- Threshold-based decision: swipe distance + velocity determines action
- Smooth animations using CSS transforms and transitions
- Stack-based card rendering (show 2-3 cards in stack)

### Data Management
- Lazy load name batches to prevent loading entire dataset
- Implement Fisher-Yates shuffle for random name order
- Cache user decisions locally before potential API sync
- Handle edge cases: no more names, undo last swipe

## Development Workflow
```bash
# Install dependencies
npm install

# Start dev server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## Key Conventions
- **File Naming**: Use PascalCase for components (`NameCard.svelte`), kebab-case for routes
- **Props**: Destructure props with default values in component `<script>`
- **Events**: Use Svelte's event forwarding or createEventDispatcher
- **Styling**: Utility-first with Tailwind, component-specific styles in `<style>` blocks
- **TypeScript**: Use `.ts` for utilities, type component props with `$$Props` interface

## Testing Strategy
- Unit tests for data utilities (shuffle, filtering, storage)
- Component tests for NameCard and SwipeContainer
- E2E tests for complete swipe flow
- Test swipe gestures with different velocities and distances

## Performance Considerations
- Virtual scrolling for favorites list if >100 names
- Preload next 5-10 names while user swipes
- Debounce storage writes to avoid excessive I/O
- Optimize card animations with `will-change` CSS property

## External Dependencies (Typical)
- `svelte-gestures` or `@use-gesture/vanilla` - Swipe detection
- `idb` - IndexedDB wrapper for robust storage
- Name dataset - Consider name-data-api or custom curated JSON

## Common Pitfalls
- Don't block UI thread with synchronous storage operations
- Handle rapid swipes without animation queue buildup
- Prevent duplicate actions during animation transitions
- Account for different screen sizes and touch vs mouse input
