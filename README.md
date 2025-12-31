import pygame
import random
import math
import sys

# Initialize Pygame
pygame.init()

# Screen setup
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Happy New Year 2026 🎉")

# Load music (replace 'song.mp3' with your music file path)
pygame.mixer.init()
pygame.mixer.music.load("happy new year/Jalte Diye Music.mp3")
pygame.mixer.music.play(-1)  # loop indefinitely

# Colors
colors = [(255,0,0), (0,255,0), (0,0,255), (255,255,0), (255,165,0), (255,192,203), (0,255,255), (255,255,255)]

# Firework class
class Firework:
    def __init__(self):
        self.x = random.randint(100, WIDTH-100)
        self.y = random.randint(50, HEIGHT-200)
        self.particles = []
        self.create_particles()

    def create_particles(self):
        for _ in range(50):
            angle = random.uniform(0, 2*math.pi)
            speed = random.uniform(2,6)
            dx = math.cos(angle) * speed
            dy = math.sin(angle) * speed
            color = random.choice(colors)
            self.particles.append([self.x, self.y, dx, dy, color, 5])

    def move(self):
        for p in self.particles:
            p[0] += p[2]
            p[1] += p[3]
            p[5] -= 0.1  # particle size decrease

    def draw(self, screen):
        for p in self.particles:
            if p[5] > 0:
                pygame.draw.circle(screen, p[4], (int(p[0]), int(p[1])), int(p[5]))

# Main loop
fireworks = []
clock = pygame.time.Clock()
font = pygame.font.SysFont("arial", 48, bold=True)
running = True

while running:
    screen.fill((0,0,0))  # black background

    # Add new fireworks randomly
    if random.randint(0, 50) == 0:
        fireworks.append(Firework())

    # Update and draw fireworks
    for fw in fireworks:
        fw.move()
        fw.draw(screen)

    # Draw Happy New Year text
    text = font.render("HAPPY NEW YEAR 2026!", True, (255,215,0))
    screen.blit(text, (WIDTH//2 - text.get_width()//2, HEIGHT//2 - text.get_height()//2))

    # Event handling
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
            pygame.mixer.music.stop()

    pygame.display.flip()
    clock.tick(60)

pygame.quit()
sys.exit()
