import pygame
import math
import random

pygame.init()

# Screen setup
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Looping Shark & Fish Scene")

clock = pygame.time.Clock()

# Colors
sky_day = (135, 206, 235)
sky_night = (10, 10, 35)
water_day = (0, 105, 148)
water_night = (0, 50, 80)

# Positions
fish_x, fish_y = -50, 500
shark_x, shark_y = -100, 480
boat_x, boat_direction = 350, 1
wave_offset = 0

# Day/Night cycle
sky_transition = 0.0
cycle_speed = 0.0015
daytime = True

# Phase system
phase = 1
school = []

# Clouds and birds
clouds = [[100, 100], [300, 80], [600, 120]]
birds = [[200, 60], [400, 50], [700, 90]]

# Main loop
running = True
while running:
    clock.tick(60)

    # Day/Night cycle
    if daytime:
        sky_transition += cycle_speed
        if sky_transition >= 1:
            sky_transition = 1
            daytime = False
    else:
        sky_transition -= cycle_speed
        if sky_transition <= 0:
            sky_transition = 0
            daytime = True

    # Interpolated colors
    sky_color = tuple(int(sky_day[i] * (1 - sky_transition) + sky_night[i] * sky_transition) for i in range(3))
    water_color = tuple(int(water_day[i] * (1 - sky_transition) + water_night[i] * sky_transition) for i in range(3))

    screen.fill(sky_color)

    # Sun and Moon
    sun_angle = math.pi * sky_transition
    moon_angle = math.pi * (1 - sky_transition)
    sun_x = int(WIDTH / 2 + math.cos(sun_angle - math.pi / 2) * 400)
    sun_y = int(300 + math.sin(sun_angle - math.pi / 2) * 200)
    moon_x = int(WIDTH / 2 + math.cos(moon_angle - math.pi / 2) * 400)
    moon_y = int(300 + math.sin(moon_angle - math.pi / 2) * 200)

    if sky_transition < 1:
        pygame.draw.circle(screen, (255, 255, 100), (sun_x, sun_y), 40)
    if sky_transition > 0:
        pygame.draw.circle(screen, (230, 230, 255), (moon_x, moon_y), 30)
        pygame.draw.circle(screen, sky_color, (moon_x - 10, moon_y - 10), 25)

    # Clouds
    for cloud in clouds:
        cloud[0] += 1
        if cloud[0] > WIDTH:
            cloud[0] = -100
        pygame.draw.ellipse(screen, (255, 255, 255), (cloud[0], cloud[1], 80, 40))
        pygame.draw.ellipse(screen, (255, 255, 255), (cloud[0] + 30, cloud[1] - 10, 60, 50))
        pygame.draw.ellipse(screen, (255, 255, 255), (cloud[0] + 50, cloud[1], 70, 40))

    # Birds
    for bird in birds:
        bird[0] -= 2
        if bird[0] < -20:
            bird[0] = WIDTH + 50
        pygame.draw.lines(screen, (0, 0, 0), False, [(bird[0], bird[1]), (bird[0] + 10, bird[1] - 5), (bird[0] + 20, bird[1])], 2)

    # Waves
    wave_offset += 0.1
    wave_points = [(x, 300 + 10 * math.sin(0.02 * x + wave_offset)) for x in range(0, WIDTH + 1, 10)]
    wave_points += [(WIDTH, HEIGHT), (0, HEIGHT)]
    pygame.draw.polygon(screen, water_color, wave_points)

    # boat
    boat_wave_y = min(y for x, y in wave_points if boat_x < x < boat_x + 120) - 20

    # Boat Hull
    pygame.draw.polygon(screen, (139, 69, 19), [
        (boat_x + 20, boat_wave_y + 30),
        (boat_x + 120, boat_wave_y + 30),
        (boat_x + 100, boat_wave_y + 60),
        (boat_x + 40, boat_wave_y + 60)
    ])
    pygame.draw.circle(screen, (139, 69, 19), (boat_x + 20, boat_wave_y + 45), 15)
    pygame.draw.circle(screen, (139, 69, 19), (boat_x + 120, boat_wave_y + 45), 15)

    # Railing
    pygame.draw.line(screen, (100, 50, 20), (boat_x + 30, boat_wave_y + 30), (boat_x + 110, boat_wave_y + 30), 2)

    # Cabin
    pygame.draw.rect(screen, (160, 82, 45), (boat_x + 55, boat_wave_y + 5, 30, 25))
    pygame.draw.rect(screen, (0, 191, 255), (boat_x + 60, boat_wave_y + 10, 10, 10))

    # Mast
    pygame.draw.line(screen, (105, 105, 105), (boat_x + 70, boat_wave_y + 30), (boat_x + 70, boat_wave_y - 30), 4)

    # Fisherman (Now standing *on* the boat)
    fisherman_base_y = boat_wave_y + 10
    pygame.draw.circle(screen, (255, 224, 189), (boat_x + 70, fisherman_base_y - 20), 6)  # head
    pygame.draw.line(screen, (0, 0, 0), (boat_x + 70, fisherman_base_y - 14), (boat_x + 70, fisherman_base_y + 10), 3)  # body
    pygame.draw.line(screen, (0, 0, 0), (boat_x + 70, fisherman_base_y - 5), (boat_x + 80, fisherman_base_y), 2)  # arm
    pygame.draw.line(screen, (0, 0, 0), (boat_x + 70, fisherman_base_y + 10), (boat_x + 65, fisherman_base_y + 25), 2)  # leg L
    pygame.draw.line(screen, (0, 0, 0), (boat_x + 70, fisherman_base_y + 10), (boat_x + 75, fisherman_base_y + 25), 2)  # leg R

    # Fishing rod + line
    pygame.draw.line(screen, (139, 69, 19), (boat_x + 80, fisherman_base_y), (boat_x + 110, boat_wave_y + 30), 2)
    pygame.draw.line(screen, (0, 0, 0), (boat_x + 110, boat_wave_y + 30), (boat_x + 110, boat_wave_y + 80), 1)

    # Hook at the end of the fishing line
    pygame.draw.circle(screen, (192, 192, 192), (boat_x + 110, boat_wave_y + 80), 4)
    pygame.draw.arc(screen, (0, 0, 0), (boat_x + 108, boat_wave_y + 78, 6, 6), math.pi, 2 * math.pi, 2)



    # --- PHASE 1: Shark chases one fish (left to right) ---
    if phase == 1:
        # Fish
        pygame.draw.ellipse(screen, (255, 165, 0), (fish_x, fish_y, 30, 15))
        pygame.draw.polygon(screen, (255, 140, 0), [(fish_x, fish_y + 7), (fish_x - 10, fish_y), (fish_x, fish_y + 15)])
        pygame.draw.circle(screen, (0, 0, 0), (int(fish_x + 22), int(fish_y + 5)), 2)

        fish_x += 2.5

        if shark_x + 50 < fish_x:
            shark_x += (fish_x - shark_x - 50) * 0.05

        # Shark facing right
        pygame.draw.ellipse(screen, (100, 100, 100), (shark_x, shark_y, 60, 30))
        pygame.draw.polygon(screen, (100, 100, 100), [(shark_x + 10, shark_y + 5), (shark_x - 10, shark_y + 15), (shark_x + 10, shark_y + 25)])
        pygame.draw.polygon(screen, (100, 100, 100), [(shark_x + 40, shark_y), (shark_x + 30, shark_y - 20), (shark_x + 20, shark_y)])
        pygame.draw.circle(screen, (255, 255, 255), (int(shark_x + 50), int(shark_y + 10)), 5)
        pygame.draw.circle(screen, (0, 0, 0), (int(shark_x + 50), int(shark_y + 10)), 2)
        pygame.draw.arc(screen, (0, 0, 0), (shark_x + 15, shark_y + 18, 30, 15), math.pi * 0.1, math.pi * 0.9, 2)

        # End phase 1 once fish is gone
        if fish_x > WIDTH + 60:
            phase = 2
            shark_x = -80
            school = [{"offset": i * 25} for i in range(10)]

    # --- PHASE 2: Shark chased by 10 fish (left to right) ---
    elif phase == 2:
        shark_x += 2

        # Shark facing right
        pygame.draw.ellipse(screen, (100, 100, 100), (shark_x, shark_y, 60, 30))
        pygame.draw.polygon(screen, (100, 100, 100), [(shark_x + 10, shark_y + 5), (shark_x - 10, shark_y + 15), (shark_x + 10, shark_y + 25)])
        pygame.draw.polygon(screen, (100, 100, 100), [(shark_x + 40, shark_y), (shark_x + 30, shark_y - 20), (shark_x + 20, shark_y)])
        pygame.draw.circle(screen, (255, 255, 255), (int(shark_x + 50), int(shark_y + 10)), 5)
        pygame.draw.circle(screen, (0, 0, 0), (int(shark_x + 50), int(shark_y + 10)), 2)
        pygame.draw.arc(screen, (0, 0, 0), (shark_x + 15, shark_y + 18, 30, 15), math.pi * 0.1, math.pi * 0.9, 2)

        # Draw chasing fish
        for i, fish in enumerate(school):
            fish_x = shark_x - fish["offset"]
            fish_y = 500 + (i % 2) * 10
            pygame.draw.ellipse(screen, (255, 165, 0), (fish_x, fish_y, 20, 10))
            pygame.draw.polygon(screen, (255, 140, 0), [(fish_x, fish_y + 5), (fish_x - 8, fish_y), (fish_x, fish_y + 10)])
            pygame.draw.circle(screen, (0, 0, 0), (int(fish_x + 15), int(fish_y + 3)), 2)

        # Only restart once shark AND trailing fish have exited
        if shark_x > WIDTH + 80:
            last_fish_x = shark_x - school[-1]["offset"]
            if last_fish_x > WIDTH + 40:
                phase = 1
                fish_x = -50
                shark_x = -100

    # Quit event
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    pygame.display.flip()

pygame.quit()

