# Baby Name Swiper - Product Requirements Document

**Version:** 1.0  
**Date:** December 31, 2025  
**Status:** POC (Proof of Concept)  
**Tech Stack:** Svelte + Vite + Tailwind CSS  
**Deployment:** Azure Static Web Apps (future)

---

## Problem Statement

Expectant parents face decision fatigue when browsing through hundreds or thousands of baby names. There's no fun, efficient way to quickly filter through large volumes of names and identify preferences without feeling overwhelmed.

---

## Core Features & Requirements

| Status | Feature | Description | Priority |
|--------|---------|-------------|----------|
| [ ] | **Swipe Interface** | Card-based swipe left (dismiss) / right (like) interaction | P0 |
| [ ] | **Name Display** | Show name, gender, origin, meaning on cards | P0 |
| [ ] | **Touch Gestures** | Support touch/mouse drag with visual feedback | P0 |
| [ ] | **Action Buttons** | Fallback X/❤️ buttons for non-swipe users | P0 |
| [ ] | **Favorites View** | Display all liked names in grid/list | P0 |
| [ ] | **Local Storage** | Persist liked/dismissed names and progress | P0 |
| [ ] | **Progress Counter** | Show "Name X of Y" indicator | P1 |
| [ ] | **Keyboard Support** | Arrow keys for accessibility | P1 |
| [ ] | **Mobile Responsive** | Works seamlessly on mobile & desktop | P0 |
| [ ] | **Card Stack Visual** | Show 2-3 cards layered for depth | P1 |
| [ ] | **Smooth Animations** | Card exit/entry transitions | P1 |
| [ ] | **Reset Function** | Clear all data and restart | P2 |
| [ ] | **End Screen** | "All names viewed" completion message | P2 |

---

## Implementation Checklist

### 📦 Phase 1: Project Setup
| Status | Task | Details |
|--------|------|---------|
| [ ] | Initialize Svelte + Vite project | `npm create vite@latest baby-name-swiper -- --template svelte` |
| [ ] | Install Tailwind CSS | Follow Tailwind + Vite setup guide |
| [ ] | Set up project structure | `/src/lib/components`, `/src/lib/stores`, `/src/lib/data` |
| [ ] | Create name dataset JSON | 200-500 names with id, name, gender, origin, meaning |
| [ ] | Configure build for static export | Vite config for Azure Static Web Apps |

### 🎨 Phase 2: Core UI Components
| Status | Component | Description |
|--------|-----------|-------------|
| [ ] | `NameCard.svelte` | Individual name card with name, gender, origin, meaning |
| [ ] | `SwipeContainer.svelte` | Container handling swipe gestures and animations |
| [ ] | `ActionButtons.svelte` | X and ❤️ buttons below card |
| [ ] | `ProgressBar.svelte` | Counter showing current position |
| [ ] | `FavoritesList.svelte` | Grid view of liked names |
| [ ] | `Navigation.svelte` | Header with Favorites link |

### 🔧 Phase 3: Core Functionality
| Status | Feature | Implementation |
|--------|---------|----------------|
| [ ] | Swipe gesture detection | Touch events + pointer events for desktop |
| [ ] | Card animation system | CSS transforms + transitions |
| [ ] | Name data management | Svelte store for current/remaining names |
| [ ] | Favorites store | Writable store synced to localStorage |
| [ ] | Progress tracking | Store current index, dismissed IDs |
| [ ] | Keyboard navigation | Event listeners for arrow keys |
| [ ] | localStorage sync | Auto-save on every action |

### 🎭 Phase 4: Polish & UX
| Status | Task | Details |
|--------|------|---------|
| [ ] | Add swipe visual feedback | Green/red overlay on drag |
| [ ] | Card tilt on drag | Rotate card based on drag distance |
| [ ] | Smooth card transitions | Exit animation + next card scale up |
| [ ] | Mobile-optimized touch | Prevent scroll during swipe |
| [ ] | Loading states | Handle JSON fetch gracefully |
| [ ] | Empty states | Messages for no favorites, all names done |
| [ ] | Responsive design | Test on mobile/tablet/desktop |

