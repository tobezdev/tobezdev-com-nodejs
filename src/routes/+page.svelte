<svelte:head>
    <link
        href="https://fonts.googleapis.com/icon?family=Material+Icons"
        rel="stylesheet"
    >
</svelte:head>

<script>
import { onMount } from "svelte";

let mounted = false;
let stars = [];
let blurs = [];

// Generate random positions for stars
function generateStars(count) {
    return Array.from({ length: count }, (_, i) => ({
        id: i,
        left: Math.random() * 100,
        top: Math.random() * 100,
        size: Math.random() * 3 + 1,
        duration: Math.random() * 3 + 2,
        delay: Math.random() * 2,
    }));
}

// Generate well-distributed blurs using grid-based positioning
function generateBlurs(count) {
    const vibrantColors = [
        "bg-purple-500/30",
        "bg-blue-500/25",
        "bg-pink-500/28",
        "bg-cyan-400/22",
        "bg-orange-500/26",
        "bg-emerald-500/24",
        "bg-red-500/25",
        "bg-yellow-400/20",
        "bg-indigo-500/28",
        "bg-teal-500/26",
        "bg-rose-500/27",
        "bg-violet-500/25",
        "bg-fuchsia-500/26",
        "bg-lime-500/22",
        "bg-amber-500/24",
        "bg-sky-500/25",
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
            top: Math.max(15, Math.min(85, top)), // Keep within 15-85% range for better visibility
            size: size,
            color: vibrantColors[
                Math.floor(Math.random() * vibrantColors.length)
            ],
            duration: Math.random() * 4 + 3, // 3-7 seconds
            delay: Math.random() * 3,
        };
    });
}

onMount(() => {
    mounted = true;
    // Generate content only on client-side to prevent SSR/client mismatch
    stars = generateStars(50);
    blurs = generateBlurs(6);
});
</script>

