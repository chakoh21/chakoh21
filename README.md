import pygame

# Define some colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# Define the screen size
WIDTH = 800
HEIGHT = 600

# Initialize Pygame
pygame.init()
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Ball in the Net")

# Define the ball and net objects
ball = pygame.Rect(WIDTH // 2 - 10, HEIGHT // 2 - 10, 20, 20)
net = pygame.Rect(WIDTH // 2 - 50, HEIGHT - 20, 100, 20)

# Define the game loop
running = True
while running:
    # Check for events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

        # Check for key presses
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                ball.x -= 5
            if event.key == pygame.K_RIGHT:
                ball.x += 5
            if event.key == pygame.K_UP:
                ball.y -= 5
            if event.key == pygame.K_DOWN:
                ball.y += 5

    # Update the ball position based on gravity
    ball.y += 1

    # Check for collisions with the net
    if ball.colliderect(net):
        # Play a sound and display a score message
        pygame.mixer.Sound("score.wav").play()
        score_text = "Score!"
        score_font = pygame.font.Font(None, 32)
        score_surface = score_font.render(score_text, True, GREEN)
        screen.blit(score_surface, (WIDTH // 2 - score_surface.get_width() // 2, 10))

    # Check for bottom wall collision and bounce
    if ball.y > HEIGHT - ball.height:
        ball.y = HEIGHT - ball.height
        ball.yvel = -ball.yvel

    # Draw the game objects
    screen.fill(BLACK)
    pygame.draw.rect(screen, WHITE, ball)
    pygame.draw.rect(screen, WHITE, net)

    # Update the display
    pygame.display.flip()

# Quit Pygame
pygame.quit()
