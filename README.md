> joguinho
> import pygame
import sys
import random

# Inicialização do Pygame
pygame.init()

# Configurações da tela
WIDTH, HEIGHT = 800, 600
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)

# Configurações do jogador
player_size = 50
player_pos = [WIDTH // 2, HEIGHT - 2 * player_size]
player_speed = 10

# Configurações dos obstáculos
obstacle_size = 50
obstacle_pos = [random.randint(0, WIDTH - obstacle_size), 0]
obstacle_speed = 10

# Configurações do jogo
screen = pygame.display.set_mode((WIDTH, HEIGHT))
clock = pygame.time.Clock()

# Função para desenhar o jogador
def draw_player():
    pygame.draw.rect(screen, WHITE, (player_pos[0], player_pos[1], player_size, player_size))

# Função para desenhar o obstáculo
def draw_obstacle():
    pygame.draw.rect(screen, WHITE, (obstacle_pos[0], obstacle_pos[1], obstacle_size, obstacle_size))

# Loop principal do jogo
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Movimentação do jogador
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_pos[0] > 0:
        player_pos[0] -= player_speed
    if keys[pygame.K_RIGHT] and player_pos[0] < WIDTH - player_size:
        player_pos[0] += player_speed

    # Movimentação do obstáculo
    obstacle_pos[1] += obstacle_speed
    if obstacle_pos[1] > HEIGHT:
        obstacle_pos[0] = random.randint(0, WIDTH - obstacle_size)
        obstacle_pos[1] = 0

    # Verifica colisão entre jogador e obstáculo
    if (player_pos[1] < obstacle_pos[1] + obstacle_size and
        player_pos[1] + player_size > obstacle_pos[1] and
        player_pos[0] < obstacle_pos[0] + obstacle_size and
        player_pos[0] + player_size > obstacle_pos[0]):
        print("Game Over")
        pygame.quit()
        sys.exit()

    # Limpa a tela e desenha os objetos
    screen.fill(BLACK)
    draw_player()
    draw_obstacle()

    # Atualiza a tela
    pygame.display.flip()
    clock.tick(30)
