import pygame
import sys

# Inicializar o pygame
pygame.init()

# Tela
LARGURA, ALTURA = 800, 600
tela = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("Plataforma 2D")

# Cores
AZUL = (0, 0, 255)
VERDE = (0, 255, 0)
BRANCO = (255, 255, 255)

# Relógio
clock = pygame.time.Clock()

# Jogador
jogador = pygame.Rect(100, 500, 50, 50)
vel_x = 0
vel_y = 0
gravidade = 0.8
pulo_forca = -15
no_chao = False

# Plataforma
plataforma = pygame.Rect(0, 550, 800, 50)

# Loop principal
while True:
    clock.tick(60)
    tela.fill(BRANCO)

    # Eventos
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Teclas
    teclas = pygame.key.get_pressed()
    vel_x = 0
    if teclas[pygame.K_LEFT]: vel_x = -5
    if teclas[pygame.K_RIGHT]: vel_x = 5
    if teclas[pygame.K_SPACE] and no_chao:
        vel_y = pulo_forca
        no_chao = False

    # Aplicar gravidade
    vel_y += gravidade
    jogador.y += vel_y
    jogador.x += vel_x

    # Colisão com o chão
    if jogador.colliderect(plataforma):
        jogador.bottom = plataforma.top
        vel_y = 0
        no_chao = True
    else:
        no_chao = False

    # Desenhar objetos
    pygame.draw.rect(tela, AZUL, jogador)
    pygame.draw.rect(tela, VERDE, plataforma)

    pygame.display.update()
