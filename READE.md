Objective: Please write snake game in python oin just one file

======================================== Task Breakdown ========================================

======================================== Refined Final Output ========================================

1. **Project Name:** Snake Arena

2. **Folder Structure:**
```json
{
  "Snake_Game": {
    "main.py": null,
    "assets": {
      "images": {
        "snake.png": null,
        "apple.png": null,
        "background.jpg": null
      }
    },
    "game_logic.py": null
  }
}
```

3. **Code Files:**

**Snake_Game/main.py**
```python
import turtle
import random
import game_logic as gl

def main():
    screen = turtle.Screen()
    screen.title("Snake Arena")
    screen.bgcolor("#000033")
    screen.setup(width=600, height=600)
    
    snake = gl.Snake()
    food = gl.Food()
    
    while True:
        screen.listen()
        screen.onkey(snake.up, "Up")
        screen.onkey(snake.down, "Down")
        screen.onkey(snake.left, "Left")
        screen.onkey(snake.right, "Right")
        
        screen.update()
        
        snake.move()
        
        if snake.eaten(food):
            food.random_location()
            snake.extend_snake()
        
        for segment in snake.get_segments()[1:]:
            if snake.head.distance(segment) < 10:
                gl.game_over(screen, snake)
        
        if snake.game_over():
            gl.end_game(snake, screen)
            
        turtle.update()
    
main()
```

**Snake_Game/game_logic.py**
```python
class Snake:
    def __init__(self):
        self.segments = [turtle.Turtle() for _ in range(3)]
        self.direction = "up"
        self.head = self.segments[0]
        self.head.speed(0)
        self.head.penup()
        self.length = 3
        
    def up(self):
        if self.direction != "down":
            self.direction = "up"
            self.turn(-90)
    
    def down(self):
        if self.direction != "up":
            self.direction = "down"
            self.turn(90)
    
    def left(self):
        if self.direction != "right":
            self.direction = "left"
            self.turn(-180)
    
    def right(self):
        if self.direction != "left":
            self.direction = "right"
            self.turn(180)
    
    def turn(self, angle):
        for segment in self.segments:
            segment.setheading(segment.getheading() + angle)
    
    def move(self):
        for segment in self.segments:
            segment.forward(10)
            if segment == self.head and self.head.xcor() >= 290 or self.head.xcor() <= -290 or self.head.ycor() >= 290 or self.head.ycor() <= -290:
                game_over(screen, self)
                end_game(self, screen)
    
    def get_segments(self):
        return [segment for segment in self.segments if not (segment == self.head and self.head.xcor() == 0 and self.head.ycor() == 0)]
    
    def extend_snake(self):
        new_segment = turtle.Turtle()
        new_segment.speed(0)
        new_segment.penup()
        new_segment.color("white")
        self.segments.insert(0, new_segment)
    
    def eaten(self, food):
        x = self.head.xcor()
        y = self.head.ycor()
        return x == food.xcor() and y == food.ycor()
    
    def game_over(screen, snake):
        screen.title("Game Over!")
        for segment in snake.segments:
            segment.hideturtle()
    
    def end_game(self, screen, snake):
        score = len(snake.get_segments()) - 1
        message = f"Your score: {score}"
        screen.textinput("End Game", message)
        turtle.mainloop()
```

**Snake_Game/game_logic.py** also includes the `Food` class:
```python
class Food:
    def __init__(self):
        self.color("red")
        self.shape("circle")
        self.speed(0)
        self.penup()
        self.random_location()
    
    def random_location(self):
        self.goto(random.randint(-290, 290), random.randint(-290, 290))
```

This code provides a basic implementation of the snake game in Python using the `turtle` module for graphics. The `game_logic` module contains the logic for the snake and the food, while the `main` function in `main.py` sets up the game loop and event listeners.