# Snake-game-
Experience the classic Snake game brought to life with Python! Navigate a pixelated snake around the screen, eating apples to grow longer while avoiding collisions with the walls and yourself. As you gobble up apples, your snake's tail lengthens, making the game progressively challenging.

import pygame
import random

pygame.init()

# Set up display
width, height = 640, 480
display = pygame.display.set_mode((width, height))
pygame.display.set_caption("Snake Game")

# Colors
black = (0, 0, 0)
white = (255, 255, 255)
green = (0, 255, 0)
red = (255, 0, 0)

# Snake and food initialization
snake_pos = [100, 50]
snake_body = [[100, 50], [90, 50], [80, 50]]
food_pos = [random.randrange(1, (width//10)) * 10,
            random.randrange(1, (height//10)) * 10]
food_spawn = True

# Direction
direction = 'RIGHT'
change_to = direction

# Initial speed
speed = 15

# Score
score = 0

# Main Function
def main():
    global direction, change_to, snake_pos, food_pos, food_spawn, score

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                quit()
            # Arrow keys for changing direction
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP:
                    change_to = 'UP'
                if event.key == pygame.K_DOWN:
                    change_to = 'DOWN'
                if event.key == pygame.K_LEFT:
                    change_to = 'LEFT'
                if event.key == pygame.K_RIGHT:
                    change_to = 'RIGHT'

        # Change direction
        if change_to == 'UP' and not direction == 'DOWN':
            direction = 'UP'
        if change_to == 'DOWN' and not direction == 'UP':
            direction = 'DOWN'
        if change_to == 'LEFT' and not direction == 'RIGHT':
            direction = 'LEFT'
        if change_to == 'RIGHT' and not direction == 'LEFT':
            direction = 'RIGHT'

        # Update snake position [x, y]
        if direction == 'RIGHT':
            snake_pos[0] += 10
        if direction == 'LEFT':
            snake_pos[0] -= 10
        if direction == 'UP':
            snake_pos[1] -= 10
        if direction == 'DOWN':
            snake_pos[1] += 10

        # Snake body growing mechanism
        snake_body.insert(0, list(snake_pos))
        if snake_pos[0] == food_pos[0] and snake_pos[1] == food_pos[1]:
            score += 1
            food_spawn = False
        else:
            snake_body.pop()

        if not food_spawn:
            food_pos = [random.randrange(1, (width//10)) * 10,
                        random.randrange(1, (height//10)) * 10]
        food_spawn = True

        # Draw snake and food
        display.fill(black)
        for pos in snake_body:
            pygame.draw.rect(display, green,
                             pygame.Rect(pos[0], pos[1], 10, 10))

        pygame.draw.rect(display, white, pygame.Rect(
            food_pos[0], food_pos[1], 10, 10))

        # Game Over conditions
        if snake_pos[0] < 0 or snake_pos[0] > width-10:
            pygame.quit()
            quit()
        if snake_pos[1] < 0 or snake_pos[1] > height-10:
            pygame.quit()
            quit()

        # Touching the snake body
        for block in snake_body[1:]:
            if snake_pos[0] == block[0] and snake_pos[1] == block[1]:
                pygame.quit()
                quit()

        pygame.display.update()
        pygame.time.Clock().tick(speed)

        # Print score
        print("Your Score is: ", score)


if __name__ == '__main__':
    main()
