<script>
	import { page } from '$app/stores';
	
	let { activeSection = 'home' } = $props();
	
	const navItems = [
		{ name: 'Home', href: '#home', id: 'home' },
		{ name: 'About', href: '#about', id: 'about' },
		{ name: 'Projects', href: '#projects', id: 'projects' },
		{ name: 'Contact', href: '#contact', id: 'contact' }
	];
	
	// Function to check if item is active (based on current section in view)
	function isActive(item) {
		// For now, default to home being active
		// You can implement intersection observer later to detect which section is in view
		return item.id === 'home';
	}
</script>

<nav class="navbar">
	<div class="navbar-container">
		<div class="navbar-menu">
			{#each navItems as item}
				<a 
					href={item.href} 
					class="nav-item" 
					class:active={isActive(item)}
					data-section={item.id}
				>
					{item.name}
				</a>
			{/each}
		</div>
	</div>
</nav>

<style>
	.navbar {
		position: fixed;
		top: env(safe-area-inset-top, 0);
		left: 0;
		right: 0;
		z-index: 1000;
		
		/* iOS 26 style glassy background */
		background: rgba(0, 0, 0, 0.6);
		backdrop-filter: blur(20px) saturate(180%);
		-webkit-backdrop-filter: blur(20px) saturate(180%);
		
		/* Subtle border for definition */
		border-bottom: 1px solid rgba(255, 255, 255, 0.1);
		
		/* Smooth transitions */
		transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
		
		/* Ensure proper backdrop behavior */
		-webkit-backface-visibility: hidden;
		backface-visibility: hidden;
	}
	
	.navbar-container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0.75rem 1.5rem;
		display: flex;
		align-items: center;
		justify-content: center;
		height: 60px;
	}
	

	
	.navbar-menu {
		display: flex;
		align-items: center;
		gap: 2rem;
	}
	
	.nav-item {
		position: relative;
		text-decoration: none;
		color: white;
		font-weight: 500;
		font-size: 0.95rem;
		padding: 0.5rem 0;
		transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
		
		/* Subtle text shadow for better readability */
		text-shadow: 0 1px 2px rgba(0, 0, 0, 0.3);
	}
	
	/* iOS-style underline effect with chroma animation */
	.nav-item::after {
		content: '';
		position: absolute;
		bottom: -2px;
		left: 0;
		right: 0;
		height: 2px;
		background: linear-gradient(
			90deg,
			#ff0057,
			#fffa00,
			#00ff85,
			#00cfff,
			#a700ff,
			#ff0057
		);
		background-size: 200% 200%;
		border-radius: 1px;
		transform: scaleX(0);
		transform-origin: center;
		transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
		animation: chroma-rainbow 5s linear infinite;
	}
	
	.nav-item:hover {
		color: rgba(255, 255, 255, 0.9);
		transform: translateY(-1px);
		text-shadow: 0 2px 8px rgba(255, 255, 255, 0.2);
	}
	
	.nav-item:hover::after {
		transform: scaleX(1);
	}
	
	/* Active state styling */
	.nav-item.active {
		color: white;
		font-weight: 600;
		opacity: 1;
	}
	
	.nav-item.active::after {
		transform: scaleX(1);
		box-shadow: 0 0 10px rgba(255, 255, 255, 0.3);
	}
	
	/* Rainbow animation for brand */
	@keyframes chroma-rainbow {
		0% {
			background-position: 0% 50%;
		}
		50% {
			background-position: 100% 50%;
		}
		100% {
			background-position: 0% 50%;
		}
	}
	
	/* Responsive design */
	@media screen and (max-width: 768px) {
		.navbar-container {
			padding: 0.5rem 1rem;
			height: 56px;
		}
		
		.navbar-menu {
			gap: 1.5rem;
		}
		
		.nav-item {
			font-size: 0.9rem;
		}
	}
	
	@media screen and (max-width: 480px) {
		.navbar-container {
			padding: 0.5rem 0.75rem;
		}
		
		.navbar-menu {
			gap: 1rem;
		}
		
		.nav-item {
			font-size: 0.85rem;
			padding: 0.25rem 0;
		}
	}
	
	/* Enhance backdrop blur on supported browsers */
	@supports (backdrop-filter: blur(20px)) {
		.navbar {
			background: rgba(0, 0, 0, 0.4);
		}
	}
	
	/* Fallback for browsers without backdrop-filter */
	@supports not (backdrop-filter: blur(20px)) {
		.navbar {
			background: rgba(0, 0, 0, 0.85);
		}
	}
</style>