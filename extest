from PyQt5.QtWidgets import QWidget, QLabel
from PyQt5.QtCore import QTimer, Qt
import random

class Point:
    pass

bonus = Point()
GAME_ROW = 20
GAME_COL = 20
GRID_WIDTH = 20
GRID_HEIGHT = 20
SNAKE_INIT_ROW = 10
SNAKE_INIT_LEMGTH = 5
SNAKE_Q_LENGTH = GAME_ROW * GAME_COL + 100
SNAKE_COLOR =      "background-color:#ffffff;"
SNAKE_HEAD_COLOR = "background-color:#ffff00;"
BONUS_COLOR      = "background-color:#ff0000;"
BG_COLOR =         "background-color:#000000;"
snake_head = SNAKE_INIT_LEMGTH - 1 
snake_tail = 0
SNAKE_ADD_LENGTH = 5
snake_move = 1

class myWindow(QWidget):
    def __init__(self):
        super().__init__()
        
    def keyPressEvent(self, event):
        global snake_dir        
        if event.key() == Qt.Key_Up:
            if snake_dir != 2:
               snake_dir = 1
        
        if event.key() == Qt.Key_Down:
            if snake_dir != 1:
               snake_dir = 2
        
        if event.key() == Qt.Key_Left:
            if snake_dir != 4:
               snake_dir = 3
    
        if event.key() == Qt.Key_Right:
            if snake_dir != 3:
               snake_dir = 4
    
window = myWindow()

def inside_snake(mode, x, y):
    #print('inside_snake():')
    global snake_q, snake_head, snake_tail, SNAKE_Q_LENGTH
    s = snake_head 
    e = snake_tail    
    if mode == 1:
        if x == snake_q[s].x and y == snake_q[s].y:
            #print('inside_snake(1)')
            return True
            
    while True:
        s -= 1
        if s == -1:
            s = SNAKE_Q_LENGTH - 1
        if x == snake_q[s].x and y == snake_q[s].y:
            #print('inside_snake(0)')
            return True

        if s == e:
            break            

    return False

def new_bonus():
    global bonus, grids, GAME_COL, GAME_ROW, BONUS_COLOR
    while True:
        bonus.x = random.randint(0, GAME_COL-1)
        bonus.y = random.randint(0, GAME_ROW-1)
        if not inside_snake(1, bonus.x, bonus.y):
            break
    #print('new_bonus():', bonus.x, bonus.y) 
    grids[bonus.y][bonus.x].setStyleSheet(BONUS_COLOR)
   
def game_init():
    global GAME_ROW, GAME_COL, grids, BG_COLOR, SNAKE_INIT_LEMGTH, snake_q, snake_head, snake_tail, snake_dir, game_mode, snake_move, score
    for i in range(GAME_ROW):
        for j in range(GAME_COL):
            grids[i][j].setStyleSheet(BG_COLOR)    

    for i in range(SNAKE_INIT_LEMGTH):
        snake_q[i].y = SNAKE_INIT_ROW; 
        snake_q[i].x = i;
        grids[SNAKE_INIT_ROW][i].setStyleSheet(SNAKE_COLOR)        
    snake_head = SNAKE_INIT_LEMGTH - 1
    snake_tail = 0;
    grids[snake_q[snake_head].y][snake_q[snake_head].x].setStyleSheet(SNAKE_HEAD_COLOR)

    while True:
        snake_dir = random.randint(1, 4)
        if snake_dir != 3:
            break
    game_mode = 1 
    snake_move = 1 
    score = 0
    new_bonus()    
#---------------------------------  main program ----------------------------------------------
grids = []
for y in range(GAME_COL):
    g_row=[] #for 1 row only
    for x in range(GAME_ROW):
        g = QLabel(window)
        g.setStyleSheet(BG_COLOR) #set all grids with black color
        g.setFixedSize(GRID_WIDTH, GRID_HEIGHT)
        g.move(GRID_WIDTH*x, GRID_HEIGHT*y)
        g_row.append(g)        
    
    grids.append(g_row) #create 2-D array of grids

snake_q = []
for i in range(SNAKE_Q_LENGTH):
    p = Point()
    snake_q.append(p)
    
for i in range(SNAKE_INIT_LEMGTH):
    p = Point()
    p.y = SNAKE_INIT_ROW
    p.x = i
    snake_q[i] = p 
    grids[SNAKE_INIT_ROW][i].setStyleSheet(SNAKE_COLOR)                
grids[snake_q[snake_head].y][snake_q[snake_head].x].setStyleSheet(SNAKE_HEAD_COLOR)

while True:
    snake_dir = random.randint(1, 4)
    if snake_dir != 3:
        break
game_mode = 1
snake_move = 1 
score = 0
new_bonus()
timer = QTimer(window)  

def timer_tick():
    global snake_q, snake_head, SNAKE_Q_LENGTH, GAME_ROW, GAME_COL, timer, SNAKE_ADD_LENGTH, snake_move, score, snake_tail, BONUS_COLOR
    #print('snake_q len=', len(snake_q), 'snake_head=', snake_head)
    y = snake_q[snake_head].y
    x = snake_q[snake_head].x
    snake_head += 1
    if snake_head == SNAKE_Q_LENGTH:
        snake_head = 0    
    snake_q[snake_head].y = y 
    snake_q[snake_head].x = x

    if snake_dir == 1:
        snake_q[snake_head].y -= 1
        if snake_q[snake_head].y < 0:
            snake_q[snake_head].y = GAME_ROW - 1    

    if snake_dir == 2:
        snake_q[snake_head].y += 1
        if snake_q[snake_head].y >= GAME_ROW:
            snake_q[snake_head].y = 0    

    if snake_dir == 3:
        snake_q[snake_head].x -= 1
        if snake_q[snake_head].x < 0:
            snake_q[snake_head].x = GAME_COL - 1    

    if snake_dir == 4:
        snake_q[snake_head].x += 1
        if snake_q[snake_head].x >= GAME_COL:
            snake_q[snake_head].x = 0
     
    grids[snake_q[snake_head].y][snake_q[snake_head].x].setStyleSheet(SNAKE_HEAD_COLOR)
    grids[y][x].setStyleSheet(SNAKE_COLOR)        
    if inside_snake(1, bonus.x, bonus.y):
        new_bonus()
        snake_move += SNAKE_ADD_LENGTH
        score += 100     
        print('score=', score)

    if inside_snake(0, snake_q[snake_head].x, snake_q[snake_head].y):
        print('game over')
        timer.stop()        
        game_init()
        timer.start()
        return     

    if snake_move == 1:
        grids[snake_q[snake_tail].y][snake_q[snake_tail].x].setStyleSheet(BG_COLOR)
        snake_tail += 1
        if snake_tail == SNAKE_Q_LENGTH:
            snake_tail = 0
    else:
        snake_move -= 1
        
window.show()
timer.timeout.connect(timer_tick)
timer.start(200)
