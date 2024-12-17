import pygame
import random

# تهيئة Pygame
pygame.init()

# إعدادات الشاشة
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("تجنب الكرات")

# الألوان
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# إعدادات اللاعب
player_width = 50
player_height = 50
player_x = screen_width // 2 - player_width // 2
player_y = screen_height - player_height - 10
player_speed = 5

# إعدادات الكرات
ball_radius = 20
ball_speed = 5
balls = []

# تحديد الخطوط الأساسية للعبة
clock = pygame.time.Clock()

# الدالة الرئيسية للرسم
def draw_game(player_x, player_y, balls):
    screen.fill(WHITE)  # ملء الشاشة باللون الأبيض
    pygame.draw.rect(screen, BLACK, (player_x, player_y, player_width, player_height))  # رسم اللاعب

    for ball in balls:  # رسم الكرات
        pygame.draw.circle(screen, RED, (ball[0], ball[1]), ball_radius)

    pygame.display.update()

# الدالة لتحريك الكرات
def move_balls(balls):
    for ball in balls:
        ball[1] += ball_speed  # تحريك الكرة إلى الأسفل
        if ball[1] > screen_height:
            balls.remove(ball)  # إذا خرجت الكرة من الشاشة، يتم إزالتها

# الدالة لإنشاء الكرات
def create_ball():
    ball_x = random.randint(0, screen_width - ball_radius*2)  # تحديد موقع الكرة بشكل عشوائي
    ball_y = -ball_radius  # بدء الكرة من فوق الشاشة
    return [ball_x, ball_y]

# الدالة لتفقد التصادم
def check_collision(player_x, player_y, balls):
    for ball in balls:
        if player_x < ball[0] + ball_radius*2 and player_x + player_width > ball[0]:
            if player_y < ball[1] + ball_radius*2 and player_y + player_height > ball[1]:
                return True  # إذا حدث التصادم
    return False

# اللعبة الرئيسية
def game():
    global player_x, player_y
    balls = []
    score = 0
    game_over = False

    while not game_over:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True

        keys = pygame.key.get_pressed()  # الحصول على المفاتيح المضغوطة
        if keys[pygame.K_LEFT] and player_x > 0:
            player_x -= player_speed  # تحريك اللاعب لليسار
        if keys[pygame.K_RIGHT] and player_x < screen_width - player_width:
            player_x += player_speed  # تحريك اللاعب لليمين

        # إضافة كرات جديدة بشكل عشوائي
        if random.randint(1, 30) == 1:
            balls.append(create_ball())

        move_balls(balls)  # تحريك الكرات للأسفل

        if check_collision(player_x, player_y, balls):  # تفقد التصادم
            game_over = True

        # زيادة النقاط
        score += 1

        draw_game(player_x, player_y, balls)  # رسم كل شيء
        pygame.display.set_caption(f"النقاط: {score}")

        clock.tick(60)  # تحديد سرعة اللعبة (60 إطار في الثانية)

    # عند انتهاء اللعبة
    print(f"اللعبة انتهت! النقاط: {score}")
    pygame.quit()

# بدء اللعبة
game()
