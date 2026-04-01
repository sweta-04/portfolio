/* =========================================================
   PREMIUM PORTFOLIO JS - Cursors, Tilt, Magnetics, GSAP-less
   ========================================================= */

// Linear Interpolation for smooth follow/easing
const lerp = (a, b, n) => (1 - n) * a + n * b;

document.addEventListener("DOMContentLoaded", () => {
    
    // 1. Custom Mouse Cursor Engine
    const cursor = document.getElementById('cursor');
    const outline = document.getElementById('cursor-follower');
    const bgGlow = document.getElementById('bg-glow');

    let mouseX = window.innerWidth / 2;
    let mouseY = window.innerHeight / 2;
    let outlineX = mouseX;
    let outlineY = mouseY;
    let glowX = mouseX;
    let glowY = mouseY;

    window.addEventListener('mousemove', (e) => {
        mouseX = e.clientX;
        mouseY = e.clientY;
        
        // Immediate update for dot
        if(cursor) {
            cursor.style.left = mouseX + 'px';
            cursor.style.top = mouseY + 'px';
        }
    });

    const render = () => {
        // Smooth follow for outline ring
        outlineX = lerp(outlineX, mouseX, 0.15);
        outlineY = lerp(outlineY, mouseY, 0.15);
        if(outline) {
            outline.style.left = outlineX + 'px';
            outline.style.top = outlineY + 'px';
        }

        // Smooth follow for background large glow orb, slowing it down drastically for dream-like feel
        glowX = lerp(glowX, mouseX, 0.05);
        glowY = lerp(glowY, mouseY, 0.05);
        if(bgGlow) {
            bgGlow.style.left = glowX + 'px';
            bgGlow.style.top = glowY + 'px';
        }

        requestAnimationFrame(render);
    };
    render();

    // Hover Effect triggers for Custom Cursor
    const hoverElements = document.querySelectorAll('a, button, .hover-trigger, .tilt-wrapper');
    hoverElements.forEach(el => {
        el.addEventListener('mouseenter', () => {
            outline.classList.add('hover-active');
        });
        el.addEventListener('mouseleave', () => {
            outline.classList.remove('hover-active');
        });
    });


    // 2. Magnetic Buttons Engine (Hover proximity physics)
    const magnetics = document.querySelectorAll('.magnetic, .magnetic-huge');
    magnetics.forEach(btn => {
        btn.addEventListener('mousemove', function(e) {
            const rect = this.getBoundingClientRect();
            const strength = this.getAttribute('data-magnetic-strength') || 20; 
            
            // Calculate center of the button relative to viewport
            const hx = rect.left + rect.width / 2;
            const hy = rect.top + rect.height / 2;
            
            // Mouse distance from center
            const dx = e.clientX - hx;
            const dy = e.clientY - hy;
            
            // Limit the movement
            const maxPx = parseInt(strength);
            const x = (dx / (rect.width / 2)) * maxPx;
            const y = (dy / (rect.height / 2)) * maxPx;

            this.style.transform = `translate(${x}px, ${y}px)`;
        });

        btn.addEventListener('mouseleave', function() {
             this.style.transform = 'translate(0px, 0px)';
        });
    });


    // 3. 3D Tilt Cards (Vanilla Tilt Math + Glare Effect)
    const cards = document.querySelectorAll('.tilt-wrapper');
    cards.forEach(card => {
        const tiltEl = card.querySelector('.tilt-element');
        const glare = card.querySelector('.glare');

        card.addEventListener('mousemove', (e) => {
            const rect = card.getBoundingClientRect();
            // X and Y coordinates inside the card container (0 to width/height)
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;
            
            // Map coordinates from [0, width] to [-1, 1]
            const xc = (x / rect.width) * 2 - 1;
            const yc = (y / rect.height) * 2 - 1;

            // Determine rotation degrees based on mouse position.
            // Move left/right rotates around Y, up/down rotates around X.
            // Multiplier defines how "deep / steep" the tilt is
            const maxTilt = 10; // degrees
            const rotateX = yc * -maxTilt;
            const rotateY = xc * maxTilt;

            // Update DOM Transform
            tiltEl.style.transform = `rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
            
            // Update Glare position
            if (glare) {
                const glareX = (x / rect.width) * 100;
                const glareY = (y / rect.height) * 100;
                glare.style.background = `radial-gradient(circle at ${glareX}% ${glareY}%, rgba(255,255,255,0.15) 0%, transparent 50%)`;
            }
        });

        card.addEventListener('mouseleave', () => {
            // Smoothly snap back on exit by CSS transition
            tiltEl.style.transform = `rotateX(0deg) rotateY(0deg)`;
            if(glare) {
                glare.style.background = `radial-gradient(circle at 50% 50%, rgba(255,255,255,0.1) 0%, transparent 60%)`;
            }
        });
    });


    // 4. Scroll Reveal via IntersectionObserver
    const observerOptions = {
        root: null,
        rootMargin: '0px 0px -10% 0px',
        threshold: 0.1
    };

    const scrollObserver = new IntersectionObserver((entries, observer) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                entry.target.classList.add('is-revealed');
                
                // If the observed element is a mask, reveal its inner lines
                if (entry.target.classList.contains('reveal-mask')) {
                    const lines = entry.target.querySelectorAll('.reveal-line');
                    lines.forEach(l => l.classList.add('is-revealed'));
                }
                
                // Optional: Unobserve after playing once
                observer.unobserve(entry.target);
            }
        });
    }, observerOptions);

    // Observe `.reveal-mask` rather than `.reveal-line` to avoid hidden/clipped intersections failing
    const revealElements = document.querySelectorAll('.reveal-mask, .reveal-up');
    revealElements.forEach(el => scrollObserver.observe(el));


    // 5. Custom Smooth Scroll hook for navbar / anchoring
    document.querySelectorAll('a[href^="#"]').forEach(anchor => {
        anchor.addEventListener('click', function(e) {
            e.preventDefault();
            const targetId = this.getAttribute('href');
            if(targetId === '#') return window.scrollTo({top: 0, behavior: 'smooth'});
            const targetElement = document.querySelector(targetId);
            if(targetElement) targetElement.scrollIntoView({ behavior: 'smooth' });
        });
    });

    // 6. Draggable CONNECT / RESUME Button
    const dragBtn = document.getElementById('draggable-btn');
    if(dragBtn) {
        // Dynamic Responsive Link & Text
        const dragBtnText = dragBtn.querySelector('.btn-text');
        function updateDragBtn() {
            if (window.innerWidth <= 768) {
                dragBtnText.textContent = 'RESUME';
                dragBtn.href = 'https://drive.google.com/file/d/1xeEq6y99IfP4BMzO_dISQSlqTVASq4LR/view?usp=sharing';
                dragBtn.target = '_blank';
            } else {
                dragBtnText.textContent = 'CONNECT';
                dragBtn.href = 'mailto:swetakumari0553@gmail.com';
                dragBtn.removeAttribute('target');
            }
        }
        updateDragBtn();
        window.addEventListener('resize', updateDragBtn);
        let isDragging = false;
        let hasDragged = false;
        let startX, startY, initialLeft, initialTop;

        dragBtn.addEventListener('mousedown', (e) => {
            isDragging = true;
            hasDragged = false;
            dragBtn.classList.add('is-dragging');
            e.preventDefault(); // Prevents native drag image of anchor
            
            startX = e.clientX;
            startY = e.clientY;
            
            const rect = dragBtn.getBoundingClientRect();
            initialLeft = rect.left;
            initialTop = rect.top;
            
            // Decouple from CSS 'right' and 'bottom' properties to use left/top for dragging
            dragBtn.style.right = 'auto';
            dragBtn.style.bottom = 'auto';
            dragBtn.style.left = initialLeft + 'px';
            dragBtn.style.top = initialTop + 'px';
        });

        window.addEventListener('mousemove', (e) => {
            if(!isDragging) return;
            
            const dx = e.clientX - startX;
            const dy = e.clientY - startY;
            
            // Consider it a drag if moved more than 5px
            if(Math.abs(dx) > 5 || Math.abs(dy) > 5) {
                hasDragged = true;
            }
            
            dragBtn.style.left = (initialLeft + dx) + 'px';
            dragBtn.style.top = (initialTop + dy) + 'px';
        });

        window.addEventListener('mouseup', () => {
            if(isDragging) {
                isDragging = false;
                dragBtn.classList.remove('is-dragging');
                // The style transform is reset by the removal of the magnetic logic when dragging,
                // but since we override magnetic transiton via css, it stays put.
            }
        });

        dragBtn.addEventListener('click', (e) => {
            // Prevent the mailto trigger if the user was just dragging the button
            if(hasDragged) {
                e.preventDefault();
            }
        });
    }
});