<div class="relative bg-black overflow-hidden ios-full-height navbar-spacing">
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
                >
                </div>
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
                >
                </div>
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
                >
                </div>
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
                >
                </div>
            {/each}
        </div>
    {/if}

    <!-- Home Section -->
    <section id="home" class="section-container home-section">
        <div class="relative z-20 flex flex-col items-center justify-center h-full">
            <div class="text-white text-center px-4 max-w-4xl mx-auto">
                <div class="relative">
                    <h1
                        class="text-4xl sm:text-6xl lg:text-7xl font-bold mb-6 bg-gradient-to-r from-white via-blue-200 to-purple-200 bg-clip-text text-transparent leading-tight"
                    >
                        Hey,<br>I'm <span class="chrome-name-overlay"
                        >tobezdev</span>.
                    </h1>
                </div>
                <p class="text-lg text-white/80 mb-8 max-w-2xl">
                    Full-stack developer passionate about creating beautiful, functional web experiences
                    with modern technologies and cosmic-inspired design.
                </p>
            </div>

            <div class="info-grid-container">
                <div
                    class="info-grid-box info-grid-box-compact"
                    data-box-type="location"
                >
                    <div class="info-grid-glow"></div>
                    <div class="info-grid-header-with-icon">
                        <span class="material-icons info-card-icon">place</span>
                        <h3 class="info-grid-header">Location</h3>
                    </div>
                    <p class="info-grid-content">Hampshire, South England</p>
                </div>
                <div
                    class="info-grid-box info-grid-box-large"
                    data-box-type="languages"
                >
                    <div class="info-grid-glow"></div>
                    <div class="info-grid-header-with-icon">
                        <span class="material-icons info-card-icon">language</span>
                        <h3 class="info-grid-header">Languages</h3>
                    </div>
                    <p class="info-grid-content">
                        English [Native, <a
                            href="https://www.coe.int/en/web/common-european-framework-reference-languages/table-1-cefr-3.3-common-reference-levels-global-scale"
                            target="_blank"
                            class="info-link"
                        >CEFR Level C2+</a>],<br> German* [<a
                            href="https://www.coe.int/en/web/common-european-framework-reference-languages/table-1-cefr-3.3-common-reference-levels-global-scale"
                            target="_blank"
                            class="info-link"
                        >CEFR Level A1+</a>]
                    </p>
                    <p class="text-xs text-blue-200 mt-1 opacity-80">
                        *Learning native Swiss-German; <a
                            href="https://studyinginswitzerland.com/swiss-german-vs-german-differences/"
                            target="_blank"
                            class="info-link"
                        >see the difference</a>.
                    </p>
                </div>
                <div
                    class="info-grid-box info-grid-box-medium"
                    data-box-type="programming"
                >
                    <div class="info-grid-glow"></div>
                    <div class="info-grid-header-with-icon">
                        <span class="material-icons info-card-icon">code</span>
                        <h3 class="info-grid-header">Programming Experience</h3>
                    </div>
                    <p class="info-grid-content">JavaScript, TypeScript, Python</p>
                    <a
                        class="text-xs text-blue-200 mt-1 opacity-80 info-link"
                        href="#projects"
                    >
                        See my code in action.
                    </a>
                </div>
                <div
                    class="info-grid-box info-grid-box-small"
                    data-box-type="hobbies"
                >
                    <div class="info-grid-glow"></div>
                    <div class="info-grid-header-with-icon">
                        <span class="material-icons info-card-icon">interests</span>
                        <h3 class="info-grid-header">Interests</h3>
                    </div>
                    <ul class="info-grid-content">
                        <li>Tech</li>
                        <li>Gaming</li>
                        <li>Music Production</li>
                    </ul>
                </div>
            </div>
        </div>
    </section>

    <!-- About Section -->
    <section id="about" class="section-container about-section">
        <div class="section-content">
            <div class="section-header">
                <h2 class="section-title">About Me</h2>
                <p class="section-subtitle">Getting to know tobezdev</p>
            </div>
            
            <div class="about-grid">
                <div class="about-card">
                    <div class="about-card-icon">
                        <span class="material-icons">psychology</span>
                    </div>
                    <h3>Background</h3>
                    <p>Passionate developer with a love for creating innovative solutions and beautiful user experiences. Always learning and exploring new technologies.</p>
                </div>
                
                <div class="about-card">
                    <div class="about-card-icon">
                        <span class="material-icons">school</span>
                    </div>
                    <h3>Learning Journey</h3>
                    <p>Self-taught developer who believes in continuous learning. Currently expanding skills in modern web frameworks and cloud technologies.</p>
                </div>
                
                <div class="about-card">
                    <div class="about-card-icon">
                        <span class="material-icons">rocket_launch</span>
                    </div>
                    <h3>Mission</h3>
                    <p>To build applications that not only function well but also provide delightful user experiences through thoughtful design and clean code.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Projects Section -->
    <section id="projects" class="section-container projects-section">
        <div class="section-content">
            <div class="section-header">
                <h2 class="section-title">Projects</h2>
                <p class="section-subtitle">Some of my recent work</p>
            </div>
            
            <div class="projects-grid">
                <div class="project-card">
                    <div class="project-card-header">
                        <div class="project-icon">
                            <span class="material-icons">web</span>
                        </div>
                        <div class="project-links">
                            <a href="#" class="project-link">
                                <span class="material-icons">link</span>
                            </a>
                            <a href="#" class="project-link">
                                <span class="material-icons">code</span>
                            </a>
                        </div>
                    </div>
                    <h3>Portfolio Website</h3>
                    <p>A modern, responsive portfolio built with SvelteKit featuring glassmorphism design and smooth animations.</p>
                    <div class="project-tech">
                        <span class="tech-tag">SvelteKit</span>
                        <span class="tech-tag">TailwindCSS</span>
                        <span class="tech-tag">TypeScript</span>
                    </div>
                </div>
                
                <div class="project-card">
                    <div class="project-card-header">
                        <div class="project-icon">
                            <span class="material-icons">dashboard</span>
                        </div>
                        <div class="project-links">
                            <a href="#" class="project-link">
                                <span class="material-icons">link</span>
                            </a>
                            <a href="#" class="project-link">
                                <span class="material-icons">code</span>
                            </a>
                        </div>
                    </div>
                    <h3>Task Manager App</h3>
                    <p>A full-stack task management application with real-time updates and collaborative features.</p>
                    <div class="project-tech">
                        <span class="tech-tag">React</span>
                        <span class="tech-tag">Node.js</span>
                        <span class="tech-tag">MongoDB</span>
                    </div>
                </div>
                
                <div class="project-card">
                    <div class="project-card-header">
                        <div class="project-icon">
                            <span class="material-icons">analytics</span>
                        </div>
                        <div class="project-links">
                            <a href="#" class="project-link">
                                <span class="material-icons">link</span>
                            </a>
                            <a href="#" class="project-link">
                                <span class="material-icons">code</span>
                            </a>
                        </div>
                    </div>
                    <h3>Data Visualization Tool</h3>
                    <p>Interactive data visualization dashboard for analyzing complex datasets with beautiful charts and insights.</p>
                    <div class="project-tech">
                        <span class="tech-tag">Python</span>
                        <span class="tech-tag">D3.js</span>
                        <span class="tech-tag">FastAPI</span>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="section-container contact-section">
        <div class="section-content">
            <div class="section-header">
                <h2 class="section-title">Get In Touch</h2>
                <p class="section-subtitle">Let's work together</p>
            </div>
            
            <div class="contact-grid">
                <div class="contact-info">
                    <div class="contact-card">
                        <div class="contact-icon">
                            <span class="material-icons">email</span>
                        </div>
                        <h3>Email</h3>
                        <p>hello@tobezdev.com</p>
                    </div>
                    
                    <div class="contact-card">
                        <div class="contact-icon">
                            <span class="material-icons">code</span>
                        </div>
                        <h3>GitHub</h3>
                        <p>github.com/tobezdev</p>
                    </div>
                    
                    <div class="contact-card">
                        <div class="contact-icon">
                            <span class="material-icons">work</span>
                        </div>
                        <h3>LinkedIn</h3>
                        <p>linkedin.com/in/tobezdev</p>
                    </div>
                </div>
                
                <div class="contact-form">
                    <form class="glass-form">
                        <div class="form-group">
                            <label for="name">Name</label>
                            <input type="text" id="name" name="name" required />
                        </div>
                        
                        <div class="form-group">
                            <label for="email">Email</label>
                            <input type="email" id="email" name="email" required />
                        </div>
                        
                        <div class="form-group">
                            <label for="message">Message</label>
                            <textarea id="message" name="message" rows="5" required></textarea>
                        </div>
                        
                        <button type="submit" class="submit-btn">
                            <span class="material-icons">send</span>
                            Send Message
                        </button>
                    </form>
                </div>
            </div>
        </div>
    </section>
