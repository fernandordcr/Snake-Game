#Configurações iniciais do jogo

import pygame     #biblioteca para permitir a criação de jogos
import random     #biblioteca para gerar números aleatórios

#No terminal, instale a biblioteca pygame com o comando: pip install pygame

pygame.init()    #inicializa o pygame
pygame.display.set_caption('Snake Game')    #título da janela do jogo
largura, altura = 800, 600    #dimensões da janela do jogo
tela = pygame.display.set_mode((largura, altura))    #cria a janela do jogo

#Cores
Preta = (0, 0, 0)
Laranja = (255, 165, 0)
Amarelo = (255, 255, 0)
Azul = (0, 0, 255)

#Parametros da cobra
tamanho_quadrado = 20   #tamanho do quadrado que compõe a cobra
velocidade_cobra = 15   #velocidade da cobra

#funçaõ para gerar comida
def gerar_comida():    #função para gerar comida
    comida_x = round (random.randrange (0, largura - tamanho_quadrado) / 20.0) * 20.0   #posição da comida no eixo x
    comida_y = round (random.randrange (0, altura - tamanho_quadrado) / 20.0) * 20.0    #posição da comida no eixo y 
    return comida_x, comida_y

#função para desenhar a comida
def desenhar_comida (tamanho, comida_x, comida_y): #pegar o tamanho, a posição da comida no eixo x e a posição da comida no eixo y
    pygame.draw.rect(tela, Amarelo, [comida_x, comida_y, tamanho, tamanho])   #desenha o retangulo e onde vai ser desenhado

#função para desenhar a cobra
def desenhar_cobra  (tamanho, pixels):    #pegar o tamanho e os pixels da cobra
    for pixel in pixels:    #percorre todos os pixels da cobra
        pygame.draw.rect(tela, Azul, [pixel[0], pixel[1], tamanho, tamanho])   #desenha o retangulo e onde vai ser desenhado

#função para desenhar os pontos
def desenhar_pontos (pontos):    #pegar os pontos
    fonte = pygame.font.SysFont("Helvetica", 40)    #cria um objeto para a fonte
    texto = fonte.render(f'Pontos: {pontos}', True, Laranja)    #criar um texto com os pontos e a cor
    tela.blit(texto, [1, 1])    #desenha o texto na tela

#função para selecionar a velocidade
def selecionar_velocidade (tecla):    #pegar a tecla
    if tecla == pygame.K_DOWN:      #se a tecla for a seta para baixo
        velocidade_X = 0    #velocidade no eixo x
        velocidade_y = tamanho_quadrado    #velocidade no eixo y
    elif tecla == pygame.K_UP:      #se a tecla for a seta para cima
        velocidade_X = 0    #velocidade no eixo x
        velocidade_y = -tamanho_quadrado  #velocidade - tamanho do quadrado, vai andar para cima
    elif tecla == pygame.K_RIGHT:      #se a tecla for a seta para direita
        velocidade_X = tamanho_quadrado   #velocidade no eixo x porque vai andar para a direita
        velocidade_y = 0  #0 porque não vai andar no eixo y
    elif tecla == pygame.K_LEFT:      #se a tecla for a seta para esquerda
        velocidade_X = -tamanho_quadrado   #- tamanho do quadrado porque vai andar para a esquerda
        velocidade_y = 0  #0 porque não vai andar no eixo y
    return velocidade_X, velocidade_y    #retorna a velocidade no eixo x e no eixo y


#Função para rodar o jogo
def rodar_jogo ():     #função para rodar o jogo
    fim_jogo = False   #variável para controlar o fim do jogo
    relogio = pygame.time.Clock()   #cria um objeto para controlar o tempo

#variavés de como vai começar o jogo

    x = largura/2   #posição inicial da cobra no eixo x
    y = altura/2    #posição inicial da cobra no eixo y


    velocidade_X = 0  #quantos pixels a cobra vai andar no eixo x
    velocidade_Y = 0  #quantos pixels a cobra vai andar no eixo y

    tamanho_cobra = 1   #tamanho inicial da cobra
    pixels_cobra = []   #lista para armazenar os pixels da cobra

    comida_x, comida_y = gerar_comida ()    #posição inicial da comida
    
    while not fim_jogo:     #enquanto o jogo não acabar
        tela.fill(Preta)    #preenche a tela de preto
        for evento in pygame.event.get():       #percorre todos os eventos do jogo
            if evento.type == pygame.QUIT:       #se o evento for fechar a janela
                fim_jogo = True             #o jogo acaba
            elif evento.type == pygame.KEYDOWN:    #se o evento for apertar uma tecla
                velocidade_X, velocidade_Y = selecionar_velocidade(evento.key)  #chama a função selecionar_velocidade()

        #desenhar comida
        desenhar_comida(tamanho_quadrado, comida_x, comida_y)   #chama a função desenhar_comida()

        #atualizar a posição da cobra
        if x < 0 or x >= largura or y < 0 or y >= altura:    #se a cobra sair da tela
            fim_jogo = True    #o jogo acaba

        x += velocidade_X    #atualiza a posição da cobra no eixo x
        y += velocidade_Y    #atualiza a posição da cobra no eixo y

        #desenhar cobra
        pixels_cobra.append ([x, y])    #adiciona a posição da cobra na lista de pixels
        if len(pixels_cobra) > tamanho_cobra:     #se o tamanho da cobra for maior que o tamanho da cobra
            del pixels_cobra[0]  #deleta o primeiro pixel da cobra
        for pixel in pixels_cobra[:-1]:   #percorre todos os pixels da cobra menos o último
            if pixel == [x,y]:    #se o pixel for igual a posição da cobra 
                fim_jogo = True   #o jogo acaba
        desenhar_cobra(tamanho_quadrado, pixels_cobra)    #chama a função desenhar_cobra()

        #desenhar pontos
        desenhar_pontos (tamanho_cobra - 1)         #chama a função desenhar_pontos()

        #atualizar a tela
        pygame.display.update()    #atualiza a tela
        relogio.tick(velocidade_cobra)    #controla a velocidade da cobra

        #criar uma nova comida
        if x  == comida_x and y == comida_y:    #se a posição da cobra for igual a posição da comida
            tamanho_cobra += 1                  #o tamanho da cobra aumenta
            comida_x, comida_y = gerar_comida()   #gera uma nova comida

rodar_jogo()    #chama a função rodar_jogo() para iniciar o jogo

