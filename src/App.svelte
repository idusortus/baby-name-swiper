<script>
  import namesData from './lib/data/names.json';

  let allNames = namesData.names;
  let genderFilter = 'All';
  let names = allNames;
  let currentIndex = 0;
  
  // Physics-based swipe state
  let isDragging = false;
  let startX = 0;
  let startY = 0;
  let startTime = 0;
  let offsetX = 0;
  let offsetY = 0;
  let velocityX = 0;
  let lastX = 0;
  let lastTime = 0;
  let isAnimating = false;

  $: currentName = names.length > 0 && currentIndex < names.length ? names[currentIndex] : null;
  $: rotation = offsetX * 0.05; // Subtle rotation based on drag
  $: scale = isDragging ? 1.05 : 1; // Slight lift when dragging
  
  // Overlay opacity based on drag distance
  $: swipeProgress = Math.min(Math.abs(offsetX) / 120, 1);
  $: overlayOpacity = swipeProgress;
  
  // Background gradient based on gender filter
  $: backgroundClass = {
    'All': 'bg-gradient-to-br from-amber-50 via-rose-50 to-pink-50',
    'Girl': 'bg-gradient-to-br from-pink-50 via-purple-50 to-pink-100',
    'Boy': 'bg-gradient-to-br from-blue-50 via-cyan-50 to-blue-100'
  }[genderFilter];

  // Get upcoming cards for stack effect
  $: upcomingCards = names.slice(currentIndex + 1, currentIndex + 3);

  function setGenderFilter(filter) {
    genderFilter = filter;
    
    if (filter === 'All') {
      names = allNames;
    } else {
      names = allNames.filter(name => name.gender === filter);
    }
    
    currentIndex = 0;
    resetCard();
  }

  function handlePointerDown(e) {
    if (isAnimating || !currentName) return;
    
    isDragging = true;
    const clientX = e.clientX ?? e.touches?.[0]?.clientX;
    const clientY = e.clientY ?? e.touches?.[0]?.clientY;
    
    startX = clientX;
    startY = clientY;
    lastX = clientX;
    startTime = Date.now();
    lastTime = startTime;
    velocityX = 0;
  }

  function handlePointerMove(e) {
    if (!isDragging) return;
    
    const clientX = e.clientX ?? e.touches?.[0]?.clientX;
    const clientY = e.clientY ?? e.touches?.[0]?.clientY;
    const now = Date.now();
    
    offsetX = clientX - startX;
    offsetY = clientY - startY;
    
    // Calculate velocity for physics-based swipe detection
    const timeDelta = now - lastTime;
    if (timeDelta > 0) {
      velocityX = (clientX - lastX) / timeDelta;
    }
    
    lastX = clientX;
    lastTime = now;
  }

  function handlePointerUp() {
    if (!isDragging) return;
    isDragging = false;

    // Swipe detection based on velocity AND distance (like Tinder)
    const absVelocity = Math.abs(velocityX);
    const absDistance = Math.abs(offsetX);
    
    // Fast swipe (velocity > 0.5) OR far enough drag (distance > 120)
    if (absVelocity > 0.5 || absDistance > 120) {
      const direction = offsetX > 0 ? 'right' : 'left';
      animateSwipe(direction);
    } else {
      // Snap back with spring animation
      resetCard();
    }
  }

  function animateSwipe(direction) {
    isAnimating = true;
    const targetX = direction === 'right' ? 1000 : -1000;
    const targetRotation = direction === 'right' ? 30 : -30;
    
    // Apply exit animation
    offsetX = targetX;
    rotation = targetRotation;

    setTimeout(() => {
      currentIndex++;
      resetCard();
      isAnimating = false;
      
      // If we reach the end, loop back
      if (currentIndex >= names.length) {
        currentIndex = 0;
      }
    }, 400);
  }

  function resetCard() {
    offsetX = 0;
    offsetY = 0;
  }

  function handleSwipeButton(direction) {
    if (isAnimating || !currentName) return;
    animateSwipe(direction);
  }
</script>

<svelte:window
  on:pointermove={handlePointerMove}
  on:pointerup={handlePointerUp}
/>