</div>

<style>
* {
    user-select: none;
}

@keyframes float {
    0%, 100% {
        transform: translateY(0px);
    }
    50% {
        transform: translateY(-20px);
    }
}

/* Global styles and smooth scrolling */
:global(html) {
    scroll-behavior: smooth;
}

:global(html, body) {
    margin: 0;
    padding: 0;
    background: black;
    -webkit-overflow-scrolling: touch;
    overflow-x: hidden;
}

/* iOS Safari bottom bar fix */
.ios-full-height {
    height: 100vh;
    height: 100dvh;
    /* Force extension beyond viewport to eliminate bottom bar */
    min-height: 100vh;
    min-height: calc(100vh + env(safe-area-inset-bottom));
}

/* Navbar spacing to prevent overlap */
.navbar-spacing {
    padding-top: 60px; /* Default navbar height */
}

@media screen and (max-width: 768px) {
    .navbar-spacing {
        padding-top: 56px; /* Mobile navbar height */
    }
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
    background-clip: text;
    -webkit-background-clip: text;
    color: transparent;
    animation: chroma-rainbow 5s linear infinite;
}

span.chrome-name-overlay::before,
span.chrome-name-overlay::after {
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    z-index: -1;
    background: inherit;
    background-clip: text;
    -webkit-background-clip: text;
    color: transparent;
    opacity: 0.5;
    pointer-events: none;
}

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

