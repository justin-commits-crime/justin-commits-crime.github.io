
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Glass Construct</title>
    <style>
        /* 1. FIXED: Body must be transparent so video shows through */
        body, html { 
            margin: 0; padding: 0; 
            background-color: transparent; /* This was the problem */
            font-family: 'Courier New', monospace; 
            cursor: crosshair; 
        }

        /* 2. VIDEO: Fixed to back */
        #video-container {
            position: fixed; top: 0; left: 0; 
            width: 100%; height: 100%;
            z-index: -1; /* Sits behind everything */
            background: black; /* Fallback color */
        }
        video {
            width: 100%; height: 100%; 
            object-fit: cover;
            opacity: 1; /* Fully visible */
        }

        /* 3. SPACER: Forces scrollbar */
        #spacer { 
            height: 600vh; 
        } 

        /* 4. UI TEXT */
        .overlay {
            position: fixed; width: 100%; text-align: center; 
            color: white; mix-blend-mode: difference; 
            pointer-events: none; top: 50%;
            font-size: 3rem; letter-spacing: 8px; 
            transform: translateY(-50%);
            text-transform: uppercase;
            transition: opacity 0.5s ease;
        }
        
        .hint {
            position: fixed; bottom: 30px; width: 100%; text-align: center;
            color: rgba(255, 255, 255, 0.7); font-size: 0.8rem; 
            mix-blend-mode: exclusion;
        }

        /* 5. MOUSE TRAIL */
        .trail {
            position: fixed;
            width: 20px; height: 20px;
            border: 1px solid white;
            pointer-events: none;
            mix-blend-mode: overlay;
            transform: translate(-50%, -50%);
        }
    </style>
</head>
<body>

    <div id="video-container">
        <video id="v0" src="video.mp4" playsinline muted preload="auto"></video>
    </div>

    <div class="overlay" id="text">ISOLATION</div>
    <div class="hint">SCROLL TO MULTIPLY</div>

    <div id="spacer"></div>

    <script>
        const video = document.getElementById('v0');
        const text = document.getElementById('text');

        // Force video start
        video.currentTime = 0.1;

        // SCROLL ENGINE
        window.addEventListener('scroll', () => {
            const scrollPx = window.scrollY;
            const maxScroll = document.body.scrollHeight - window.innerHeight;
            const scrollPercent = scrollPx / maxScroll;
            
            if (video.duration) {
                video.currentTime = video.duration * scrollPercent;
            }

            // Text Switching
            if(scrollPercent < 0.2) {
                text.innerText = "ISOLATION";
                text.style.opacity = 1;
            } else if(scrollPercent < 0.5) {
                text.innerText = "BREAKOUT";
                text.style.opacity = 0.8;
            } else if(scrollPercent < 0.8) {
                text.innerText = "MULTIPLICATION";
                text.style.opacity = 1;
            } else {
                text.innerText = "CONFORMITY";
                text.style.opacity = 1;
            }
        });

        // MOUSE TRAIL ENGINE
        document.addEventListener('mousemove', (e) => {
            const square = document.createElement('div');
            square.classList.add('trail');
            document.body.appendChild(square);

            square.style.left = e.clientX + 'px';
            square.style.top = e.clientY + 'px';

            const size = Math.random() * 30 + 10;
            square.style.width = size + 'px';
            square.style.height = size + 'px';

            const animation = square.animate([
                { opacity: 1, transform: `translate(-50%, -50%) scale(1)` },
                { opacity: 0, transform: `translate(-50%, -50%) scale(0)` }
            ], {
                duration: 800,
                easing: 'ease-out'
            });

            animation.onfinish = () => square.remove();
        });
    </script>
</body>
</html>
