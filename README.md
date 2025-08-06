'''
Author: Tej Patel
Date Created: 06/06/2025
Last Date Modified: 06/08/2025
Purpose: Create a math quiz program to test the user on a variety of math questions.
'''

# Import all necessary libraries for the quiz to function. 
from tkinter import *
from tkinter import messagebox
import random
from PIL import Image, ImageTk, ImageSequence

# Create, name and resize all windows.
# Hide all windows which are not needed to begin with.
welcome_window = Tk()
welcome_window.title("Welcome to The Counting Kingdom")
welcome_window.geometry("800x500")
welcome_window.resizable(False, False)

difficulty_window = Toplevel(welcome_window)
difficulty_window.title("Select Difficulty")
difficulty_window.geometry("600x400")
difficulty_window.resizable(False, False)
difficulty_window.withdraw()

basic_window = Toplevel(welcome_window)
basic_window.title("Basic Battle")
basic_window.geometry("800x500")
basic_window.resizable(False, False)
basic_window.withdraw()

advanced_window = Toplevel(welcome_window)
advanced_window.title("Advanced Academy")
advanced_window.geometry("850x650")
advanced_window.resizable(False, False)
advanced_window.withdraw()

expert_window = Toplevel(welcome_window)
expert_window.title("Expert Endeavour")
expert_window.geometry("850x650")
expert_window.resizable(False, False)
expert_window.withdraw()

scoreboard_window = Toplevel(welcome_window)
scoreboard_window.title("Scoreboard")
scoreboard_window.geometry("800x300")
scoreboard_window.resizable(False, False)
scoreboard_window.withdraw()

leaderboard_window = Toplevel(welcome_window)
leaderboard_window.title("Leaderboard")
leaderboard_window.geometry("800x400")
leaderboard_window.resizable(False, False)
leaderboard_window.withdraw()

# Set up a dictionary to keep track of the scores for each difficulty. 
scores = {"basic": 0, "advanced": 0, "expert": 0}

# Set up a variables to keep track of how many questions have been asked. 
advanced_question_count = 0
expert_question_count = 0

# Create constants which can be used throughout the program for styling.
FONT1 = "Times New Roman"
BACKGROUND_COLOUR = "#e6f2ff"
TEXT_COLOUR = "#002147"
BUTTON_BG_COLOUR = "#004080"
SKIP_BUTTON_COLOUR = "#888888"
RESTART_BUTTON_COLOUR = "#ff6600"
LEADERBOARD_BUTTON_COLOUR = "#009933"
SCOREBOARD_TEXT_BG_COLOUR = "#FFEBD6"


# Create a list which holds the all questions, images and answers for advanced questions.
advanced_question_data = [
    {"question": "What is the area of this shape?", "answer": "24", "image": "q1.png"},
    {"question": "What is the length of side x?", "answer": "5", "image": "q2.png"},
    {"question": "What is x-intercept of this parabola?", "answer": "4", "image": "q3.png"},
    {"question": "What is x-coordinate of the point of inflexion?", "answer": "2", "image": "q4.png"},
    {"question": "What is area of this parallelogram?", "answer": "150", "image": "q5.png"},
    {"question": "What is gradient of this line?", "answer": "2", "image": "q6.png"},
    {"question": "What is y-intercept of this parabola?", "answer": "3", "image": "q7.png"},
    {"question": "What is value of the discriminant for this parabola?", "answer": "0", "image": "q8.png"},
    {"question": "What is volume of this cube?", "answer": "100", "image": "q9.png"},
    {"question": "What is area of this trapezium?", "answer": "20", "image": "q10.png"},
]

# Create a list which holds the all questions, images and answers for expert questions.
expert_question_data = [
    {"question": "Differentiate with respect to x:", "answer": "6", "image": "q11.png"},
    {"question": "What does the following trigonometric identity equal?", "answer": "1", "image": "q12.png"},
    {"question": "What is the value of the following?", "answer": "2", "image": "q13.png"},
    {"question": "Which number makes the trigonometric identity below true?", "answer": "1", "image": "q14.png"},
    {"question": "What is the value of r squared in the equation of this circle?", "answer": "4", "image": "q15.png"},
    {"question": "What is the value of the exact value below?", "answer": "1", "image": "q16.png"},
    {"question": "At which x-value is the gradient of the tangent to this curve equal to 0?", "answer": "3", "image": "q17.png"},
    {"question": "At which x-value is the local minimum?", "answer": "3", "image": "q18.png"},
    {"question": "What is the value of a squared in the equation of this ellipse?", "answer": "9", "image": "q19.png"},
    {"question": "Differentiate with respect to x:", "answer": "0", "image": "q20.png"}
]

# Create main function which can be run to start the program.
def main():
    build_welcome_window()
    build_difficulty_window()
    build_basic_window()
    build_advanced_window()
    build_expert_window()
    build_leaderboard()
    welcome_window.mainloop()