/* Info Grid Glassmorphism Animations */
@keyframes infoBoxMorph {
    0%, 100% {
        transform: scale(1) rotate(0deg) skew(0deg, 0deg);
        border-radius: 16px;
    }
    25% {
        transform: scale(1.02) rotate(0.5deg) skew(1deg, -0.5deg);
        border-radius: 20px;
    }
    50% {
        transform: scale(0.98) rotate(-0.3deg) skew(-0.5deg, 1deg);
        border-radius: 12px;
    }
    75% {
        transform: scale(1.01) rotate(0.7deg) skew(0.8deg, -0.3deg);
        border-radius: 18px;
    }
}

@keyframes infoGlowPulse {
    0%, 100% {
        box-shadow:
            0 0 20px rgba(59, 130, 246, 0.4),
            0 0 40px rgba(59, 130, 246, 0.2),
            inset 0 0 20px rgba(59, 130, 246, 0.1),
            0 1px 3px rgba(0, 0, 0, 0.3);
    }
    50% {
        box-shadow:
            0 0 30px rgba(59, 130, 246, 0.6),
            0 0 60px rgba(59, 130, 246, 0.3),
            inset 0 0 30px rgba(59, 130, 246, 0.15),
            0 2px 6px rgba(0, 0, 0, 0.4);
    }
}

@keyframes floatGentle {
    0%, 100% {
        transform: translateY(0px);
    }
    50% {
        transform: translateY(-8px);
    }
}

/* Info Grid Container */
.info-grid-container {
    display: flex;
    flex-wrap: wrap;
    gap: 1.5rem;
    margin-top: 3rem;
    padding: 0 1rem;
    max-width: 1200px;
    width: 100%;
    justify-content: center;
    align-items: flex-start;
}

/* Info Grid Box Base Styles */
.info-grid-box {
    position: relative;
    background: linear-gradient(
        135deg,
        rgba(59, 130, 246, 0.15) 0%,
        rgba(147, 51, 234, 0.12) 30%,
        rgba(59, 130, 246, 0.18) 70%,
        rgba(37, 99, 235, 0.10) 100%
    );
    backdrop-filter: blur(16px) saturate(200%);
    -webkit-backdrop-filter: blur(16px) saturate(200%);
    border: 1px solid rgba(59, 130, 246, 0.5);
    border-radius: 16px;
    padding: 1.5rem;
    overflow: hidden;
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    animation:
        infoBoxMorph 12s ease-in-out infinite,
        infoGlowPulse 8s ease-in-out infinite,
        floatGentle 6s ease-in-out infinite;
    will-change: transform, box-shadow, border-radius;
    width: fit-content;
    min-width: 250px;
}

/* Card Size Variations */
.info-grid-box-compact {
    min-width: 220px;
    max-width: 280px;
}

.info-grid-box-medium {
    min-width: 250px;
    max-width: 320px;
}

.info-grid-box-large {
    min-width: 300px;
    max-width: 400px;
}

.info-grid-box-small {
    min-width: 180px;
    max-width: 240px;
}

/* Header with Icon Styles */
.info-grid-header-with-icon {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    margin-bottom: 0.75rem;
}

.info-card-icon {
    font-size: 1.5rem;
    color: rgba(59, 130, 246, 0.8);
    text-shadow: 0 0 10px rgba(59, 130, 246, 0.4);
    position: relative;
    z-index: 2;
    transition: all 0.3s ease;
}

.info-grid-box::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(
        135deg,
        rgba(255, 255, 255, 0.1) 0%,
        transparent 50%,
        rgba(59, 130, 246, 0.05) 100%
    );
    border-radius: inherit;
    opacity: 0;
    transition: opacity 0.3s ease;
    pointer-events: none;
}

.info-grid-box:hover::before {
    opacity: 1;
}

