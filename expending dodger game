# Dodger!.py

# Name: Michael Varnerin
# Date: 4 September 2021

# Import Necessary Libraries
import tkinter as tk
from tkinter import ttk
from tkinter import *
from random import *
import time
import statistics
import threading


class Dodger:
    """
    Welcome to Dodger! This is a game made by Michael Varnerin for CSI 31. In this game, you will control a circular
    character and try to dodge fast-falling objects. If one of the objects hits you, it's Game Over! I hope you enjoy
    this game!
    """
    coords = [0, 0]

    def __init__(self):
        """
        This is the constructor that creates the character customizer window.
        """
        self.char_window = tk.Tk()
        self.char_window.title("Welcome to Dodger!")
        self.char_window_canvas = tk.Canvas(width=200, height=200)
        self.create_char_widgets()
        try:
            HighScore = open('HighScore.txt', 'r')
        except FileNotFoundError:
            HighScore = open('HighScore.txt.', 'w')
            HighScore.write('0')
            HighScore = open('HighScore.txt', 'r')
        self.highScore = HighScore.read()
        HighScore.close()

    def create_char_widgets(self):
        """
        This method provides the framing for the GUI that the user will interact with.
        This is where the user can read how to play Dodger!, in addition to choosing the color character they wish to play as.
        """
        self.char_window['padx'] = 5
        self.char_window['pady'] = 5

        self.instruction_frame = ttk.LabelFrame(self.char_window, text='How to Play:', relief=tk.RIDGE)
        self.instruction_frame.pack()

        instruction_label = ttk.Label(self.instruction_frame,
                                      text="Welcome to Dodger! In this game, your goal is to survive as long as possible while dodging randomly falling objects.\n\nHow to Play:\n  1. Choose your character's color using the customizer below.\n  2. When you are ready, click the 'Start Game' button to begin playing!\n  3. To move your character, move your mouse while holding down the left-click button. YOUR CHARACTER WILL NOT MOVE UNLESS YOU ARE HOLDING DOWN THE LEFT-CLICK BUTTON!\n  4. Survive for as long as possible!\n\n Good Luck, and I hope you enjoy playing this game!")
        instruction_label.grid(row=1, columnspan=2)

        self.main_frame = ttk.LabelFrame(self.char_window, text='Character Customizer', relief=tk.RIDGE)
        self.main_frame.pack()

        character_label = ttk.Label(self.main_frame,
                                    text="What color would you like your character to be?  \n(Red, Blue, Green, Pink, Yellow, Purple, or Orange) ")
        character_label.grid(row=2, column=1)

        self.character_color_entry = ttk.Entry(self.main_frame, width=50)
        self.character_color_entry.grid(row=2, column=2)

        button_color = ttk.Button(self.main_frame, text="Preview Character", command=self.character_builder)
        button_color.grid(row=3, column=2)

        self.startGame_frame = ttk.Frame(self.char_window, relief=tk.RIDGE)
        self.startGame_frame.pack()

    def character_builder(self):
        """
        This method creates the character object in addition to showing a preview of the character of choice to the user prior to the game starting.
        """
        self.char_window_canvas.delete('all')
        self.color_choice = self.character_color_entry.get()
        self.color_choice = self.color_choice.lower()
        if self.color_choice in ['red', 'blue', 'green', 'pink', 'yellow', 'purple', 'orange']:
            self.char_window_canvas.create_text(100, 50, text="This will be your character:", font='Helvetica 11 bold')
            self.char_window_canvas.create_oval(75, 75, 125, 125, fill=self.color_choice)
            button_game_start = ttk.Button(self.startGame_frame, text="            Start Game!            ",
                                           command=self.game)
            button_game_start.grid(row=1, columnspan=4)
        else:
            self.char_window_canvas.create_text(100, 100, text="Please enter a valid color.", fill='red',
                                                font='Helvetica 12 bold')
            button_game_start = ttk.Button(self.startGame_frame, text="Please Enter a Valid Color")
            button_game_start.grid(row=1, columnspan=4)
        self.char_window_canvas.pack()

    def game(self):

        self.char_window.destroy()

        self.game_window = tk.Tk()
        self.game_window.title("Dodger!")
        self.game_window.attributes('-fullscreen', True)
        self.screen_width = self.game_window.winfo_screenwidth()
        self.screen_height = self.game_window.winfo_screenheight()
        self.platform_y = self.screen_height - 120

        self.game_window_canvas = tk.Canvas(
            self.game_window,
            width=self.screen_width,
            height=self.screen_height,
            bg='black'
        )

        self.game_window_canvas.pack()

        # PLATFORM
        self.platform = self.game_window_canvas.create_rectangle(
            0,
            self.platform_y + 30,
            self.screen_width,
            self.platform_y + 60,
            fill="white"
        )

        # CHARACTER START
        x1 = self.screen_width // 2 - 25
        x2 = self.screen_width // 2 + 25

        y1 = self.platform_y - 25
        y2 = self.platform_y + 25

        self.score = 0
        self.running = True
        self.move_speed = 20

        self.character = self.game_window_canvas.create_oval(
            x1,
            y1,
            x2,
            y2,
            fill=self.color_choice
        )

        self.coords = [self.screen_width // 2, self.platform_y]

        self.create_game_widgets()

        self.game_window.focus_force()

        self.game_window.bind("<Left>", lambda e: self.move_left())
        self.game_window.bind("<Right>", lambda e: self.move_right())
        self.game_window.bind("a", lambda e: self.move_left())
        self.game_window.bind("d", lambda e: self.move_right())
        self.original_meteor = tk.PhotoImage(file="Meteor.png")
        self.meteor_image = self.original_meteor.subsample(3, 3)
        self.run_game()
    def run_game(self):
        if not self.running:
            return

        self.spikes_easy()  

        self.game_window.after(30, self.run_game)
    def movement(self, event):

        if 25 <= event.x <= self.screen_width - 25:
            self.coords[0] = event.x

        self.drawCharacter(self.coords[0], self.platform_y)

    def spikes_easy(self):
        # Create spike once
        if not hasattr(self, "current_spike"):
            self.rand_x = randrange(50, self.screen_width - 50)
            self.spike_y = -50
            self.current_spike = self.game_window_canvas.create_image( self.rand_x, self.spike_y, image=self.meteor_image)

        # Move spike down
        self.spike_y += 10
        self.game_window_canvas.coords(
            self.current_spike,
            self.rand_x,
            self.spike_y
        )
        # Collision check
        if abs(self.coords[0] - self.rand_x) < 50 and abs(self.coords[1] - self.spike_y) < 50:
            self.game_over()
        
        # If spike goes off screen → reset
        if self.spike_y > self.screen_height:
            self.game_window_canvas.delete(self.current_spike)
            del self.current_spike
            self.score += 1
            self.scoreLabel.config(text="Score: " + str(self.score))
    def spikes_medium(self):
        """
        This method draws two spikes and causes them to fall from the top of the screen to the bottom of the screen. Every time a spike is dodged, this method adds 1 to the player's score.
        Additionally, in order to detect collision, this method contains a conditional statement that will check if the character's x and y coordinates match those of the falling spikes. If so,
        a Game Over is triggered.
        """
        self.scoreLabel.configure(text="Score: " + str(self.score))
        self.rand_x = randrange(30, self.screen_width - 50)
        self.rand_x2 = randrange(30, self.screen_width - 50)
        spike = self.game_window_canvas.create_oval(self.rand_x, -20, self.rand_x + 40, 50, fill='grey')
        spike2 = self.game_window_canvas.create_oval(self.rand_x2, -20, self.rand_x2 + 40, 50, fill='grey')
        self.timeDelay = .008
        for i in range(83):
            self.i = i
            threading.Thread(time.sleep(self.timeDelay)).run()
            threading.Thread(self.game_window_canvas.move(spike, 0, 10)).run()
            threading.Thread(self.game_window_canvas.move(spike2, 0, 10)).run()
            self.game_window_canvas.update()
            self.spike_x_coord = self.rand_x
            self.spike2_x_coord = self.rand_x2
            self.spike_y_coord = 50 + 10 * self.i
            if self.spike_y_coord - 35 <= self.coords[1] <= self.spike_y_coord + 35 and (
                    self.spike_x_coord - 35 <= self.coords[0] <= self.spike_x_coord + 35 or self.spike2_x_coord - 35 <=
                    self.coords[0] <= self.spike2_x_coord + 35):
                self.game_over()
                break
            elif not self.running:
                break
        self.score += 2

    def spikes_hard(self):
        """
        This method draws three spikes and causes them to fall from the top of the screen to the bottom of the screen. Every time a spike is dodged, this method adds 1 to the player's score.
        Additionally, in order to detect collision, this method contains a conditional statement that will check if the character's x and y coordinates match those of the falling spikes. If so,
        a Game Over is triggered.
        """
        self.scoreLabel.configure(text="Score: " + str(self.score))
        self.rand_x = randrange(30, self.screen_width - 50)
        self.rand_x2 = randrange(30, self.screen_width - 50)
        self.rand_x3 = randrange(30, self.screen_width - 50)
        spike = self.game_window_canvas.create_oval(self.rand_x, -20, self.rand_x + 40, 50, fill='grey')
        spike2 = self.game_window_canvas.create_oval(self.rand_x2, -20, self.rand_x2 + 40, 50, fill='grey')
        spike3 = self.game_window_canvas.create_oval(self.rand_x3, -20, self.rand_x3 + 40, 50, fill='grey')
        self.timeDelay = .006
        for i in range(83):
            self.i = i
            time.sleep(self.timeDelay)
            self.game_window_canvas.move(spike, 0, 10)
            self.game_window_canvas.move(spike2, 0, 10)
            self.game_window_canvas.move(spike3, 0, 10)
            self.game_window_canvas.update()
            self.spike_x_coord = self.rand_x
            self.spike2_x_coord = self.rand_x2
            self.spike3_x_coord = self.rand_x3
            self.spike_y_coord = 50 + 10 * self.i
            if self.spike_y_coord - 35 <= self.coords[1] <= self.spike_y_coord + 35 and (
                    self.spike_x_coord - 35 <= self.coords[0] <= self.spike_x_coord + 35 or self.spike2_x_coord - 35 <=
                    self.coords[0] <= self.spike2_x_coord + 35 or self.spike3_x_coord - 35 <= self.coords[
                        0] <= self.spike3_x_coord + 35):
                self.game_over()
                break
            elif not self.running:
                break
        self.score += 3

    def drawCharacter(self, x, y):

        self.game_window_canvas.coords( self.character, x - 25, self.platform_y - 25, x + 25, self.platform_y + 25
    )
    def create_game_widgets(self):
        """
        This method contains widgets that bind the character's movements to the self.movement method, in addition to providing the user
        with their current Score and a Quit Game button.
        """
        self.game_window['padx'] = 1
        self.game_window['pady'] = 1

        self.game_command_frame = ttk.LabelFrame(self.game_window, relief=tk.RIDGE)
        self.game_command_frame.pack()
        self.game_window_canvas.focus_set()
        self.game_window_canvas.focus_force()
        self.game_window_canvas.bind('<Motion>', self.movement)

        self.scoreLabel = ttk.Label(self.game_command_frame, text="Score: " + str(self.score))
        self.scoreLabel.grid(row=1, column=1)
        self.highScoreLabel = ttk.Label(self.game_command_frame, text="High Score: " + str(self.highScore))
        self.highScoreLabel.grid(row=2, column=1)
        quit_button = ttk.Button(self.game_command_frame, text="Quit Game", command=self.quit_game)
        quit_button.grid(row=3, columnspan=3)
        self.game_window_canvas.bind("<Left>", lambda e: self.move_left())
        self.game_window_canvas.bind("<Right>", lambda e: self.move_right())
        self.game_window_canvas.bind("<KeyPress-a>", lambda e: self.move_left())
        self.game_window_canvas.bind("<KeyPress-d>", lambda e: self.move_right())
    def quit_game(self):
        self.running = False
        self.game_window.destroy()
    def move_right(self):
        if self.coords[0] + self.move_speed < self.screen_width - 25:
         self.coords[0] += self.move_speed
         self.drawCharacter(self.coords[0], self.platform_y)

    def move_left(self):

        if self.coords[0] - self.move_speed > 25:
            self.coords[0] -= self.move_speed

        self.drawCharacter(self.coords[0], self.platform_y)
        self.drawCharacter(self.coords[0], self.platform_y)
    def play_again(self):
        """
        This method destroys the game over window, and restarts the program.
        """
        self.game_over_window.destroy()
        Dodger()

    def game_over(self):
    
        self.running = False

    # Destroy old game window
        self.game_window.destroy()

    # Create fullscreen game over window
        self.game_over_window = tk.Tk()
        self.game_over_window.title("Game Over")
        self.game_over_window.attributes('-fullscreen', True)

    # Fullscreen canvas
        self.game_over_window_canvas = tk.Canvas(
        self.game_over_window,
        width=self.screen_width,
        height=self.screen_height,
        bg='black'
    )
        self.game_over_window_canvas.pack(fill="both", expand=True)

    # Main text
        self.game_over_window_canvas.create_text(
        self.screen_width // 2,
        200,
        text="GAME OVER",
        fill='red',
        font=('Helvetica', 48, 'bold')
    )

    # Score text
        self.game_over_window_canvas.create_text(
        self.screen_width // 2,
        320,
        text="Final Score: " + str(self.score),
        fill='white',
        font=('Helvetica', 28, 'bold')
    )

    # High score handling
        if self.score > int(self.highScore):

            self.game_over_window_canvas.create_text(
                self.screen_width // 2,
            400,
            text="NEW HIGH SCORE!",
            fill='yellow',
            font=('Helvetica', 32, 'bold')
        )

        HighScore = open('HighScore.txt', 'w')
        HighScore.write(str(self.score))
        HighScore.close()

    # Play Again button
        play_again_button = Button(
            self.game_over_window,
        text='Play Again',
        font=('Helvetica', 20),
        width=15,
        height=2,
        command=self.play_again
    )

        play_again_button_window = self.game_over_window_canvas.create_window(
        self.screen_width // 2,
        520,
        window=play_again_button
    )

    # Quit button
        quit_button = Button(
        self.game_over_window,
        text='Quit',
        font=('Helvetica', 20),
        width=15,
        height=2,
        command=self.game_over_window.destroy
    )

        quit_button_window = self.game_over_window_canvas.create_window(
        self.screen_width // 2,
        620,
        window=quit_button
    )

    # Escape key quits
        self.game_over_window.bind("<Escape>", lambda e: self.game_over_window.destroy())
# Create thedawdawdsadaawdasdawd entire GUI program
program = Dodger()

# Start the GUI event loop
program.char_window.mainloop()