# This function builds the welcome window by creating all its widgets. 
def build_welcome_window():
    global name_entry # Global this variable so it can be used in this function.


    # Create a label and place the background image in it. 
    bg_welcome_image = Image.open("the_counting_kingdom.jpg")
    bg_welcome_image = bg_welcome_image.resize((800, 500))
    bg_welcome_photo = ImageTk.PhotoImage(bg_welcome_image)
    background_label = Label(welcome_window, image=bg_welcome_photo)
    background_label.place(x=0, y=0, relwidth=1, relheight=1)
    background_label.image = bg_welcome_photo
    
    # Display the instructions to the code.
    instructions = Label(welcome_window, text="You are the last hope of The Counting Kingdom. \nAnswer Questions correct to defeat The Mighty Algeblaze!", font=(FONT1, 13), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    instructions.place(relx=0.5, rely=0.55, anchor="center")
    
    # Display the name entry box along with a label which asks the user to enter their name. 
    name_entry = Entry(welcome_window, width=17, font=(FONT1, 12))
    name_entry.place(relx=0.5, rely=0.72, anchor="center")
    enter_name = Label(welcome_window, text="Please enter your name:", font=("Georgia", 12), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    enter_name.place(relx=0.5, rely=0.65, anchor="center")
    
    # Create buttons for the user to start or exit the quiz
    start_button = Button(welcome_window, text="Start", command=name_entry_validation, width=15, borderwidth=2, font=(FONT1, 12), bg=BUTTON_BG_COLOUR, fg="white")
    start_button.place(relx=0.4, rely=0.85, anchor="center")
    exit_button = Button(welcome_window, text="Exit", command=end_program, width=15, borderwidth=2, font=(FONT1, 12), bg=BUTTON_BG_COLOUR, fg="white")
    exit_button.place(relx=0.6, rely=0.85, anchor="center")

# Create a function to hide the welcome window and show the difficulty window.
def show_difficulty_window():
    difficulty_window.deiconify()
    welcome_window.withdraw()

# Create a function which will create the widgets for the basic question window.
def build_difficulty_window():
    
    # Create a label and place the background image in it. 
    bg_difficulty_image = Image.open("kingdom_pathway.jpg")
    bg_difficulty_image = bg_difficulty_image.resize((600, 400))
    bg_difficulty_photo = ImageTk.PhotoImage(bg_difficulty_image)
    bg_difficulty_label = Label(difficulty_window, image=bg_difficulty_photo)
    bg_difficulty_label.image = bg_difficulty_photo
    bg_difficulty_label.place(x=0, y=0, relwidth=1, relheight=1)

    # Display a label to ask the user to choose a difficulty.
    select_difficulty = Label(difficulty_window, text="Please Select a Challenge!", font=(FONT1, 30), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    select_difficulty.pack(pady=20)

    # Display buttons for each difficulty level.
    basic_difficulty = Button(difficulty_window, text="Basic Battle", command=basic_questions, width=20, height=2, borderwidth=2, font=(FONT1, 14), bg=BUTTON_BG_COLOUR, fg="white")
    basic_difficulty.place(relx=0.5, rely=0.40, anchor="center")
    advanced_difficulty = Button(difficulty_window, text="Advanced Academy", command=advanced_questions, width=20, height=2, borderwidth=2, font=(FONT1, 14), bg=BUTTON_BG_COLOUR, fg="white")
    advanced_difficulty.place(relx=0.5, rely=0.6, anchor="center")
    expert_difficulty = Button(difficulty_window, text="Expert Endeavour", command=expert_questions, width=20, height=2, borderwidth=2, font=(FONT1, 14), bg=BUTTON_BG_COLOUR, fg="white")
    expert_difficulty.place(relx=0.5, rely=0.80, anchor="center")

# This function ensures that the name entered by the user is valid.
def name_entry_validation():
    name_check = name_entry.get()
    
    # Check if the user has left their name blank.
    if name_check == "":
        messagebox.showerror(title="Invalid Input", message="You cannot leave your name blank. Try again.")
        name_entry.delete(0, 'end')
        
    # Check if the name entered is more than 15 characters.
    elif len(name_check) > 15:
        messagebox.showerror("Invalid Input", "The entered name cannot exceed 15 characters. Try again.")
        name_entry.delete(0, 'end')
        
    # Check if the name entered only contains alphabet characters.
    elif not name_check.replace(" ", "").replace("-", "").isalpha():
        messagebox.showerror(title="Invalid Input", message="The name entered must only contain letters. Try again.")
        name_entry.delete(0, 'end')
    else:
        # Display the difficulty window as name entered is valid. 
        show_difficulty_window()

# Create a function to save the user's score they gain from basic questions. 
def save_basic_score():
    name = name_entry.get()
    score = scores["basic"]
    
    # Open the .txt file and save the user's name and score.
    with open("score.txt", "a") as file:
        file.write(f"{name} - Basic: {score}\n")

# Create a function to save the user's score they gain from advanced questions. 
def save_advanced_score():
    name = name_entry.get()
    score = scores["advanced"]
    
    # Open the .txt file and save the user's name and score.
    with open("score.txt", "a") as file:
        file.write(f"{name} - Advanced: {score}\n")
        
# Create a function to save the user's score they gain from expert questions. 
def save_expert_score():
    name = name_entry.get()
    score = scores["expert"]
    
    # Open the .txt file and save the user's name and score.
    with open("score.txt", "a") as file:
        file.write(f"{name} - Expert: {score}\n")
        

# Build a scoreboard if the user answered basic questions.
def show_basic_scoreboard():
    # Hide the basic question window and show the scoreboard window. 
    basic_window.withdraw()
    scoreboard_window.deiconify()
    
    # Create score labels, exit button, restart button and leaderboard button.
    basic_score_label = Label(scoreboard_window, font=(FONT1, 20), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    basic_score_label.place(relx=0.5, rely=0.15, anchor="center")
    basic_score_display = Label(scoreboard_window, text=f"Score: {scores['basic']}", font=(FONT1, 20), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    basic_score_display.place(relx=0.5, rely=0.4, anchor="center")
    basic_exit = Button(scoreboard_window, text="Exit", command=end_program, height=1, width=15, borderwidth=2, font=(FONT1, 15), bg=BUTTON_BG_COLOUR, fg="white")
    basic_exit.place(relx=0.35, rely=0.6, anchor="center")
    basic_leaderboard_button = Button(scoreboard_window, text="Leaderboard", command=show_leaderboard, height=1, width=15, borderwidth=2, font=(FONT1, 15), bg=LEADERBOARD_BUTTON_COLOUR, fg="white")
    basic_leaderboard_button.place(relx=0.65, rely=0.6, anchor="center")
    basic_restart_button = Button(scoreboard_window, text="Restart", command=restart, height=1, width=15, borderwidth=2, font=(FONT1, 15), bg=RESTART_BUTTON_COLOUR, fg="white")
    basic_restart_button.place(relx=0.5, rely=0.8, anchor="center")

    # Check if the user's score is equal to or above 70.
    if scores["basic"] >= 70:

        # If the user's score is equal to or above 70, save their score to the .txt file, display a congratulations label and display the image indicating they have won. 
        save_basic_score()
        basic_score_label.config(text=f"Congratulations {name_entry.get()}! \nYou saved The Counting Kingdom by defeating Algeblaze!")

        basic_result_image = Image.open("winner.jpg")
        basic_result_image = basic_result_image.resize((800, 457))
        basic_result_photo = ImageTk.PhotoImage(basic_result_image)
        basic_result_label = Label(scoreboard_window, image=basic_result_photo)
        basic_result_label.place(x=0, y=0, relwidth=1, relheight=1)
        basic_result_label.image = basic_result_photo
        basic_result_label.lower()
        
    else:
        # If it is less than 70, display a sorry label and display image indicating they have lost.
        basic_score_label.config(bg = SCOREBOARD_TEXT_BG_COLOUR, text=f"Sorry {name_entry.get()}! \nYou were not able to save The Counting Kingdom from Algeblaze!")
        basic_score_display.config(bg = SCOREBOARD_TEXT_BG_COLOUR)

        basic_result_image = Image.open("loser.jpg")
        basic_result_image = basic_result_image.resize((800, 457))
        basic_result_photo = ImageTk.PhotoImage(basic_result_image)
        basic_result_label = Label(scoreboard_window, image=basic_result_photo)
        basic_result_label.place(x=0, y=0, relwidth=1, relheight=1)
        basic_result_label.image = basic_result_photo
        basic_result_label.lower()

# Build a scoreboard if the user answered advanced questions.
def show_advanced_scoreboard():
    # Hide the advanced question window and show the scoreboard window. 
    advanced_window.withdraw()
    scoreboard_window.deiconify()
    
    # Create score labels, exit button, restart button and leaderboard button.
    advanced_score_label = Label(scoreboard_window, font=(FONT1, 20), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    advanced_score_label.place(relx=0.5, rely=0.15, anchor="center")
    advanced_score_display = Label(scoreboard_window, text=f"Score: {scores['advanced']}", font=(FONT1, 20), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    advanced_score_display.place(relx=0.5, rely=0.4, anchor="center")
    advanced_exit = Button(scoreboard_window, text="Exit",  command=end_program, height=1, width=15, borderwidth=2, font=(FONT1, 15), bg=BUTTON_BG_COLOUR, fg="white")
    advanced_exit.place(relx=0.35, rely=0.6, anchor="center")
    advanced_leaderboard_button = Button(scoreboard_window, text="Leaderboard", command=show_leaderboard, height=1, width=15, borderwidth=2, font=(FONT1, 15), bg=LEADERBOARD_BUTTON_COLOUR, fg="white")
    advanced_leaderboard_button.place(relx=0.65, rely=0.6, anchor="center")
    advanced_restart_button = Button(scoreboard_window, text="Restart", command=restart, height=1, width=15, borderwidth=2, font=(FONT1, 15), bg=RESTART_BUTTON_COLOUR, fg="white")
    advanced_restart_button.place(relx=0.5, rely=0.8, anchor="center")

    # Check if the user's score is equal to or above 140.
    if scores["advanced"] >= 140:

        # If the user's score is equal to or above 140, save their score to the .txt file, display a congratulations label and display the image indicating they have won. 
        save_advanced_score()
        advanced_score_label.config(text=f"Congratulations {name_entry.get()}! \nYou saved The Counting Kingdom by defeating Algeblaze!")

        advanced_result_image = Image.open("winner.jpg")
        advanced_result_image = advanced_result_image.resize((800, 457))
        advanced_result_photo = ImageTk.PhotoImage(advanced_result_image)
        advanced_result_label = Label(scoreboard_window, image=advanced_result_photo)
        advanced_result_label.place(x=0, y=0, relwidth=1, relheight=1)
        advanced_result_label.image = advanced_result_photo
        advanced_result_label.lower()
        
    else:
        # If it is less than 140, display a sorry label and display image indicating they have lost.
        advanced_score_label.config(bg = SCOREBOARD_TEXT_BG_COLOUR, text=f"Sorry {name_entry.get()}! \nYou were not able to save The Counting Kingdom from Algeblaze!")
        advanced_score_display.config(bg = SCOREBOARD_TEXT_BG_COLOUR)

        advanced_result_image = Image.open("loser.jpg")
        advanced_result_image = advanced_result_image.resize((800, 457))
        advanced_result_photo = ImageTk.PhotoImage(advanced_result_image)
        advanced_result_label = Label(scoreboard_window, image=advanced_result_photo)
        advanced_result_label.place(x=0, y=0, relwidth=1, relheight=1)
        advanced_result_label.image = advanced_result_photo
        advanced_result_label.lower()

# Build a scoreboard if the user answered expert questions.
def show_expert_scoreboard():
    # Hide the expert question window and show the scoreboard window. 
    expert_window.withdraw()
    scoreboard_window.deiconify()
    
    # Create score labels, exit button, restart button and leaderboard button.
    expert_score_label = Label(scoreboard_window, font=(FONT1, 20), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    expert_score_label.place(relx=0.5, rely=0.15, anchor="center")
    expert_score_display = Label(scoreboard_window, text=f"Score: {scores['expert']}", font=(FONT1, 20), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    expert_score_display.place(relx=0.5, rely=0.4, anchor="center")
    expert_exit = Button(scoreboard_window, text="Exit",  command=end_program, height=1, width=15, borderwidth=2, font=(FONT1, 15), bg=BUTTON_BG_COLOUR, fg="white")
    expert_exit.place(relx=0.35, rely=0.6, anchor="center")
    expert_leaderboard_button = Button(scoreboard_window, text="Leaderboard", command=show_leaderboard, height=1, width=15, borderwidth=2, font=(FONT1, 15), bg=LEADERBOARD_BUTTON_COLOUR, fg="white")
    expert_leaderboard_button.place(relx=0.65, rely=0.6, anchor="center")
    expert_restart_button = Button(scoreboard_window, text="Restart", command=restart, height=1, width=15, borderwidth=2, font=(FONT1, 15), bg=RESTART_BUTTON_COLOUR, fg="white")
    expert_restart_button.place(relx=0.5, rely=0.8, anchor="center")

    # Check if the user's score is equal to or above 210.
    if scores["expert"] >= 210:

        # If the user's score is equal to or above 210, save their score to the .txt file, display a congratulations label and display the image indicating they have won. 
        save_expert_score()
        expert_score_label.config(text=f"Congratulations {name_entry.get()}! \nYou saved The Counting Kingdom by defeating Algeblaze!")
        expert_score_display.config(bg = SCOREBOARD_TEXT_BG_COLOUR)

        expert_result_image = Image.open("winner.jpg")
        expert_result_image = expert_result_image.resize((800, 457))
        expert_result_photo = ImageTk.PhotoImage(expert_result_image)
        expert_result_label = Label(scoreboard_window, image=expert_result_photo)
        expert_result_label.place(x=0, y=0, relwidth=1, relheight=1)
        expert_result_label.image = expert_result_photo
        expert_result_label.lower()
        
    else:
        # If it is less than 210, display a sorry label and display image indicating they have lost.
        expert_score_label.config(bg = SCOREBOARD_TEXT_BG_COLOUR, text=f"Sorry {name_entry.get()}! \nYou were not able to save The Counting Kingdom from Algeblaze!")

        expert_result_image = Image.open("loser.jpg")
        expert_result_image = expert_result_image.resize((800, 457))
        expert_result_photo = ImageTk.PhotoImage(expert_result_image)
        expert_result_label = Label(scoreboard_window, image=expert_result_photo)
        expert_result_label.place(x=0, y=0, relwidth=1, relheight=1)
        expert_result_label.image = expert_result_photo
        expert_result_label.lower()

# This function will ask the user if they want to quit the program.  
def end_program():
    confirm = messagebox.askyesno("Exit Quiz", "Are you sure you want to exit the quiz?")
    # If the answer is yes, the program will end.
    if confirm:
        welcome_window.destroy()

# This function will show the basic question window and hide the difficulty window. 
def basic_questions():
    basic_window.deiconify()
    difficulty_window.withdraw()

# This function will ask basic questions along with checking if they are valid and correct. 
def build_basic_window():
    global basic_score, basic_question_count
    basic_question_count = 0

    # Create a label and place the background image in it. 
    bg_basic_image = Image.open("dragon1.jpg")
    bg_basic_image = bg_basic_image.resize((800, 500))
    bg_basic_photo = ImageTk.PhotoImage(bg_basic_image)
    bg_basic_label = Label(basic_window, image=bg_basic_photo)
    bg_basic_label.place(x=0, y=0, relwidth=1, relheight=1)
    bg_basic_label.image = bg_basic_photo

    # This function will generate basic questions. 
    def generate_basic_question():
        global basic_answer

        # Generate two random numbers between 1 and 12 and store the correct answer in a variable. 
        num1 = random.randint(1,12)
        num2 = random.randint(1,12)
        basic_answer = num1 * num2
        
        # Display the question as the product of the two randomly generated numbers. 
        basic_question.config(text=f"What is {num1} x {num2}?")
        
    # This function will the check to see if the user's input is valid and correct.
    def check_basic_answer():
        global basic_question_count
        basic_user_answer = basic_entry.get()
        
        # Check if the user's input was left blank.
        if basic_user_answer == "":
            # Display error message and clear entry box if entry is left blank.
            messagebox.showerror(title="Invalid Input", message="You cannot leave your answer blank. Try again.")
            basic_entry.delete(0, 'end')

        # Check if the user's input is not a number.
        elif not basic_user_answer.isdigit():
            # Display error message and clear entry box if entry is not a number.
            messagebox.showerror(title="Invalid Input", message="Your answer must only contain numbers. Make sure you don't include units! Try again.") 
            basic_entry.delete(0, 'end')
        else: 
            # Check if user's answer is correct.
            if int(basic_user_answer) == basic_answer:

                # Display a message to let the user know their answer is correct but only display the message for 2 seconds. 
                basic_feedback.lift()
                basic_feedback.config(text=" Correct! ", fg="green")
                basic_feedback.after(2000, lambda: (basic_feedback.config(text=""), basic_feedback.lower()))
                basic_question_count += 1

                # Add 10 to the user's score and display the new score on the window. 
                scores["basic"] += 10
                current_basic_score = scores["basic"]
                basic_score.config(text=f"Score: {current_basic_score}")
                basic_entry.delete(0, 'end')

                # Ask a new basic question or display the scoreboard if 10 questions have been asked. 
                if basic_question_count >= 10:
                    show_basic_scoreboard()
                else:
                    generate_basic_question()
            else:
                # Display the message to let ther user know their answer is incorrect but only display the message for 2 seconds. 
                basic_feedback.lift()
                basic_feedback.config(text=" Incorrect. Try Again! ", fg="red")
                basic_feedback.after(2000, lambda: (basic_feedback.config(text=""), basic_feedback.lower()))
                basic_entry.delete(0, 'end')

    # This function will allow the user to skip questions. 
    def skip_basic_question():

        # Increase the basic question count to ensure only 10 questions get asked even if questions are skipped. 
        global basic_question_count
        basic_question_count += 1
        basic_entry.delete(0, 'end')
                
        # Ask a new basic question or display the scoreboard if 10 questions have been asked. 
        if basic_question_count >= 10:
            show_basic_scoreboard()
        else:
            generate_basic_question()

    # Create and setup all the widgets required for the basic window.
    basic_question = Label(basic_window, font=(FONT1, 20), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    basic_question.pack(pady=30)

    basic_entry = Entry(basic_window, font=(FONT1, 30))
    basic_entry.pack(pady=30)

    basic_window.bind("<Return>", lambda event: check_basic_answer())

    basic_feedback = Label(basic_window, font=(FONT1, 15))
    basic_feedback.pack()
    basic_feedback.lower()
    
    basic_submit = Button(basic_window, text="Submit", width=15, font=(FONT1, 15), bg=BUTTON_BG_COLOUR, fg="white", command=check_basic_answer)
    basic_submit.pack(pady=10)
    
    basic_skip = Button(basic_window, text="Skip", width=15, font=(FONT1, 15), bg=SKIP_BUTTON_COLOUR, fg="white", command=skip_basic_question)
    basic_skip.pack(pady=10)

    basic_score = Label(basic_window, text="Score: 0", font=(FONT1, 15), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    basic_score.pack(pady=15)

    # Run the basic check answer function when the user presses submit.
    basic_submit.config(command=check_basic_answer)
    
    # Generate the first basic question to start the quiz.
    generate_basic_question()

# This function will show the advanced question window and hide the difficulty window. 
def advanced_questions():
    advanced_window.deiconify()
    difficulty_window.withdraw()

# This function will ask advanced questions along with checking if they are valid and correct. 
def build_advanced_window():
    global advanced_score, remaining_advanced_questions

    # Create a label and place the background image in it. 
    bg_advanced_image = Image.open("dragon2.jpg")
    bg_advanced_image = bg_advanced_image.resize((850, 650))
    bg_advanced_photo = ImageTk.PhotoImage(bg_advanced_image)
    bg_advanced_label = Label(advanced_window, image=bg_advanced_photo)
    bg_advanced_label.place(x=0, y=0, relwidth=1, relheight=1)
    bg_advanced_label.image = bg_advanced_photo

    # Create a list which contains numbers which consist of the range of the the number of advanced questions. 
    remaining_advanced_questions = list(range(len(advanced_question_data)))

    # This function will display advanced questions.
    def generate_advanced_question():
        global current_advanced_question

        # If all advanced questions have been asked, display the scoreboard. 
        if not remaining_advanced_questions:
            show_advanced_scoreboard()
            return

        # Randomly choose a question to be asked and then remove the possibility of the same question being asked again. 
        current_advanced_question = random.choice(remaining_advanced_questions)
        remaining_advanced_questions.remove(current_advanced_question)

        # Display the randomly chosen question.
        advanced_question_number = advanced_question_data[current_advanced_question]
        advanced_question.config(text=advanced_question_number["question"])
        
        # Display the image for that specific question. 
        img = Image.open(advanced_question_number["image"])
        img = img.resize((350, 250))
        photo = ImageTk.PhotoImage(img)
        advanced_image.config(image=photo)
        advanced_image.image = photo

    # This function will the check to see if the user's input is valid and correct.
    def check_advanced_answer():
        advanced_question_number = advanced_question_data[current_advanced_question]
        advanced_user_answer = advanced_entry.get()

        # Check if the user's input was left blank.
        if advanced_user_answer == "":
            # Display error message and clear entry box if entry is left blank.
            messagebox.showerror(title="Invalid Input", message="You cannot leave your answer blank. Try again.")
            advanced_entry.delete(0, 'end')

        # Check if the user's input is not a number.
        elif not advanced_user_answer.isdigit():
            # Display error message and clear entry box if entry is not a number.
            messagebox.showerror(title="Invalid Input", message="Your answer must only contain numbers. Make sure you don't include units! Try again.")
            advanced_entry.delete(0, 'end')
        else:
            # Check if user's answer is correct.
            if advanced_user_answer == advanced_question_number["answer"]:
                
                # Display a message to let the user know their answer is correct but only display the message for 2 seconds. 
                advanced_feedback.lift()
                advanced_feedback.config(text=" Correct! ", fg="green")
                advanced_feedback.after(2000, lambda: (advanced_feedback.config(text=""), advanced_feedback.lower()))

                # Add 20 to the user's score and display the new score on the window. 
                scores["advanced"] += 20
                current_advanced_score = scores["advanced"]
                advanced_score.config(text=f"Score: {current_advanced_score}")
                advanced_entry.delete(0, 'end')

                # Continue with next question or show scoreboard if done.
                if remaining_advanced_questions:
                    generate_advanced_question()
                else:
                    show_advanced_scoreboard()
            else:
                # Display the message to let ther user know their answer is incorrect but only display the message for 2 second
                advanced_feedback.lift()
                advanced_feedback.config(text=" Incorrect. Try again! ", fg="red")
                advanced_feedback.after(2000, lambda: (advanced_feedback.config(text=""), advanced_feedback.lower()))
                advanced_entry.delete(0, 'end')

    # This function will allow the user to skip questions. 
    def skip_advanced_question():
        global advanced_question_count

        # Increase the advanced question count to ensure only 10 questions get asked even if questions are skipped.
        advanced_question_count += 1
        advanced_entry.delete(0, 'end')

        # Ask a new advanced question or display the scoreboard if 10 questions have been asked. 
        if advanced_question_count >= 10 or not remaining_advanced_questions:
            show_advanced_scoreboard()
        else:
            generate_advanced_question()
        
    # Create and setup all the widgets required for the advanced window.
    advanced_question = Label(advanced_window, font=(FONT1, 20), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    advanced_question.pack(pady=20)

    advanced_image = Label(advanced_window)
    advanced_image.pack(pady=15)

    advanced_entry = Entry(advanced_window, font=(FONT1, 15), bg="white", fg=TEXT_COLOUR)
    advanced_entry.pack(pady=15)

    advanced_window.bind("<Return>", lambda event: check_advanced_answer())

    advanced_feedback = Label(advanced_window, font=(FONT1, 15))
    advanced_feedback.pack()
    advanced_feedback.lower()

    advanced_submit = Button(advanced_window, text="Submit", width=15, borderwidth=2, font=(FONT1, 15), bg=BUTTON_BG_COLOUR, fg="white")
    advanced_submit.pack(pady=15)

    advanced_skip = Button(advanced_window, text="Skip", width=15, font=(FONT1, 15), bg=SKIP_BUTTON_COLOUR, fg="white", command=skip_advanced_question)
    advanced_skip.pack(pady=10)

    advanced_score = Label(advanced_window, text="Score: 0", font=(FONT1, 15), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    advanced_score.pack(pady=15)

    # Run the advanced check answer function when the user presses submit.
    advanced_submit.config(command=check_advanced_answer)
    
    # Generate the first advanced question to begin.
    generate_advanced_question()

# This function will show the expert question window and hide the difficulty window. 
def expert_questions():
    expert_window.deiconify()
    difficulty_window.withdraw()

# This function will ask expert questions along with checking if they are valid and correct. 
def build_expert_window():
    global expert_score, remaining_expert_questions

    # Create a label and place the background image in it.
    bg_expert_image = Image.open("dragon3.jpg")
    bg_expert_image = bg_expert_image.resize((850, 650))
    bg_expert_photo = ImageTk.PhotoImage(bg_expert_image)
    bg_expert_label = Label(expert_window, image=bg_expert_photo)
    bg_expert_label.place(x=0, y=0, relwidth=1, relheight=1)
    bg_expert_label.image = bg_expert_photo

    # Create a list which contains numbers which consist of the range of the the number of expert questions. 
    remaining_expert_questions = list(range(len(expert_question_data)))

    # This function will display expert questions.
    def generate_expert_question():
        global current_expert_question

        # If all expert questions have been asked, display the scoreboard. 
        if not remaining_expert_questions:  
            show_expert_scoreboard()         
            return

        # Randomly choose a question to be asked and then remove the possibility of the same question being asked again. 
        current_expert_question = random.choice(remaining_expert_questions)
        remaining_expert_questions.remove(current_expert_question)

        # Display the randomly chosen question.
        expert_question_number = expert_question_data[current_expert_question]
        expert_question.config(text=expert_question_number["question"])

        # Display the image for that specific question. 
        img = Image.open(expert_question_number["image"])
        img = img.resize((350, 250))
        photo = ImageTk.PhotoImage(img)
        expert_image.config(image=photo)
        expert_image.image = photo

    # This function will the check to see if the user's input is valid and correct.
    def check_expert_answer():
        expert_question_number = expert_question_data[current_expert_question]
        expert_user_answer = expert_entry.get()
        
        # Check if the user's input was left blank.
        if expert_user_answer == "":
            # Display error message and clear entry box if entry is left blank.
            messagebox.showerror(title="Invalid Input", message="You cannot leave your answer blank. Try again.")
            expert_entry.delete(0, 'end')

        # Check if the user's input is not a number.            
        elif not expert_user_answer.isdigit():
            # Display error message and clear entry box if entry is not a number.
            messagebox.showerror(title="Invalid Input", message="Your answer must only contain numbers. Make sure you don't include units! Try again.")
            expert_entry.delete(0, 'end')
        else:
            # Check if user's answer is correct
            if expert_user_answer == expert_question_number["answer"]:
                
                # Display a message to let the user know their answer is correct but only display the message for 2 seconds. 
                expert_feedback.lift()
                expert_feedback.config(text=" Correct! ", fg="green")
                expert_feedback.after(2000, lambda: (expert_feedback.config(text=""), expert_feedback.lower()))

                # Add 20 to the user's score and display the new score on the window. 
                scores["expert"] += 30
                current_expert_score = scores["expert"]
                expert_score.config(text=f"Score: {current_expert_score}")
                expert_entry.delete(0, 'end')

                # Continue with next question or show scoreboard if done.
                if remaining_expert_questions:
                    generate_expert_question()
                else:
                    show_expert_scoreboard()

            else:
                # Display a message to let the user know their answer is correct but only display the message for 2 seconds. 
                expert_feedback.lift()
                expert_feedback.config(text=" Incorrect. Try Again! ", fg="red")
                expert_feedback.after(2000, lambda: (expert_feedback.config(text=""), expert_feedback.lower()))
                expert_entry.delete(0, 'end')

    # This function will allow the user to skip questions. 
    def skip_expert_question():
        global expert_question_count

        # Increase the expert question count to ensure only 10 questions get asked even if questions are skipped.
        expert_question_count += 1
        expert_entry.delete(0, 'end')

        # Ask a new advanced question or display the scoreboard if 10 questions have been asked. 
        if expert_question_count >= 10 or not remaining_expert_questions:
            show_expert_scoreboard()
        else:
            generate_expert_question()
                
    # Create and setup all the widgets required for the advanced window.
    expert_question = Label(expert_window, font=(FONT1, 20), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    expert_question.pack(pady=15)

    expert_image = Label(expert_window)
    expert_image.pack(pady=15)

    expert_entry = Entry(expert_window, font=(FONT1, 15), bg="white", fg=TEXT_COLOUR)
    expert_entry.pack(pady=15)

    expert_window.bind("<Return>", lambda event: check_expert_answer())

    expert_feedback = Label(expert_window, font=(FONT1, 15))
    expert_feedback.pack()
    expert_feedback.lower()

    expert_submit = Button(expert_window, text="Submit", width=15, borderwidth=2, font=(FONT1, 15), bg=BUTTON_BG_COLOUR, fg="white")
    expert_submit.pack(pady=15)

    expert_skip = Button(expert_window, text="Skip", font=(FONT1, 15), width=15, bg=SKIP_BUTTON_COLOUR, fg="white", command=skip_expert_question)
    expert_skip.pack(pady=10)   
    
    expert_score = Label(expert_window, text="Score: 0", font=(FONT1, 15), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    expert_score.pack(pady=15)

    # Run the expert check answer function when the user presses submit.
    expert_submit.config(command=check_expert_answer)
    
    # Generate the first expert question to begin.
    generate_expert_question()

# This function will create the widgets on the leaderboard window.
def build_leaderboard():
    global bg_leaderboard_label, leaderboard_heading

    # This function will hide the leaderboard window and show the scoreboard window.
    def back():
        leaderboard_window.withdraw()
        scoreboard_window.deiconify()

    # Create a label and place the background image in it. 
    bg_leaderboard_image = Image.open("leaderboard.jpg")
    bg_leaderboard_image = bg_leaderboard_image.resize((800, 500))
    bg_leaderboard_photo = ImageTk.PhotoImage(bg_leaderboard_image)
    bg_leaderboard_label = Label(leaderboard_window, image=bg_leaderboard_photo)
    bg_leaderboard_label.place(x=0, y=0, relwidth=1, relheight=1)
    bg_leaderboard_label.image = bg_leaderboard_photo
    bg_leaderboard_label.lower()

    
    # Create a heading label for the leadership window.
    leaderboard_heading = Label(leaderboard_window, text="The Counting Kingdom's Saviours", font=(FONT1, 25), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
    leaderboard_heading.pack(pady=20)

    # Read scores from the file and display them.
    with open("score.txt", "r") as file:
        scores = file.readlines()
        for line in scores:
            # Display the scored in the label created below.
            leaderboard_score_label = Label(leaderboard_window, text=line.strip(), font=(FONT1, 15), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
            leaderboard_score_label.pack()

    # Create the back button to return to the scoreboard.
    back_button = Button(leaderboard_window, text="Back to Scoreboard", command=back, width=15, borderwidth=2, font=(FONT1, 15), bg=BUTTON_BG_COLOUR, fg="white")
    back_button.pack(pady=20)

# This function will show the leaderboard.
def show_leaderboard():
    leaderboard_window.deiconify()
    scoreboard_window.withdraw()


    for widget in leaderboard_window.winfo_children():
        # Destroy all widgets in leaderboard window, except the leaderboard background label and leaderboard heading.
        if isinstance(widget, Label) and widget not in [bg_leaderboard_label, leaderboard_heading]:
            widget.destroy()

        # Destroy all buttons in leaderboard window.
        if isinstance(widget, Button):
            widget.destroy()

    # Read scores from the file and display them.
    with open("score.txt", "r") as file:
        for line in file:
            # Display the scored in the label created below.
            leaderboard_score = Label(leaderboard_window, text=line.strip(), font=(FONT1, 15), bg=BACKGROUND_COLOUR, fg=TEXT_COLOUR)
            leaderboard_score.pack()

    # Create the back button to return to the scoreboard.
    leaderboard_back = Button(leaderboard_window, text="Back to Scoreboard", command=lambda: [leaderboard_window.withdraw(), scoreboard_window.deiconify()], width=15, borderwidth=2, font=(FONT1, 15), bg=BUTTON_BG_COLOUR, fg="white")
    leaderboard_back.pack(pady=20)

# This function will restart the entire program.
def restart():
    # The user will be asked if they want to restart.
    confirm_restart = messagebox.askyesno("Restart Quiz", "Are you sure you want to restart the program?")
    if confirm_restart:
        global scores, basic_question_count, advanced_question_count, expert_question_count
        global remaining_advanced_questions, remaining_expert_questions
        global remaining_advanced_questions, remaining_expert_questions
        global basic_score, advanced_score, expert_score

        # If they say yes the scoreboard will be hidden and the welcome window will be shown. 
        scoreboard_window.withdraw()
        welcome_window.deiconify()
        name_entry.delete(0, 'end')

        # All the score labels will try to be reset to 0.
        try:
            basic_score.config(text="Score: 0")
        except NameError:
            pass

        try:
            advanced_score.config(text="Score: 0")
        except NameError:
            pass

        try:
            expert_score.config(text="Score: 0")
        except NameError:
            pass

        for widget in scoreboard_window.winfo_children():
            if isinstance(widget, Label):
                widget.destroy()

        # All the question counts and scores will be reset.
        scores = {"basic": 0, "advanced": 0, "expert": 0}
        basic_question_count = 0
        advanced_question_count = 0
        expert_question_count = 0

        # Reset the variables which ensure the same question doesn't get asked twice.
        remaining_advanced_questions = list(range(len(advanced_question_data)))
        remaining_expert_questions = list(range(len(expert_question_data)))


# Run the program by running the main function. 
main()