/* Emissive Glow Layer */
.info-grid-glow {
    position: absolute;
    top: -2px;
    left: -2px;
    right: -2px;
    bottom: -2px;
    background: linear-gradient(
        135deg,
        rgba(59, 130, 246, 0.3),
        rgba(147, 51, 234, 0.2),
        rgba(59, 130, 246, 0.3)
    );
    border-radius: 18px;
    filter: blur(6px);
    opacity: 0.6;
    z-index: -1;
    animation: infoGlowPulse 8s ease-in-out infinite;
}

/* Box Type Variations */
.info-grid-box[data-box-type="location"] {
    animation-delay: 0s;
}

.info-grid-box[data-box-type="languages"] {
    animation-delay: 2s;
    background: linear-gradient(
        135deg,
        rgba(147, 51, 234, 0.15) 0%,
        rgba(59, 130, 246, 0.12) 50%,
        rgba(168, 85, 247, 0.18) 100%
    );
}

.info-grid-box[data-box-type="programming"] {
    animation-delay: 4s;
    background: linear-gradient(
        135deg,
        rgba(37, 99, 235, 0.15) 0%,
        rgba(59, 130, 246, 0.18) 50%,
        rgba(29, 78, 216, 0.12) 100%
    );
}

.info-grid-box[data-box-type="hobbies"] {
    animation-delay: 6s;
    background: linear-gradient(
        135deg,
        rgba(168, 85, 247, 0.15) 0%,
        rgba(147, 51, 234, 0.12) 50%,
        rgba(139, 92, 246, 0.18) 100%
    );
}

/* Text Styles */
.info-grid-header {
    font-size: 1.125rem;
    font-weight: 600;
    color: rgba(255, 255, 255, 0.95);
    margin: 0;
    text-shadow: 0 0 10px rgba(59, 130, 246, 0.3);
    position: relative;
    z-index: 2;
}

.info-grid-content {
    font-size: 0.9rem;
    color: rgba(255, 255, 255, 0.75);
    line-height: 1.6;
    position: relative;
    z-index: 2;
}

.info-link {
    color: rgba(147, 197, 253, 0.9);
    text-decoration: none;
    border-bottom: 1px solid rgba(147, 197, 253, 0.3);
    transition: all 0.3s ease;
}

.info-link:hover {
    color: rgba(147, 197, 253, 1);
    border-bottom-color: rgba(147, 197, 253, 0.8);
    text-shadow: 0 0 8px rgba(147, 197, 253, 0.5);
}

/* Hover Effects */
.info-grid-box:hover {
    transform: translateY(-8px) scale(1.02);
    border-color: rgba(59, 130, 246, 0.8);
    box-shadow:
        0 0 35px rgba(59, 130, 246, 0.6),
        0 0 70px rgba(59, 130, 246, 0.3),
        inset 0 0 35px rgba(59, 130, 246, 0.15),
        0 4px 20px rgba(0, 0, 0, 0.4);
}

.info-grid-box:hover .info-grid-glow {
    opacity: 1;
    filter: blur(8px);
}

.info-grid-box:hover .info-card-icon {
    color: rgba(59, 130, 246, 1);
    text-shadow: 0 0 15px rgba(59, 130, 246, 0.6);
    transform: scale(1.1);
}

/* Responsive Design */
@media screen and (max-width: 768px) {
    .info-grid-container {
        grid-template-columns: 1fr;
        gap: 1.5rem;
        margin-top: 2rem;
        padding: 0 0.5rem;
    }

    .info-grid-box-compact,
    .info-grid-box-medium,
    .info-grid-box-large,
    .info-grid-box-small {
        min-width: auto;
        max-width: none;
        width: 100%;
    }

    .info-grid-box {
        backdrop-filter: blur(12px) saturate(180%);
        -webkit-backdrop-filter: blur(12px) saturate(180%);
        padding: 1.25rem;
    }
}

@media screen and (max-width: 480px) {
    .info-grid-container {
        gap: 1rem;
    }

    .info-grid-box {
        backdrop-filter: blur(10px) saturate(160%);
        -webkit-backdrop-filter: blur(10px) saturate(160%);
        padding: 1rem;
    }

    .info-grid-header {
        font-size: 1rem;
    }

    .info-grid-content {
        font-size: 0.85rem;
    }
}

