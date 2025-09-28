<script>
	import { onMount } from 'svelte';

	let mounted = false;
	let stars = [];
	let blurs = [];
	let gridBoxes = [];

	// Generate random positions for stars
	function generateStars(count) {
		return Array.from({ length: count }, (_, i) => ({
			id: i,
			left: Math.random() * 100,
			top: Math.random() * 100,
			size: Math.random() * 3 + 1,
			duration: Math.random() * 3 + 2,
			delay: Math.random() * 2
		}));
	}

	// Generate well-distributed blurs using grid-based positioning
	function generateBlurs(count) {
		const vibrantColors = [
			'bg-purple-500/30', 'bg-blue-500/25', 'bg-pink-500/28', 'bg-cyan-400/22',
			'bg-orange-500/26', 'bg-emerald-500/24', 'bg-red-500/25', 'bg-yellow-400/20',
			'bg-indigo-500/28', 'bg-teal-500/26', 'bg-rose-500/27', 'bg-violet-500/25',
			'bg-fuchsia-500/26', 'bg-lime-500/22', 'bg-amber-500/24', 'bg-sky-500/25'
		];

		// Create a 3x2 grid to ensure good distribution
		const gridCols = 3;
		const gridRows = 2;
		const cellWidth = 100 / gridCols;
		const cellHeight = 100 / gridRows;
		
		return Array.from({ length: count }, (_, i) => {
			const size = Math.random() * 200 + 200; // Size between 200-400px
			
			// Assign to grid cell, then add randomness within cell
			const col = i % gridCols;
			const row = Math.floor(i / gridCols) % gridRows;
			
			// Base position in grid cell (center of cell)
			const baseCellLeft = col * cellWidth + cellWidth / 2;
			const baseCellTop = row * cellHeight + cellHeight / 2;
			
			// Add randomness within the cell (smaller range to keep within bounds)
			const randomOffsetX = (Math.random() - 0.5) * (cellWidth * 0.6); // ±30% of cell width
			const randomOffsetY = (Math.random() - 0.5) * (cellHeight * 0.6); // ±30% of cell height
			
			const left = baseCellLeft + randomOffsetX;
			const top = baseCellTop + randomOffsetY;
			
			return {
				id: i,
				left: Math.max(15, Math.min(85, left)), // Keep within 15-85% range for better visibility
				top: Math.max(15, Math.min(85, top)),   // Keep within 15-85% range for better visibility
				size: size,
				color: vibrantColors[Math.floor(Math.random() * vibrantColors.length)],
				duration: Math.random() * 4 + 3, // 3-7 seconds
				delay: Math.random() * 3
			};
		});
	}

	// Generate structured grid boxes for enhanced readability and visual interest
	function generateGridBoxes(count) {
		const gridPositions = [
			// Top row - smaller decorative boxes
			{ x: 20, y: 25, w: 100, h: 60, type: 'small' },
			{ x: 80, y: 20, w: 120, h: 80, type: 'small' },
			// Middle row - larger backdrop boxes
			{ x: 15, y: 45, w: 180, h: 100, type: 'backdrop' },
			{ x: 85, y: 50, w: 160, h: 90, type: 'backdrop' },
			// Center area - main text support
			{ x: 50, y: 40, w: 300, h: 140, type: 'main' },
			// Bottom row - accent boxes
			{ x: 25, y: 75, w: 140, h: 70, type: 'accent' },
			{ x: 75, y: 80, w: 110, h: 85, type: 'accent' },
			// Additional floating elements
			{ x: 35, y: 65, w: 90, h: 50, type: 'float' },
			{ x: 65, y: 30, w: 80, h: 60, type: 'float' }
		];

		return gridPositions.slice(0, count).map((pos, i) => {
			const morphIntensity = pos.type === 'main' ? 0.3 : 0.7; // Less morphing for main backdrop
			const glowIntensity = pos.type === 'main' ? 0.6 : 0.3 + Math.random() * 0.4;
			const baseOpacity = {
				'main': 0.25,
				'backdrop': 0.18,
				'small': 0.12,
				'accent': 0.15,
				'float': 0.08
			}[pos.type] || 0.12;

			return {
				id: i,
				left: pos.x + (Math.random() - 0.5) * 5, // Small random offset
				top: pos.y + (Math.random() - 0.5) * 5,
				width: pos.w + Math.random() * 40 - 20,
				height: pos.h + Math.random() * 30 - 15,
				type: pos.type,
				rotation: (Math.random() - 0.5) * 20, // ±10 degrees
				morphDuration: 6 + Math.random() * 8, // 6-14 seconds
				morphDelay: Math.random() * 4,
				morphIntensity: morphIntensity,
				glowIntensity: glowIntensity,
				borderRadius: 8 + Math.random() * 24,
				opacity: baseOpacity + Math.random() * 0.08,
				zIndex: pos.type === 'main' ? 5 : (pos.type === 'backdrop' ? 3 : 1)
			};
		});
	}

	onMount(() => {
		mounted = true;
		// Generate content only on client-side to prevent SSR/client mismatch
		stars = generateStars(50);
		blurs = generateBlurs(6);
		gridBoxes = generateGridBoxes(9);
	});