<div class="min-h-screen {backgroundClass} transition-colors duration-700 flex flex-col">
  <!-- Header -->
  <header class="pt-8 pb-4 px-4">
    <h1 class="text-3xl font-bold text-center mb-1 text-gray-800">Baby Name Swiper</h1>
    <p class="text-center text-sm text-gray-600">Find the perfect name for your little one</p>
    
    <!-- Gender Filter -->
    <div class="flex gap-2 justify-center mt-4">
      <button 
        on:click={() => setGenderFilter('All')}
        class="px-5 py-2 rounded-full text-sm font-semibold transition-all duration-200 {
          genderFilter === 'All' 
            ? 'bg-purple-600 text-white shadow-lg scale-105' 
            : 'bg-white/80 text-gray-700 hover:bg-white hover:shadow-md'
        }">
        All Names
      </button>
      <button 
        on:click={() => setGenderFilter('Girl')}
        class="px-5 py-2 rounded-full text-sm font-semibold transition-all duration-200 {
          genderFilter === 'Girl' 
            ? 'bg-pink-600 text-white shadow-lg scale-105' 
            : 'bg-white/80 text-gray-700 hover:bg-white hover:shadow-md'
        }">
        Girls
      </button>
      <button 
        on:click={() => setGenderFilter('Boy')}
        class="px-5 py-2 rounded-full text-sm font-semibold transition-all duration-200 {
          genderFilter === 'Boy' 
            ? 'bg-blue-600 text-white shadow-lg scale-105' 
            : 'bg-white/80 text-gray-700 hover:bg-white hover:shadow-md'
        }">
        Boys
      </button>
    </div>
  </header>

  <!-- Card Stack Container -->
  <main class="flex-1 flex items-center justify-center px-4 pb-32">
    <div class="relative w-full max-w-sm" style="height: 550px;">
      
      <!-- Stack of upcoming cards -->
      {#each upcomingCards.slice(0, 2) as card, i}
        <div 
          class="absolute inset-0 bg-white rounded-3xl shadow-2xl pointer-events-none"
          style="
            transform: scale({1 - (i + 1) * 0.04}) translateY({(i + 1) * 10}px);
            opacity: {1 - (i + 1) * 0.25};
            z-index: {10 - i};
          "
        />
      {/each}

      <!-- Current card -->
      {#if currentName}
        <div
          class="absolute inset-0 bg-white rounded-3xl shadow-2xl overflow-hidden touch-none"
          style="
            transform: translateX({offsetX}px) translateY({offsetY}px) rotate({rotation}deg) scale({scale});
            transition: {isAnimating ? 'all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275)' : isDragging ? 'none' : 'all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275)'};
            cursor: {isDragging ? 'grabbing' : 'grab'};
            z-index: 20;
          "
          on:pointerdown={handlePointerDown}
          role="button"
          tabindex="0"
        >
          <!-- LOVE overlay (right swipe) -->
          <div 
            class="absolute top-10 left-10 px-6 py-3 border-[6px] border-green-500 text-green-500 text-3xl font-black rounded-2xl rotate-[20deg] pointer-events-none"
            style="opacity: {offsetX > 0 ? overlayOpacity : 0}; transform: scale({1 + overlayOpacity * 0.2});"
          >
            LOVE IT
          </div>

          <!-- PASS overlay (left swipe) -->
          <div 
            class="absolute top-10 right-10 px-6 py-3 border-[6px] border-red-500 text-red-500 text-3xl font-black rounded-2xl -rotate-[20deg] pointer-events-none"
            style="opacity: {offsetX < 0 ? overlayOpacity : 0}; transform: scale({1 + overlayOpacity * 0.2});"
          >
            PASS
          </div>

          <!-- Card content -->
          <div class="h-full flex flex-col items-center justify-center p-10 bg-gradient-to-b from-white to-gray-50">
            <div class="text-7xl mb-6" style="filter: drop-shadow(0 4px 12px rgba(0,0,0,0.1));">
              {currentName.gender === 'Girl' ? '👶🏻' : '👶🏼'}
            </div>
            
            <h2 class="text-6xl font-extrabold mb-3 text-gray-900 tracking-tight">
              {currentName.name}
            </h2>
            
            <p class="text-xl font-medium text-gray-500 mb-2">{currentName.origin}</p>
            
            <div class="max-w-xs text-center">
              <p class="text-base text-gray-600 italic leading-relaxed">
                "{currentName.meaning}"
              </p>
            </div>
            
            <div class="mt-6 px-4 py-2 bg-gradient-to-r from-purple-100 to-pink-100 rounded-full">
              <span class="text-sm font-semibold text-gray-700">
                {currentName.gender === 'Girl' ? '✨ Girl' : '⚡ Boy'} name
              </span>
            </div>
          </div>
        </div>
      {:else}
        <div class="absolute inset-0 flex items-center justify-center bg-white rounded-3xl shadow-2xl">
          <div class="text-center p-10">
            <div class="text-7xl mb-4">🎉</div>
            <h2 class="text-3xl font-bold text-gray-900 mb-2">All Done!</h2>
            <p class="text-gray-600">You've reviewed all names</p>
          </div>
        </div>
      {/if}
    </div>
  </main>

  <!-- Bottom actions -->
  <footer class="fixed bottom-0 left-0 right-0 pb-8 px-4 bg-gradient-to-t from-white/90 to-transparent backdrop-blur-sm">
    <div class="max-w-sm mx-auto">
      <!-- Progress indicator -->
      {#if currentName}
        <div class="text-center mb-4">
          <span class="text-sm font-medium text-gray-600">
            {currentIndex + 1} / {names.length}
          </span>
        </div>
      {/if}

      <!-- Action buttons -->
      <div class="flex items-center justify-center gap-6">
        <button 
          on:click={() => handleSwipeButton('left')}
          class="w-20 h-20 rounded-full bg-white shadow-xl hover:shadow-2xl transition-all duration-200 flex items-center justify-center text-4xl hover:scale-110 active:scale-95 border-4 border-red-100"
          disabled={isAnimating || !currentName}
        >
          <span style="filter: drop-shadow(0 2px 4px rgba(239, 68, 68, 0.3));">✕</span>
        </button>
        
        <button 
          on:click={() => handleSwipeButton('right')}
          class="w-20 h-20 rounded-full bg-gradient-to-br from-green-400 to-emerald-500 shadow-xl hover:shadow-2xl transition-all duration-200 flex items-center justify-center text-4xl hover:scale-110 active:scale-95"
          disabled={isAnimating || !currentName}
        >
          <span class="text-white" style="filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.2));">♥</span>
        </button>
      </div>
    </div>
  </footer>
</div>

<style>
  /* Prevent text selection during drag */
  .touch-none {
    -webkit-user-select: none;
    user-select: none;
    -webkit-touch-callout: none;
  }
</style>