/* Section Layouts */
.section-container {
    min-height: 100vh;
    position: relative;
    z-index: 20;
}

.home-section {
    display: flex;
    align-items: center;
    justify-content: center;
}

.about-section, .projects-section, .contact-section {
    padding: 6rem 1rem 4rem;
    display: flex;
    align-items: center;
    justify-content: center;
}

.section-content {
    max-width: 1200px;
    width: 100%;
    margin: 0 auto;
}

.section-header {
    text-align: center;
    margin-bottom: 4rem;
}

.section-title {
    font-size: 3rem;
    font-weight: 700;
    color: white;
    margin-bottom: 1rem;
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
    background-clip: text;
    -webkit-background-clip: text;
    color: transparent;
    animation: chroma-rainbow 5s linear infinite;
}

.section-subtitle {
    font-size: 1.125rem;
    color: rgba(255, 255, 255, 0.7);
    margin: 0;
}

/* About Section Styles */
.about-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    margin-top: 3rem;
}

.about-card {
    background: linear-gradient(
        135deg,
        rgba(59, 130, 246, 0.15) 0%,
        rgba(147, 51, 234, 0.12) 50%,
        rgba(59, 130, 246, 0.18) 100%
    );
    backdrop-filter: blur(16px) saturate(200%);
    -webkit-backdrop-filter: blur(16px) saturate(200%);
    border: 1px solid rgba(59, 130, 246, 0.3);
    border-radius: 16px;
    padding: 2rem;
    text-align: center;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.about-card:hover {
    transform: translateY(-8px);
    border-color: rgba(59, 130, 246, 0.6);
    box-shadow: 0 20px 40px rgba(59, 130, 246, 0.2);
}

.about-card-icon {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 60px;
    height: 60px;
    background: linear-gradient(135deg, rgba(59, 130, 246, 0.3), rgba(147, 51, 234, 0.3));
    border-radius: 50%;
    margin-bottom: 1.5rem;
}

.about-card-icon .material-icons {
    font-size: 2rem;
    color: white;
}

.about-card h3 {
    font-size: 1.5rem;
    font-weight: 600;
    color: white;
    margin-bottom: 1rem;
}

.about-card p {
    color: rgba(255, 255, 255, 0.8);
    line-height: 1.6;
}

/* Projects Section Styles */
.projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
    gap: 2rem;
    margin-top: 3rem;
}

.project-card {
    background: linear-gradient(
        135deg,
        rgba(59, 130, 246, 0.15) 0%,
        rgba(147, 51, 234, 0.12) 50%,
        rgba(59, 130, 246, 0.18) 100%
    );
    backdrop-filter: blur(16px) saturate(200%);
    -webkit-backdrop-filter: blur(16px) saturate(200%);
    border: 1px solid rgba(59, 130, 246, 0.3);
    border-radius: 16px;
    padding: 2rem;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.project-card:hover {
    transform: translateY(-8px);
    border-color: rgba(59, 130, 246, 0.6);
    box-shadow: 0 20px 40px rgba(59, 130, 246, 0.2);
}

.project-card-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1.5rem;
}

.project-icon {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 50px;
    height: 50px;
    background: linear-gradient(135deg, rgba(59, 130, 246, 0.3), rgba(147, 51, 234, 0.3));
    border-radius: 12px;
}

.project-icon .material-icons {
    font-size: 1.5rem;
    color: white;
}

.project-links {
    display: flex;
    gap: 0.5rem;
}

.project-link {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 8px;
    color: white;
    text-decoration: none;
    transition: all 0.3s ease;
}

.project-link:hover {
    background: rgba(59, 130, 246, 0.3);
    transform: scale(1.1);
}

.project-card h3 {
    font-size: 1.25rem;
    font-weight: 600;
    color: white;
    margin-bottom: 0.75rem;
}

.project-card p {
    color: rgba(255, 255, 255, 0.8);
    line-height: 1.6;
    margin-bottom: 1.5rem;
}

