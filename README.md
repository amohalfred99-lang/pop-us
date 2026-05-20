[mygame.py](https://github.com/user-attachments/files/28036848/mygame.py)
import pygame
import random
import sys

# Initialize pygame
pygame.init()

# Screen setup
WIDTH, HEIGHT = 600, 800
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Bubble Up Game")

# Colors
WHITE = (255, 255, 255)
BLUE = (100, 150, 255)

# Bubble class
class Bubble:
    def __init__(self):
        self.radius = random.randint(20, 40)
        self.x = random.randint(self.radius, WIDTH - self.radius)
        self.y = HEIGHT + self.radius
        self.speed = random.uniform(1, 3)

    def move(self):
        self.y -= self.speed

    def draw(self):
        pygame.draw.circle(screen, BLUE, (int(self.x), int(self.y)), self.radius)

    def is_clicked(self, pos):
        return (self.x - pos[0])**2 + (self.y - pos[1])**2 <= self.radius**2

# Game loop
clock = pygame.time.Clock()
bubbles = []
score = 0
font = pygame.font.SysFont(None, 36)

while True:
    screen.fill(WHITE)

    # Event handling
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        elif event.type == pygame.MOUSEBUTTONDOWN:
            pos = pygame.mouse.get_pos()
            for bubble in bubbles[:]:
                if bubble.is_clicked(pos):
                    bubbles.remove(bubble)
                    score += 1

    # Add bubbles
    if random.random() < 0.02:  # spawn rate
        bubbles.append(Bubble())

    # Move and draw bubbles
    for bubble in bubbles[:]:
        bubble.move()
        bubble.draw()
        if bubble.y + bubble.radius < 0:  # bubble off screen
            bubbles.remove(bubble)

    # Draw score
    score_text = font.render(f"Score: {score}", True, (0, 0, 0))
    screen.blit(score_text, (10, 10))

    pygame.display.flip()
    clock.tick(60)
