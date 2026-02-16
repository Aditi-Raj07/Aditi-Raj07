<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aditi | Frontend Engineer</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
    
    <style>
        /* ======================= RESET & VARIABLES ======================= */
        :root {
            --bg-color: #0d1117;
            --card-bg: rgba(22, 27, 34, 0.7);
            --glass-border: rgba(255, 255, 255, 0.1);
            --text-primary: #E6EDF3;
            --text-secondary: #9BA1A6;
            --accent-color: #58A6FF;
            --accent-glow: rgba(88, 166, 255, 0.4);
            --font-main: 'Inter', sans-serif;
            --transition-speed: 0.3s;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-primary);
            font-family: var(--font-main);
            overflow-x: hidden;
            line-height: 1.6;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: var(--bg-color);
        }
        ::-webkit-scrollbar-thumb {
            background: #30363d;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: var(--accent-color);
        }

        /* ======================= UTILITIES ======================= */
        .container {
            max-width: 1100px;
            margin: 0 auto;
            padding: 0 20px;
            position: relative;
            z-index: 2;
        }

        .section-title {
            font-size: 2rem;
            margin-bottom: 2rem;
            display: flex;
            align-items: center;
            gap: 10px;
            background: linear-gradient(90deg, var(--text-primary), var(--accent-color));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            opacity: 0; /* Animated in via JS */
            transform: translateY(20px);
            transition: all 0.6s ease-out;
        }

        .section-title.visible {
            opacity: 1;
            transform: translateY(0);
        }

        /* ======================= CANVAS BACKGROUND ======================= */
        #bg-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
            pointer-events: none;
        }

        /* ======================= HEADER / HERO ======================= */
        header {
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            position: relative;
        }

        .profile-badge {
            background: rgba(56, 139, 253, 0.15);
            color: var(--accent-color);
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 0.9rem;
            border: 1px solid var(--glass-border);
            margin-bottom: 20px;
            backdrop-filter: blur(5px);
            opacity: 0;
            animation: fadeInDown 1s ease forwards 0.5s;
        }

        .hero-title {
            font-size: 3.5rem;
            font-weight: 700;
            line-height: 1.2;
            margin-bottom: 1rem;
            min-height: 4.2rem; /* Prevent layout shift */
        }

        .typing-cursor {
            display: inline-block;
            width: 3px;
            background-color: var(--accent-color);
            animation: blink 1s step-end infinite;
            margin-left: 5px;
        }

        .hero-subtitle {
            font-size: 1.2rem;
            color: var(--text-secondary);
            max-width: 600px;
            margin-top: 1rem;
            opacity: 0;
            animation: fadeInUp 1s ease forwards 1s;
        }

        .scroll-indicator {
            position: absolute;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
            animation: bounce 2s infinite;
            opacity: 0.7;
        }

        /* ======================= GLASS CARD COMPONENT ======================= */
        .glass-card {
            background: var(--card-bg);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid var(--glass-border);
            border-radius: 16px;
            padding: 2rem;
            margin-bottom: 3rem;
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            opacity: 0; /* For scroll animation */
            transform: translateY(30px);
        }

        .glass-card.visible {
            opacity: 1;
            transform: translateY(0);
        }

        .glass-card:hover {
            box-shadow: 0 0 20px var(--accent-glow);
            border-color: rgba(88, 166, 255, 0.3);
        }

        /* ======================= ABOUT ======================= */
        .about-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 1.5rem;
        }

        .tag-list {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 1.5rem;
        }

        .tag {
            background: rgba(255, 255, 255, 0.05);
            padding: 6px 14px;
            border-radius: 8px;
            font-size: 0.9rem;
            border: 1px solid transparent;
            transition: all 0.3s;
        }

        .tag:hover {
            border-color: var(--accent-color);
            background: rgba(88, 166, 255, 0.1);
            color: var(--accent-color);
        }

        /* ======================= TECH STACK ======================= */
        .tech-grid {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 20px;
        }

        .tech-item {
            position: relative;
            background: rgba(22, 27, 34, 0.6);
            border: 1px solid var(--glass-border);
            border-radius: 12px;
            padding: 15px;
            width: 80px;
            height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            cursor: pointer;
        }

        .tech-item img {
            width: 100%;
            height: 100%;
            object-fit: contain;
            filter: grayscale(100%) brightness(0.8);
            transition: filter 0.3s;
        }

        .tech-item:hover {
            transform: translateY(-10px) scale(1.1);
            border-color: var(--accent-color);
            box-shadow: 0 10px 20px -5px var(--accent-glow);
        }

        .tech-item:hover img {
            filter: grayscale(0%) brightness(1.2);
        }

        /* ======================= STATS ======================= */
        .stats-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
        }

        .stat-wrapper {
            background: rgba(13, 17, 23, 0.5);
            border-radius: 12px;
            overflow: hidden;
            border: 1px solid var(--glass-border);
            position: relative;
        }

        .stat-wrapper img {
            width: 100%;
            height: auto;
            display: block;
            opacity: 0.9;
            transition: opacity 0.3s;
        }
        
        .stat-wrapper:hover img {
            opacity: 1;
        }

        /* ======================= FOCUS AREAS ======================= */
        .focus-list {
            list-style: none;
            display: grid;
            gap: 15px;
        }

        .focus-item {
            display: flex;
            align-items: center;
            padding: 15px;
            background: rgba(255, 255, 255, 0.03);
            border-radius: 8px;
            border-left: 3px solid var(--accent-color);
            transition: transform 0.2s;
        }

        .focus-item:hover {
            transform: translateX(10px);
            background: rgba(255, 255, 255, 0.05);
        }

        .focus-icon {
            margin-right: 15px;
            font-size: 1.2rem;
        }

        /* ======================= CONTACT ======================= */
        .contact-wrapper {
            display: flex;
            justify-content: center;
            gap: 20px;
            flex-wrap: wrap;
        }

        .contact-btn {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 12px 24px;
            border-radius: 8px;
            text-decoration: none;
            font-weight: 600;
            transition: all 0.3s;
            border: 1px solid var(--glass-border);
            background: rgba(255, 255, 255, 0.05);
            color: var(--text-primary);
            position: relative;
            overflow: hidden;
        }

        .contact-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.1), transparent);
            transition: left 0.5s;
        }

        .contact-btn:hover::before {
            left: 100%;
        }

        .contact-btn:hover {
            background: var(--accent-color);
            color: #fff;
            border-color: var(--accent-color);
            transform: translateY(-3px);
            box-shadow: 0 5px 15px var(--accent-glow);
        }

        /* ======================= FOOTER ======================= */
        footer {
            text-align: center;
            padding: 3rem 0;
            color: var(--text-secondary);
            font-size: 0.9rem;
            border-top: 1px solid var(--glass-border);
            margin-top: 4rem;
        }

        /* ======================= TOAST ======================= */
        #toast {
            visibility: hidden;
            min-width: 250px;
            background-color: var(--accent-color);
            color: #fff;
            text-align: center;
            border-radius: 8px;
            padding: 16px;
            position: fixed;
            z-index: 100;
            left: 50%;
            bottom: 30px;
            transform: translateX(-50%);
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
            font-weight: 600;
            opacity: 0;
            transition: opacity 0.3s, bottom 0.3s;
        }

        #toast.show {
            visibility: visible;
            opacity: 1;
            bottom: 50px;
        }

        /* ======================= ANIMATIONS ======================= */
        @keyframes blink {
            50% { opacity: 0; }
        }

        @keyframes fadeInDown {
            from { opacity: 0; transform: translateY(-20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translate(-50%, 0); }
            40% { transform: translate(-50%, -10px); }
            60% { transform: translate(-50%, -5px); }
        }

        /* Responsive */
        @media (max-width: 768px) {
            .hero-title { font-size: 2.2rem; }
            .tech-grid { gap: 15px; }
            .tech-item { width: 60px; height: 60px; }
            .stats-container { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>

    <!-- Canvas Background -->
    <canvas id="bg-canvas"></canvas>

    <!-- Toast Notification -->
    <div id="toast">Email copied to clipboard! 📋</div>

    <!-- Header / Hero -->
    <header>
        <div class="container">
            <div class="profile-badge">
                <img src="https://komarev.com/ghpvc/?username=Aditi-Raj07&label=Profile%20Views&color=1f6feb&style=flat" alt="Profile Views" />
            </div>
            
            <h1 class="hero-title">
                <span id="typing-text"></span><span class="typing-cursor"></span>
            </h1>
            
            <p class="hero-subtitle">
                Building Scalable, Performant, and Accessible Web Experiences.
            </p>

            <div class="scroll-indicator">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M7 13l5 5 5-5M7 6l5 5 5-5"/>
                </svg>
            </div>
        </div>
    </header>

    <main class="container">

        <!-- About Section -->
        <section id="about">
            <h2 class="section-title">👩‍💻 About Me</h2>
            <div class="glass-card tilt-card">
                <div class="about-grid">
                    <p>
                        Frontend Engineer passionate about building <strong>scalable, performant, and accessible</strong> web applications. 
                        Focused on clean component architecture, design systems, and seamless user experience.
                    </p>
                    <div class="tag-list">
                        <span class="tag">⚛️ React Ecosystem</span>
                        <span class="tag">🎨 UI/UX Design</span>
                        <span class="tag">🚀 Performance</span>
                        <span class="tag">🧠 Problem Solving</span>
                        <span class="tag">📈 Engineering Fundamentals</span>
                    </div>
                </div>
            </div>
        </section>

        <!-- Tech Stack Section -->
        <section id="stack">
            <h2 class="section-title">🧊 Tech Stack</h2>
            <div class="glass-card">
                <div class="tech-grid">
                    <!-- Using the icons from the user's request -->
                    <div class="tech-item"><img src="https://skillicons.dev/icons?i=html" alt="HTML"></div>
                    <div class="tech-item"><img src="https://skillicons.dev/icons?i=css" alt="CSS"></div>
                    <div class="tech-item"><img src="https://skillicons.dev/icons?i=js" alt="JavaScript"></div>
                    <div class="tech-item"><img src="https://skillicons.dev/icons?i=react" alt="React"></div>
                    <div class="tech-item"><img src="https://skillicons.dev/icons?i=tailwind" alt="Tailwind"></div>
                    <div class="tech-item"><img src="https://skillicons.dev/icons?i=typescript" alt="TypeScript"></div>
                    <div class="tech-item"><img src="https://skillicons.dev/icons?i=git" alt="Git"></div>
                    <div class="tech-item"><img src="https://skillicons.dev/icons?i=github" alt="GitHub"></div>
                    <div class="tech-item"><img src="https://skillicons.dev/icons?i=vscode" alt="VSCode"></div>
                    <div class="tech-item"><img src="https://skillicons.dev/icons?i=vercel" alt="Vercel"></div>
                </div>
            </div>
        </section>

        <!-- Stats Section -->
        <section id="stats">
            <h2 class="section-title">📊 Engineering Analytics</h2>
            <div class="stats-container">
                <div class="stat-wrapper tilt-card">
                    <img src="https://github-readme-stats.vercel.app/api?username=Aditi-Raj07&show_icons=true&theme=transparent&hide_border=true&title_color=E6EDF3&text_color=9BA1A6&icon_color=58A6FF" alt="GitHub Stats" />
                </div>
                <div class="stat-wrapper tilt-card">
                    <img src="https://github-readme-streak-stats.herokuapp.com/?user=Aditi-Raj07&theme=transparent&hide_border=true&ring=58A6FF&fire=58A6FF&currStreakLabel=E6EDF3&sideLabels=9BA1A6&dates=8B949E" alt="GitHub Streak" />
                </div>
                <div class="stat-wrapper tilt-card">
                    <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=Aditi-Raj07&layout=compact&theme=transparent&hide_border=true&title_color=E6EDF3&text_color=9BA1A6" alt="Top Languages" />
                </div>
            </div>
        </section>

        <!-- Focus Areas Section -->
        <section id="focus">
            <h2 class="section-title">🚀 Focus Areas</h2>
            <div class="glass-card">
                <ul class="focus-list">
                    <li class="focus-item">
                        <span class="focus-icon">🧩</span>
                        Component Architecture & Reusability
                    </li>
                    <li class="focus-item">
                        <span class="focus-icon">📱</span>
                        Responsive & Mobile-First Design
                    </li>
                    <li class="focus-item">
                        <span class="focus-icon">🗄️</span>
                        State Management Patterns
                    </li>
                    <li class="focus-item">
                        <span class="focus-icon">🔌</span>
                        API Integration & Async Handling
                    </li>
                    <li class="focus-item">
                        <span class="focus-icon">✨</span>
                        Clean Code & Maintainability
                    </li>
                </ul>
            </div>
        </section>

        <!-- Contact Section -->
        <section id="contact">
            <h2 class="section-title">📫 Connect</h2>
            <div class="glass-card" style="text-align: center;">
                <p style="margin-bottom: 2rem; color: var(--text-secondary);">Open to Impactful Opportunities</p>
                <div class="contact-wrapper">
                    <a href="mailto:aditih2006@gmail.com" class="contact-btn" id="email-btn">
                        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path><polyline points="22,6 12,13 2,6"></polyline></svg>
                        Send Email
                    </a>
                    <a href="https://linkedin.com/" target="_blank" class="contact-btn">
                        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"></path><rect x="2" y="9" width="4" height="12"></rect><circle cx="4" cy="4" r="2"></circle></svg>
                        LinkedIn
                    </a>
                </div>
            </div>
        </section>

    </main>

    <footer>
        <p><i>Engineered with precision. Built for impact.</i></p>
        <p style="margin-top: 10px; font-size: 0.8rem;">&copy; <span id="year"></span> Aditi. All rights reserved.</p>
    </footer>

    <script>
        // ======================= TYPEWRITER EFFECT =======================
        const typingText = document.getElementById('typing-text');
        const phrases = [
            "Hi 👋, I'm Aditi",
            "Frontend Engineer",
            "React | JavaScript | UI Architecture",
            "Building Scalable Web Experiences",
            "Open to Impactful Opportunities"
        ];
        
        let phraseIndex = 0;
        let charIndex = 0;
        let isDeleting = false;
        let typeSpeed = 100;

        function type() {
            const currentPhrase = phrases[phraseIndex];
            
            if (isDeleting) {
                typingText.textContent = currentPhrase.substring(0, charIndex - 1);
                charIndex--;
                typeSpeed = 50; // Faster deletion
            } else {
                typingText.textContent = currentPhrase.substring(0, charIndex + 1);
                charIndex++;
                typeSpeed = 100; // Normal typing
            }

            if (!isDeleting && charIndex === currentPhrase.length) {
                isDeleting = true;
                typeSpeed = 2000; // Pause at end
            } else if (isDeleting && charIndex === 0) {
                isDeleting = false;
                phraseIndex = (phraseIndex + 1) % phrases.length;
                typeSpeed = 500; // Pause before new word
            }

            setTimeout(type, typeSpeed);
        }

        // Start typing on load
        document.addEventListener('DOMContentLoaded', type);

        // ======================= SCROLL ANIMATIONS =======================
        const observerOptions = {
            threshold: 0.1,
            rootMargin: "0px 0px -50px 0px"
        };

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('visible');
                    observer.unobserve(entry.target); // Only animate once
                }
            });
        }, observerOptions);

        document.querySelectorAll('.section-title, .glass-card').forEach(el => {
            observer.observe(el);
        });

        // ======================= 3D TILT EFFECT (Vanilla JS) =======================
        document.querySelectorAll('.tilt-card').forEach(card => {
            card.addEventListener('mousemove', (e) => {
                const rect = card.getBoundingClientRect();
                const x = e.clientX - rect.left;
                const y = e.clientY - rect.top;
                
                const centerX = rect.width / 2;
                const centerY = rect.height / 2;
                
                const rotateX = ((y - centerY) / centerY) * -5; // Max -5deg rotation
                const rotateY = ((x - centerX) / centerX) * 5;

                card.style.transform = `perspective(1000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg) scale3d(1.02, 1.02, 1.02)`;
            });

            card.addEventListener('mouseleave', () => {
                card.style.transform = 'perspective(1000px) rotateX(0) rotateY(0) scale3d(1, 1, 1)';
            });
        });

        // ======================= COPY EMAIL TO CLIPBOARD =======================
        const emailBtn = document.getElementById('email-btn');
        const toast = document.getElementById('toast');

        emailBtn.addEventListener('click', (e) => {
            e.preventDefault();
            const email = "aditih2006@gmail.com";
            
            navigator.clipboard.writeText(email).then(() => {
                showToast();
            }).catch(err => {
                console.error('Failed to copy: ', err);
                // Fallback: allow default mailto behavior
                window.location.href = `mailto:${email}`;
            });
        });

        function showToast() {
            toast.className = "show";
            setTimeout(function(){ toast.className = toast.className.replace("show", ""); }, 3000);
        }

        // Set Copyright Year
        document.getElementById('year').textContent = new Date().getFullYear();

        // ======================= PARTICLE BACKGROUND =======================
        const canvas = document.getElementById('bg-canvas');
        const ctx = canvas.getContext('2d');
        