.project-tech {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
}

.tech-tag {
    background: rgba(59, 130, 246, 0.2);
    color: white;
    padding: 0.25rem 0.75rem;
    border-radius: 20px;
    font-size: 0.875rem;
    border: 1px solid rgba(59, 130, 246, 0.3);
}

/* Contact Section Styles */
.contact-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    margin-top: 3rem;
}

.contact-info {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
}

.contact-card {
    background: linear-gradient(
        135deg,
        rgba(59, 130, 246, 0.15) 0%,
        rgba(147, 51, 234, 0.12) 50%,
        rgba(59, 130, 246, 0.18) 100%
    );
    backdrop-filter: blur(16px) saturate(200%);
    -webkit-backdrop-filter: blur(16px) saturate(200%);
    border: 1px solid rgba(59, 130, 246, 0.3);
    border-radius: 16px;
    padding: 1.5rem;
    display: flex;
    align-items: center;
    gap: 1rem;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.contact-card:hover {
    transform: translateX(8px);
    border-color: rgba(59, 130, 246, 0.6);
}

.contact-icon {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 50px;
    height: 50px;
    background: linear-gradient(135deg, rgba(59, 130, 246, 0.3), rgba(147, 51, 234, 0.3));
    border-radius: 12px;
    flex-shrink: 0;
}

.contact-icon .material-icons {
    font-size: 1.5rem;
    color: white;
}

.contact-card h3 {
    font-size: 1.125rem;
    font-weight: 600;
    color: white;
    margin: 0 0 0.25rem 0;
}

.contact-card p {
    color: rgba(255, 255, 255, 0.8);
    margin: 0;
}

/* Contact Form Styles */
.glass-form {
    background: linear-gradient(
        135deg,
        rgba(59, 130, 246, 0.15) 0%,
        rgba(147, 51, 234, 0.12) 50%,
        rgba(59, 130, 246, 0.18) 100%
    );
    backdrop-filter: blur(16px) saturate(200%);
    -webkit-backdrop-filter: blur(16px) saturate(200%);
    border: 1px solid rgba(59, 130, 246, 0.3);
    border-radius: 16px;
    padding: 2rem;
}

.form-group {
    margin-bottom: 1.5rem;
}

.form-group label {
    display: block;
    color: white;
    font-weight: 500;
    margin-bottom: 0.5rem;
}

.form-group input,
.form-group textarea {
    width: 100%;
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 8px;
    padding: 0.75rem;
    color: white;
    font-size: 1rem;
    transition: all 0.3s ease;
}

.form-group input:focus,
.form-group textarea:focus {
    outline: none;
    border-color: rgba(59, 130, 246, 0.6);
    box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}

.form-group input::placeholder,
.form-group textarea::placeholder {
    color: rgba(255, 255, 255, 0.5);
}

.submit-btn {
    background: linear-gradient(135deg, rgba(59, 130, 246, 0.8), rgba(147, 51, 234, 0.8));
    border: 1px solid rgba(59, 130, 246, 0.6);
    border-radius: 8px;
    padding: 0.75rem 1.5rem;
    color: white;
    font-weight: 600;
    font-size: 1rem;
    cursor: pointer;
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    justify-content: center;
    width: 100%;
}

.submit-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 10px 20px rgba(59, 130, 246, 0.3);
}

/* Responsive Design for Sections */
@media screen and (max-width: 768px) {
    .section-title {
        font-size: 2.5rem;
    }
    
    .about-grid,
    .projects-grid {
        grid-template-columns: 1fr;
        gap: 1.5rem;
    }
    
    .contact-grid {
        grid-template-columns: 1fr;
        gap: 2rem;
    }
    
    .about-section, .projects-section, .contact-section {
        padding: 4rem 1rem 3rem;
    }
}

@media screen and (max-width: 480px) {
    .section-title {
        font-size: 2rem;
    }
    
    .section-header {
        margin-bottom: 2rem;
    }
    
    .about-card,
    .project-card,
    .glass-form {
        padding: 1.5rem;
    }
}
</style>