</script>

<div class="relative bg-black overflow-hidden ios-full-height">
	{#if mounted}
		<!-- Animated Background Blurs -->
		<div class="absolute inset-0 isolate">
			{#each blurs as blur (blur.id)}
				<div
					class="absolute {blur.color} rounded-full blur-3xl animate-pulse will-change-transform"
					style:left="{blur.left}%"
					style:top="{blur.top}%"
					style:width="{blur.size}px"
					style:height="{blur.size}px"
					style:animation-duration="{blur.duration}s"
					style:animation-delay="{blur.delay}s"
					style:transform="translate3d(-50%, -50%, 0)"
				></div>
			{/each}
		</div>

		<!-- Animated Stars Overlay -->
		<div class="absolute inset-0 pointer-events-none">
			{#each stars as star (star.id)}
				<div
					class="absolute rounded-full bg-white animate-ping"
					style:left="{star.left}%"
					style:top="{star.top}%"
					style:width="{star.size}px"
					style:height="{star.size}px"
					style:animation-duration="{star.duration}s"
					style:animation-delay="{star.delay}s"
					style:box-shadow="0 0 {star.size * 2}px rgba(255, 255, 255, 0.8)"
				></div>
			{/each}
		</div>

		<!-- Additional Glowing Stars with Different Animation -->
		<div class="absolute inset-0 pointer-events-none">
			{#each Array.from({ length: 30 }) as _, i}
				{@const left = Math.random() * 100}
				{@const top = Math.random() * 100}
				{@const size = Math.random() * 2 + 0.5}
				{@const duration = Math.random() * 4 + 3}
				{@const delay = Math.random() * 5}
				<div
					class="absolute rounded-full bg-gradient-to-r from-white to-blue-200 animate-pulse"
					style:left="{left}%"
					style:top="{top}%"
					style:width="{size}px"
					style:height="{size}px"
					style:animation-duration="{duration}s"
					style:animation-delay="{delay}s"
					style:box-shadow="0 0 10px rgba(255, 255, 255, 0.6)"
				></div>
			{/each}
		</div>

		<!-- Floating Particles -->
		<div class="absolute inset-0 pointer-events-none">
			{#each Array.from({ length: 20 }) as _, i}
				{@const left = Math.random() * 100}
				{@const top = Math.random() * 100}
				{@const size = Math.random() * 4 + 2}
				{@const duration = Math.random() * 6 + 4}
				{@const delay = Math.random() * 3}
				<div
					class="absolute rounded-full bg-gradient-to-r from-purple-400/40 to-pink-400/40 animate-bounce"
					style:left="{left}%"
					style:top="{top}%"
					style:width="{size}px"
					style:height="{size}px"
					style:animation-duration="{duration}s"
					style:animation-delay="{delay}s"
					style:filter="blur(1px)"
				></div>
			{/each}
		</div>

		<!-- Dynamic Grid Boxes System -->
		<div class="absolute inset-0 pointer-events-none grid-container">
			{#each gridBoxes as box (box.id)}
				<div
					class="absolute grid-box grid-box-{box.type}"
					style:left="{box.left}%"
					style:top="{box.top}%"
					style:width="{box.width}px"
					style:height="{box.height}px"
					style:z-index="{box.zIndex}"
					style:transform="translate(-50%, -50%) rotate({box.rotation}deg)"
					style:animation-duration="{box.morphDuration}s"
					style:animation-delay="{box.morphDelay}s"
					style:border-radius="{box.borderRadius}px"
					style:--glow-intensity="{box.glowIntensity}"
					style:--box-opacity="{box.opacity}"
					style:--morph-intensity="{box.morphIntensity}"
					style:--base-rotation="{box.rotation}deg"
				></div>
			{/each}
		</div>
	{/if}

	<!-- Content Area -->
	<div class="relative z-20 flex items-center justify-center h-full">
		<div class="text-white text-center px-4 max-w-4xl mx-auto">
			<div class="relative">
				<h1 class="text-4xl sm:text-6xl lg:text-7xl font-bold mb-6 bg-gradient-to-r from-white via-blue-200 to-purple-200 bg-clip-text text-transparent leading-tight">
					Hey,<br>I'm <span class="chrome-name-overlay">tobezdev</span>.
				</h1>
			</div>
		</div>
    </div>
</div>

<style>
	@keyframes float {
		0%, 100% {
			transform: translateY(0px);
		}
		50% {
			transform: translateY(-20px);
		}
	}

	/* Mobile-specific fixes */
	:global(html, body) {
		height: 100%;
		margin: 0;
		padding: 0;
		overflow: hidden;
		background: black;
		-webkit-overflow-scrolling: touch;
	}

	/* iOS Safari bottom bar fix */
	.ios-full-height {
		height: 100vh;
		height: 100dvh;
		/* Force extension beyond viewport to eliminate bottom bar */
		min-height: 100vh;
		min-height: calc(100vh + env(safe-area-inset-bottom));
	}

	/* Fix mobile blur rendering artifacts */
	:global(*) {
		-webkit-backface-visibility: hidden;
		-moz-backface-visibility: hidden;
		-ms-backface-visibility: hidden;
		backface-visibility: hidden;
	}

	/* Alternative approach for stubborn iOS Safari cases */
	@supports (height: 100dvh) {
		.ios-full-height {
			height: 100dvh;
			/* Extend slightly beyond to cover Safari UI */
			min-height: calc(100dvh + 20px);
			margin-bottom: -20px;
		}
	}

	/* Ensure content extends into safe areas */
	@supports (padding: max(0px)) {
		.ios-full-height {
			/* Extend into safe areas */
			padding-top: env(safe-area-inset-top);
			padding-bottom: env(safe-area-inset-bottom);
			margin-top: calc(-1 * env(safe-area-inset-top));
			margin-bottom: calc(-1 * env(safe-area-inset-bottom));
		}
	}

	/* Force full coverage on iOS devices */
	@media screen and (max-width: 768px) {
		.ios-full-height {
			height: calc(100vh + 100px);
			margin-bottom: -100px;
		}
		
		@supports (height: 100dvh) {
			.ios-full-height {
				height: calc(100dvh + 50px);
				margin-bottom: -50px;
			}
		}
	}

    span.chrome-name-overlay {
        position: relative;
        display: inline-block;
        background: linear-gradient(90deg, #ff0057, #fffa00, #00ff85, #00cfff, #a700ff, #ff0057);
        background-size: 200% 200%;
        background-clip: text;
        -webkit-background-clip: text;
        color: transparent;
        animation: chroma-rainbow 5s linear infinite;

        }

        span.chrome-name-overlay::before,
        span.chrome-name-overlay::after {
            content: '';
            position: absolute;
            left: 0; top: 0; right: 0; bottom: 0;
            z-index: -1;
            background: inherit;
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            opacity: 0.5;
            pointer-events: none;
        }

		@keyframes chroma-rainbow {
			0% { background-position: 0% 50%; }
			50% { background-position: 100% 50%; }
			100% { background-position: 0% 50%; }
		}

</style>
