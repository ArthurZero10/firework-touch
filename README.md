# firework-touch
<!DOCTYPE html><html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fireworks Animation</title>
    <style>
        body { margin: 0; background: black; overflow: hidden; }
        canvas { display: block; }
    </style>
</head>
<body>
    <canvas id="fireworksCanvas"></canvas>
    <script>
        const canvas = document.getElementById("fireworksCanvas");
        const ctx = canvas.getContext("2d");
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;class Firework {
        constructor(x, y, color) {
            this.x = x;
            this.y = y;
            this.color = color;
            this.particles = [];
            this.createParticles();
        }

        createParticles() {
            const numParticles = 50;
            for (let i = 0; i < numParticles; i++) {
                const angle = (Math.PI * 2 * i) / numParticles;
                const speed = Math.random() * 4 + 2;
                this.particles.push({
                    x: this.x,
                    y: this.y,
                    vx: Math.cos(angle) * speed,
                    vy: Math.sin(angle) * speed,
                    alpha: 1
                });
            }
        }

        update() {
            this.particles.forEach((p) => {
                p.x += p.vx;
                p.y += p.vy;
                p.vy += 0.05; // Gravity effect
                p.alpha -= 0.02;
            });
            this.particles = this.particles.filter((p) => p.alpha > 0);
        }

        draw() {
            this.particles.forEach((p) => {
                ctx.globalAlpha = p.alpha;
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.arc(p.x, p.y, 2, 0, Math.PI * 2);
                ctx.fill();
            });
        }
    }

    let fireworks = [];
    function animate() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        fireworks.forEach((firework, index) => {
            firework.update();
            firework.draw();
            if (firework.particles.length === 0) {
                fireworks.splice(index, 1);
            }
        });
        requestAnimationFrame(animate);
    }
    
    canvas.addEventListener("click", (e) => {
        const colors = ["red", "blue", "yellow", "purple", "green", "white"];
        const color = colors[Math.floor(Math.random() * colors.length)];
        fireworks.push(new Firework(e.clientX, e.clientY, color));
    });
    
    animate();
</script>

</body>
</html>