### 🧪 Phase 5: Testing & Deployment
| Status | Task | Details |
|--------|------|---------|
| [ ] | Test on iOS Safari | Verify touch gestures work |
| [ ] | Test on Chrome Mobile | Android compatibility |
| [ ] | Test on desktop browsers | Chrome, Firefox, Safari, Edge |
| [ ] | Test localStorage persistence | Close/reopen browser |
| [ ] | Test with 5 users | Collect feedback |
| [ ] | Performance check | Bundle size < 500KB, fast load |
| [ ] | Build for production | `npm run build` |
| [ ] | Deploy to local test | Test dist folder |
| [ ] | **(Future)** Azure Static Web App | Deploy with custom domain |

---

## Technical Specifications

### Tech Stack
| Layer | Technology | Purpose |
|-------|------------|---------|
| Framework | Svelte 4 | Reactive UI components |
| Build Tool | Vite | Fast dev server & bundling |
| Styling | Tailwind CSS | Utility-first styling |
| Gestures | Native touch/pointer events | No external library needed |
| Storage | localStorage API | Persist user data |
| Deployment | Azure Static Web Apps | Production hosting |

### Data Schema
```json
{
  "names": [
    {
      "id": 1,
      "name": "Emma",
      "gender": "Girl",
      "origin": "German",
      "meaning": "Universal, whole"
    }
  ]
}
```

### localStorage Keys
- `baby-swiper-favorites` - Array of liked name IDs
- `baby-swiper-dismissed` - Array of dismissed name IDs
- `baby-swiper-index` - Current position in name list

---

## UI/UX Specifications

### Color Palette
| Element | Color | Hex |
|---------|-------|-----|
| Like | Green | `#10B981` |
| Dismiss | Red | `#EF4444` |
| Background | Light Gray | `#F9FAFB` |
| Card | White | `#FFFFFF` |
| Text | Dark Gray | `#1F2937` |

### Interaction Patterns
| Action | Trigger | Result |
|--------|---------|--------|
| Swipe Right | Drag card right >100px | Add to favorites, show next |
| Swipe Left | Drag card left >100px | Dismiss, show next |
| Click ❤️ | Tap like button | Same as swipe right |
| Click ❌ | Tap dismiss button | Same as swipe left |
| Press → | Right arrow key | Like current name |
| Press ← | Left arrow key | Dismiss current name |

---

## Out of Scope (Future Phases)

| Feature | Phase | Notes |
|---------|-------|-------|
| User Authentication | Phase 2 | Login/accounts |
| Partner Sharing | Phase 2 | Compare favorites with partner |
| Backend API | Phase 2 | Sync across devices |
| Advanced Filters | Phase 2 | By origin, length, etc. |
| Undo/Redo | Phase 2 | Reverse last action |
| Name Pronunciation | Phase 3 | Audio playback |
| Analytics | Phase 3 | Usage tracking |
| Export Favorites | Phase 3 | PDF/CSV download |

---

## Success Criteria

| Metric | Target | Status |
|--------|--------|--------|
| User completes flow without help | 5/5 test users | [ ] |
| Works on iOS Safari | No critical bugs | [ ] |
| Works on Chrome Mobile | No critical bugs | [ ] |
| Data persists across sessions | 100% reliability | [ ] |
| First paint time | < 2 seconds | [ ] |
| Swipe response time | < 100ms | [ ] |
| Bundle size | < 500KB | [ ] |

---

## Resources

### Name Data Sources
- [SSA Baby Names Database](https://www.ssa.gov/oact/babynames/) - US popularity data
- [Behind the Name](https://www.behindthename.com/) - Etymology & meanings
- [Nameberry](https://nameberry.com/) - Curated lists & trends

### Design References
- Tinder - Card swipe mechanics
- Duolingo - Minimal lesson UI
- Headspace - Playful animations

---

## Next Steps
1. ✅ Review PRD
2. [ ] Follow QUICKSTART.md to set up project
3. [ ] Create initial name dataset (200 names)
4. [ ] Build core swipe interface
5. [ ] Deploy POC for testing
