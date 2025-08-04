'''
Author: Tej Patel
Date Created: 06/06/2025
Last Date Modified: 03/08/2025
Purpose: Create a math quiz program to test the user on a variety of math questions.
'''

# Import all necessary libraries for GUI and image handling.
from tkinter import *
from tkinter import messagebox
import random
from PIL import Image, ImageTk, ImageSequence

# Create all the required windows for the GUI, with initial settings.
welcome_window = Tk()
welcome_window.title("Welcome to The Counting Kingdom")
welcome_window.geometry("800x500")
welcome_window.resizable(False, False)

# Additional windows are created but hidden at first.
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

# Set up a dictionary to keep track of scores for different difficulty levels.
scores = {"basic": 0, "advanced": 0, "expert": 0}

# Set up counters for advanced and expert questions to control the question limit.
advanced_question_count = 0
expert_question_count = 0

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

# List of expert-level question dictionaries with question, answer, and image.
expert_question_data = [
    {"question": "Differentiate with respect to x:", "answer": "6", "image": "q11.png"},
    {"question": "What does the following trigonometric identity equal?", "answer": "1", "image": "q12.png"},
    {"question": "Which number makes the equation below true?", "answer": "90", "image": "q13.png"},
    {"question": "Which number makes the trigonometric identity below true?", "answer": "1", "image": "q14.png"},
    {"question": "What is the value of r squared in the equation of this circle?", "answer": "4", "image": "q15.png"},
    {"question": "What is the value of the exact value below?", "answer": "1", "image": "q16.png"},
    {"question": "At which x-value is the gradient of the tangent to this curve equal to 0?", "answer": "3", "image": "q17.png"},
    {"question": "At which x-value is the local minimum?", "answer": "3", "image": "q18.png"},
    {"question": "What is the value of a squared in the equation of this ellipse?", "answer": "9", "image": "q19.png"},
    {"question": "Differentiate with respect to x:", "answer": "0", "image": "q20.png"}
]

def main():
    build_welcome_window()
    build_difficulty_window()
    build_basic_window()
    build_advanced_window()
    build_expert_window()
    build_leaderboard()
    welcome_window.mainloop()

def build_welcome_window():  # This function creates the welcome window and all its widgets.
    global name_entry

    bg_welcome_image = Image.open("the_counting_kingdom.jpg")
    bg_welcome_image = bg_welcome_image.resize((800, 500))
    bg_welcome_photo = ImageTk.PhotoImage(bg_welcome_image)
    background_label = Label(welcome_window, image=bg_welcome_photo)
    background_label.place(x=0, y=0, relwidth=1, relheight=1)
    background_label.image = bg_welcome_photo
    
    # Display welcome messages and buttons for starting or exiting the quiz.
    instructions = Label(welcome_window, text="You are the last hope of The Counting Kingdom. \nAnswer Questions correct to defeat The Mighty Algeblaze!", font=("Georgia", 13), bg="#e6f2ff", fg="#002147")
    instructions.place(relx=0.5, rely=0.55, anchor="center")
    
    # Entry field for the user's name.
    name_entry = Entry(welcome_window, width=17, font=("Times New Roman", 12))
    name_entry.place(relx=0.5, rely=0.72, anchor="center")
    enter_name = Label(welcome_window, text="Please enter your name:", font=("Georgia", 12), bg="#e6f2ff", fg="#002147")
    enter_name.place(relx=0.5, rely=0.65, anchor="center")
    
    # Buttons to start the quiz or exit the program.
    start_button = Button(welcome_window, text="Start", command=name_entry_validation, width=15, borderwidth=2, font=("Times New Roman", 12), bg="#004080", fg="white")
    start_button.place(relx=0.4, rely=0.85, anchor="center")
    exit_button = Button(welcome_window, text="Exit", command=end_program, width=15, borderwidth=2, font=("Times New Roman", 12), bg="#004080", fg="white")
    exit_button.place(relx=0.6, rely=0.85, anchor="center")

    welcome_window.bind('<Return>', lambda event: name_entry_validation())

def show_difficulty_window():  # This function builds the difficulty selection screen.
    difficulty_window.deiconify()
    welcome_window.withdraw()

def build_difficulty_window():

    bg_difficulty_image = Image.open("kingdom_pathway.jpg")  # Replace with your image filename
    bg_difficulty_image = bg_difficulty_image.resize((600, 400))      # Resize to window size
    bg_difficulty_photo = ImageTk.PhotoImage(bg_difficulty_image)

    # Create label for background image
    bg_difficulty_label = Label(difficulty_window, image=bg_difficulty_photo)
    bg_difficulty_label.image = bg_difficulty_photo  # Keep a reference so image isn't garbage collected
    bg_difficulty_label.place(x=0, y=0, relwidth=1, relheight=1)

    # Display difficulty options for the quiz.
    select_difficulty = Label(difficulty_window, text="Please Select a Challenge!", font=("Times New Roman", 30), bg="#e6f2ff", fg="#002147")
    select_difficulty.pack(pady=20)

    # Buttons for each difficulty level.
    basic_difficulty = Button(difficulty_window, text="Basic Battle", command=basic_questions, width=20, height=2, borderwidth=2, font=("Times New Roman", 14), bg="#004080", fg="white")
    basic_difficulty.place(relx=0.5, rely=0.40, anchor="center")
    advanced_difficulty = Button(difficulty_window, text="Advanced Academy", command=advanced_questions, width=20, height=2, borderwidth=2, font=("Times New Roman", 14), bg="#004080", fg="white")
    advanced_difficulty.place(relx=0.5, rely=0.6, anchor="center")
    expert_difficulty = Button(difficulty_window, text="Expert Endeavour", command=expert_questions, width=20, height=2, borderwidth=2, font=("Times New Roman", 14), bg="#004080", fg="white")
    expert_difficulty.place(relx=0.5, rely=0.80, anchor="center")

def name_entry_validation():  # This function validates the name input before starting the quiz.
    name_check = name_entry.get()
    if name_check == "":
        messagebox.showerror(title="Invalid Input", message="You cannot leave your name blank. Try again.")
        name_entry.delete(0, 'end')
    elif len(name_check) > 15:
        messagebox.showerror("Invalid Input", "The entered name cannot exceed 15 characters. Try again.")
        name_entry.delete(0, 'end')
    elif not name_check.replace(" ", "").replace("-", "").isalpha():
        messagebox.showerror(title="Invalid Input", message="The name entered must only contain letters. Try again.")
        name_entry.delete(0, 'end')
    else:
        
        show_difficulty_window()

def end_program():  # This function ends the entire quiz program.
    welcome_window.destroy()

def save_basic_score():
    name = name_entry.get()
    score = scores["basic"]
    with open("score.txt", "a") as file:
        file.write(f"{name} - Basic: {score}\n")

def save_advanced_score():
    name = name_entry.get()
    score = scores["advanced"]
    with open("score.txt", "a") as file:
        file.write(f"{name} - Advanced: {score}\n")

def save_expert_score():
    name = name_entry.get()
    score = scores["expert"]
    with open("score.txt", "a") as file:
        file.write(f"{name} - Expert: {score}\n")
        

# Scoreboard functions show final score and allow exit.
def show_basic_scoreboard():
    basic_window.withdraw()
    scoreboard_window.deiconify()
    basic_score_label = Label(scoreboard_window, font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    basic_score_label.place(relx=0.5, rely=0.15, anchor="center")
    basic_score_display = Label(scoreboard_window, text=f"Score: {scores['basic']}", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    basic_score_display.place(relx=0.5, rely=0.4, anchor="center")
    basic_exit = Button(scoreboard_window, text="Exit", command=end_program, height=1, width=15, borderwidth=2, font=("Times New Roman", 15), bg="#004080", fg="white")
    basic_exit.place(relx=0.35, rely=0.6, anchor="center")
    basic_leaderboard_button = Button(scoreboard_window, text="Leaderboard", command=show_leaderboard, height=1, width=15, borderwidth=2, font=("Times New Roman", 15), bg="#009933", fg="white")
    basic_leaderboard_button.place(relx=0.65, rely=0.6, anchor="center")
    basic_restart_button = Button(scoreboard_window, text="Restart", command=restart, height=1, width=15, borderwidth=2, font=("Times New Roman", 15), bg="#ff6600", fg="white")
    basic_restart_button.place(relx=0.5, rely=0.8, anchor="center")
    
    if scores["basic"] >= 70:
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
        basic_score_label.config(text=f"Sorry {name_entry.get()}! \nYou were not able to save The Counting Kingdom from Algeblaze!")

        basic_result_image = Image.open("loser.jpg")
        basic_result_image = basic_result_image.resize((800, 457))
        basic_result_photo = ImageTk.PhotoImage(basic_result_image)
        basic_result_label = Label(scoreboard_window, image=basic_result_photo)
        basic_result_label.place(x=0, y=0, relwidth=1, relheight=1)
        basic_result_label.image = basic_result_photo
        basic_result_label.lower()

def show_advanced_scoreboard():
    advanced_window.withdraw()
    scoreboard_window.deiconify()
    advanced_score_label = Label(scoreboard_window, font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    advanced_score_label.place(relx=0.5, rely=0.15, anchor="center")
    advanced_score_display = Label(scoreboard_window, text=f"Score: {scores['advanced']}", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    advanced_score_display.place(relx=0.5, rely=0.4, anchor="center")
    advanced_exit = Button(scoreboard_window, text="Exit",  command=end_program, height=1, width=15, borderwidth=2, font=("Times New Roman", 15), bg="#004080", fg="white")
    advanced_exit.place(relx=0.35, rely=0.6, anchor="center")
    advanced_leaderboard_button = Button(scoreboard_window, text="Leaderboard", command=show_leaderboard, height=1, width=15, borderwidth=2, font=("Times New Roman", 15), bg="#009933", fg="white")
    advanced_leaderboard_button.place(relx=0.65, rely=0.6, anchor="center")
    advanced_restart_button = Button(scoreboard_window, text="Restart", command=restart, height=1, width=15, borderwidth=2, font=("Times New Roman", 15), bg="#ff6600", fg="white")
    advanced_restart_button.place(relx=0.5, rely=0.8, anchor="center")
    if scores["advanced"] >= 140:
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
        advanced_score_label.config(text=f"Sorry {name_entry.get()}! \nYou were not able to save The Counting Kingdom from Algeblaze!")

        advanced_result_image = Image.open("loser.jpg")
        advanced_result_image = advanced_result_image.resize((800, 457))
        advanced_result_photo = ImageTk.PhotoImage(advanced_result_image)
        advanced_result_label = Label(scoreboard_window, image=advanced_result_photo)
        advanced_result_label.place(x=0, y=0, relwidth=1, relheight=1)
        advanced_result_label.image = advanced_result_photo
        advanced_result_label.lower()
    
def show_expert_scoreboard():
    expert_window.withdraw()
    scoreboard_window.deiconify()
    expert_score_label = Label(scoreboard_window, font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    expert_score_label.place(relx=0.5, rely=0.15, anchor="center")
    expert_score_display = Label(scoreboard_window, text=f"Score: {scores['expert']}", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    expert_score_display.place(relx=0.5, rely=0.4, anchor="center")
    expert_exit = Button(scoreboard_window, text="Exit",  command=end_program, height=1, width=15, borderwidth=2, font=("Times New Roman", 15), bg="#004080", fg="white")
    expert_exit.place(relx=0.35, rely=0.6, anchor="center")
    expert_leaderboard_button = Button(scoreboard_window, text="Leaderboard", command=show_leaderboard, height=1, width=15, borderwidth=2, font=("Times New Roman", 15), bg="#009933", fg="white")
    expert_leaderboard_button.place(relx=0.65, rely=0.6, anchor="center")
    expert_restart_button = Button(scoreboard_window, text="Restart", command=restart, height=1, width=15, borderwidth=2, font=("Times New Roman", 15), bg="#ff6600", fg="white")
    expert_restart_button.place(relx=0.5, rely=0.8, anchor="center")
    if scores["expert"] >= 210:
        save_expert_score()
        expert_score_label.config(text=f"Congratulations {name_entry.get()}! \nYou saved The Counting Kingdom by defeating Algeblaze!")

        expert_result_image = Image.open("winner.jpg")
        expert_result_image = expert_result_image.resize((800, 457))
        expert_result_photo = ImageTk.PhotoImage(expert_result_image)
        expert_result_label = Label(scoreboard_window, image=expert_result_photo)
        expert_result_label.place(x=0, y=0, relwidth=1, relheight=1)
        expert_result_label.image = expert_result_photo
        expert_result_label.lower()
        
    else:
        expert_score_label.config(text=f"Sorry {name_entry.get()}! \nYou were not able to save The Counting Kingdom from Algeblaze!")

        expert_result_image = Image.open("loser.jpg")
        expert_result_image = expert_result_image.resize((800, 457))
        expert_result_photo = ImageTk.PhotoImage(expert_result_image)
        expert_result_label = Label(scoreboard_window, image=expert_result_photo)
        expert_result_label.place(x=0, y=0, relwidth=1, relheight=1)
        expert_result_label.image = expert_result_photo
        expert_result_label.lower()
    
def end_program():
    # Ends the entire program by closing the welcome window and exiting the application.
    confirm = messagebox.askyesno("Exit Program", "Are you sure you want to exit?")
    if confirm:
        welcome_window.destroy()

def basic_questions():
    basic_window.deiconify()
    difficulty_window.withdraw()

def build_basic_window():
    
    # Initialize counter for number of basic questions answered/skipped.
    global basic_question_count
    basic_question_count = 0

    bg_basic_image = Image.open("dragon1.jpg")
    bg_basic_image = bg_basic_image.resize((800, 500))
    bg_basic_photo = ImageTk.PhotoImage(bg_basic_image)
    bg_basic_label = Label(basic_window, image=bg_basic_photo)
    bg_basic_label.place(x=0, y=0, relwidth=1, relheight=1)
    bg_basic_label.image = bg_basic_photo

    def generate_basic_question():
        # Generate a random multiplication question with two numbers between 1 and 12.
        global basic_answer
        num1 = random.randint(1,12)
        num2 = random.randint(1,12)
        basic_answer = num1 * num2
        # Display the generated question on the basic question label.
        basic_question.config(text=f"What is {num1} x {num2}?")

    def check_basic_answer(): # This function will the check to see if the user's input is valid and correct.
        global basic_question_count
        basic_user_answer = basic_entry.get()
        
        # Validate that the input is not empty and is a number.
        if basic_user_answer == "":
            messagebox.showerror(title="Invalid Input", message="You cannot leave your answer blank. Try again.")
            basic_entry.delete(0, 'end')
        elif not basic_user_answer.isdigit():
            messagebox.showerror(title="Invalid Input", message="Your answer must only contain numbers. Try again.")
            basic_entry.delete(0, 'end')
        else: 
            # Check if user's answer matches the correct answer.
            if int(basic_user_answer) == basic_answer:
                # If correct, update score and question display.
                basic_feedback.lift()
                basic_feedback.config(text="✅ Correct!", fg="green")
                basic_feedback.after(2000, lambda: (basic_feedback.config(text=""), basic_feedback.lower()))
                basic_question_count += 1
                scores["basic"] += 10
                current_basic_score = scores["basic"]
                basic_score.config(text=f"Score: {current_basic_score}")
                basic_entry.delete(0, 'end')

                # Continue with next question or show scoreboard if done.
                if basic_question_count >= 10:
                    show_basic_scoreboard()
                else:
                    generate_basic_question()
            else:
                # If answer is incorrect, show error and let user try again.
                basic_feedback.lift()
                basic_feedback.config(text="❌ Incorrect", fg="red")
                basic_feedback.after(2000, lambda: (basic_feedback.config(text=""), basic_feedback.lower()))
                basic_entry.delete(0, 'end')

    def skip_basic_question():
        # Allow skipping basic questions and track the number of questions that have been asked.
        global basic_question_count
        basic_question_count += 1
        basic_entry.delete(0, 'end')
                
        # If 10 questions have been answered/skipped, show scoreboard.
        if basic_question_count >= 10:
            show_basic_scoreboard()
        else:
            generate_basic_question()

    # Setup GUI widgets for basic questions window.
    basic_question = Label(basic_window, font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    basic_question.pack(pady=30)

    basic_entry = Entry(basic_window, font=("Times New Roman", 30))
    basic_entry.pack(pady=30)

    basic_window.bind("<Return>", lambda event: check_basic_answer())

    basic_feedback = Label(basic_window, font=("Times New Roman", 15))
    basic_feedback.pack()
    basic_feedback.lower()
    
    basic_submit = Button(basic_window, text="Submit", width=15, font=("Times New Roman", 15), bg="#004080", fg="white", command=check_basic_answer)
    basic_submit.pack(pady=10)
    
    basic_skip = Button(basic_window, text="Skip", width=15, font=("Times New Roman", 15), bg="#888888", fg="white", command=skip_basic_question)
    basic_skip.pack(pady=10)

    basic_score = Label(basic_window, text="Score: 0", font=("Times New Roman", 15), bg="#e6f2ff", fg="#002147")
    basic_score.pack(pady=15)

    # Run the badsic check answer function when the user presses submit.
    basic_submit.config(command=check_basic_answer)
    
    # Generate the first basic question to start the quiz.
    generate_basic_question()


def advanced_questions():
    # Show the advanced questions window and hide difficulty selection.
    advanced_window.deiconify()
    difficulty_window.withdraw()

def build_advanced_window():

    bg_advanced_image = Image.open("dragon2.jpg")
    bg_advanced_image = bg_advanced_image.resize((850, 650))
    bg_advanced_photo = ImageTk.PhotoImage(bg_advanced_image)
    bg_advanced_label = Label(advanced_window, image=bg_advanced_photo)
    bg_advanced_label.place(x=0, y=0, relwidth=1, relheight=1)
    bg_advanced_label.image = bg_advanced_photo

    # Create a list to track which questions have not been asked yet.
    global remaining_advanced_questions
    remaining_advanced_questions = list(range(len(advanced_question_data)))

    def generate_advanced_question():
        # Randomly select an advanced question from remaining questions.
        global current_advanced_question
        if not remaining_advanced_questions:
            show_advanced_scoreboard()
            return
        current_advanced_question = random.choice(remaining_advanced_questions)
        remaining_advanced_questions.remove(current_advanced_question)

        # Retrieve question data and display question.
        advanced_question_number = advanced_question_data[current_advanced_question]
        advanced_question.config(text=advanced_question_number["question"])
        
        # Load, resize, and display the question image.
        img = Image.open(advanced_question_number["image"])
        img = img.resize((350, 250))
        photo = ImageTk.PhotoImage(img)
        advanced_image.config(image=photo)
        advanced_image.image = photo

    def check_advanced_answer(): # This function will the check to see if the user's input is valid and correct.
        
        # Validate and check the advanced level answer.
        advanced_question_number = advanced_question_data[current_advanced_question]
        advanced_user_answer = advanced_entry.get()

        # Validate user input is not blank and is a number.
        if advanced_user_answer == "":
            messagebox.showerror(title="Invalid Input", message="You cannot leave your answer blank. Try again.")
            advanced_entry.delete(0, 'end')
        elif not advanced_user_answer.isdigit():
            messagebox.showerror(title="Invalid Input", message="Your answer must only contain numbers. Try again.")
            advanced_entry.delete(0, 'end')
        else: 
            # Check if user's answer matches the correct answer.
            if advanced_user_answer == advanced_question_number["answer"]:
                # If correct, update score and question display.
                advanced_feedback.lift()
                advanced_feedback.config(text="✅ Correct!", fg="green")
                advanced_feedback.after(2000, lambda: (advanced_feedback.config(text=""), advanced_feedback.lower()))
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
                # Show error message for incorrect answer.
                advanced_feedback.lift()
                advanced_feedback.config(text="❌ Incorrect", fg="red")
                advanced_feedback.after(2000, lambda: (advanced_feedback.config(text=""), advanced_feedback.lower()))
                advanced_entry.delete(0, 'end')

    def skip_advanced_question():
        # Allow skipping advanced questions and track the number of questions that have been asked.
        global advanced_question_count
        advanced_question_count += 1
        advanced_entry.delete(0, 'end')

        # After 10 skips or questions, show scoreboard; else next question.
        if advanced_question_count >= 10 or not remaining_advanced_questions:
            show_advanced_scoreboard()
        else:
            generate_advanced_question()
        
    # Setup GUI widgets for advanced questions window.
    advanced_question = Label(advanced_window, font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    advanced_question.pack(pady=20)

    advanced_image = Label(advanced_window)
    advanced_image.pack(pady=15)

    advanced_entry = Entry(advanced_window, font=("Times New Roman", 15), bg="white", fg="#002147")
    advanced_entry.pack(pady=15)

    advanced_window.bind("<Return>", lambda event: check_advanced_answer())

    advanced_feedback = Label(advanced_window, font=("Times New Roman", 15))
    advanced_feedback.pack()
    advanced_feedback.lower()

    advanced_submit = Button(advanced_window, text="Submit", width=15, borderwidth=2, font=("Times New Roman", 15), bg="#004080", fg="white")
    advanced_submit.pack(pady=15)

    advanced_skip = Button(advanced_window, text="Skip", width=15, font=("Times New Roman", 15), bg="#888888", fg="white", command=skip_advanced_question)
    advanced_skip.pack(pady=10)

    advanced_score = Label(advanced_window, text="Score: 0", font=("Times New Roman", 15), bg="#e6f2ff", fg="#002147")
    advanced_score.pack(pady=15)

    # Run the advanced check answer function when the user presses submit.
    advanced_submit.config(command=check_advanced_answer)
    
    # Generate the first advanced question to begin
    generate_advanced_question()


def expert_questions():
    # Show the expert questions window and hide the difficulty selection.
    expert_window.deiconify()
    difficulty_window.withdraw()

def build_expert_window():

    bg_expert_image = Image.open("dragon3.jpg")
    bg_expert_image = bg_expert_image.resize((850, 650))
    bg_expert_photo = ImageTk.PhotoImage(bg_expert_image)
    bg_expert_label = Label(expert_window, image=bg_expert_photo)
    bg_expert_label.place(x=0, y=0, relwidth=1, relheight=1)
    bg_expert_label.image = bg_expert_photo

    # Create a list to track which questions have not been asked yet.
    global remaining_expert_questions
    remaining_expert_questions = list(range(len(expert_question_data)))

    def generate_expert_question():
        # Randomly select an expert question from remaining questions.
        global current_expert_question
        if not remaining_expert_questions:  
            show_expert_scoreboard()         
            return
        current_expert_question = random.choice(remaining_expert_questions)
        remaining_expert_questions.remove(current_expert_question)

        # Retrieve question data and display question.
        expert_question_number = expert_question_data[current_expert_question]
        expert_question.config(text=expert_question_number["question"])

        # Load, resize, and display the question image.
        img = Image.open(expert_question_number["image"])
        img = img.resize((350, 250))
        photo = ImageTk.PhotoImage(img)
        expert_image.config(image=photo)
        expert_image.image = photo

    def check_expert_answer(): # This function will the check to see if the user's input is valid and correct.
        # Validate and check the expert level answer.
        expert_question_number = expert_question_data[current_expert_question]
        expert_user_answer = expert_entry.get()
        # Validate that the input is not empty and is a number.
        if expert_user_answer == "":
            messagebox.showerror(title="Invalid Input", message="You cannot leave your answer blank. Try again.")
            expert_entry.delete(0, 'end')
        elif not expert_user_answer.isdigit():
            messagebox.showerror(title="Invalid Input", message="Your answer must only contain numbers. Try again.")
            expert_entry.delete(0, 'end')
        else:
            # Check if user's answer matches the correct answer.
            if expert_user_answer == expert_question_number["answer"]:
                # If correct, update score and question display.
                expert_feedback.lift()
                expert_feedback.config(text="✅ Correct!", fg="green")
                expert_feedback.after(2000, lambda: (expert_feedback.config(text=""), expert_feedback.lower()))
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
                # Show error message for incorrect answer.
                expert_feedback.lift()
                expert_feedback.config(text="❌ Incorrect", fg="red")
                expert_feedback.after(2000, lambda: (expert_feedback.config(text=""), expert_feedback.lower()))
                expert_entry.delete(0, 'end')

    def skip_expert_question():
        # Allow skipping expert questions and track the number of questions that have been asked.
        global expert_question_count
        expert_question_count += 1
        expert_entry.delete(0, 'end')

        # After 10 questions/skips, show the scoreboard; else next question.
        if expert_question_count >= 10 or not remaining_expert_questions:
            show_expert_scoreboard()
        else:
            generate_expert_question()
                
    # Setup GUI widgets for expert questions window.
    expert_question = Label(expert_window, font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    expert_question.pack(pady=15)

    expert_image = Label(expert_window)
    expert_image.pack(pady=15)

    expert_entry = Entry(expert_window, font=("Times New Roman", 15), bg="white", fg="#002147")
    expert_entry.pack(pady=15)

    expert_window.bind("<Return>", lambda event: check_expert_answer())

    expert_feedback = Label(expert_window, font=("Times New Roman", 15))
    expert_feedback.pack()
    expert_feedback.lower()

    expert_submit = Button(expert_window, text="Submit", width=15, borderwidth=2, font=("Times New Roman", 15), bg="#004080", fg="white")
    expert_submit.pack(pady=15)

    expert_skip = Button(expert_window, text="Skip", font=("Times New Roman", 15), width=15, bg="#888888", fg="white", command=skip_expert_question)
    expert_skip.pack(pady=10)   
    
    expert_score = Label(expert_window, text="Score: 0", font=("Times New Roman", 15), bg="#e6f2ff", fg="#002147")
    expert_score.pack(pady=15)

    # Run the expert check answer function when the user presses submit.
    expert_submit.config(command=check_expert_answer)
    
    # Generate the first expert question to begin.
    generate_expert_question()


def build_leaderboard():
    global bg_leaderboard_label, leaderboard_heading
    
    bg_leaderboard_image = Image.open("leaderboard.jpg")
    bg_leaderboard_image = bg_leaderboard_image.resize((800, 500))
    bg_leaderboard_photo = ImageTk.PhotoImage(bg_leaderboard_image)
    bg_leaderboard_label = Label(leaderboard_window, image=bg_leaderboard_photo)
    bg_leaderboard_label.place(x=0, y=0, relwidth=1, relheight=1)
    bg_leaderboard_label.image = bg_leaderboard_photo

    # Heading label
    leaderboard_heading = Label(leaderboard_window, text="The Counting Kingdom's Saviours", font=("Times New Roman", 25), bg="#e6f2ff", fg="#002147")
    leaderboard_heading.pack(pady=20)


    # Read scores from the file and display them
    with open("score.txt", "r") as file:
        scores = file.readlines()
        for line in scores:
            leaderboard_score_label = Label(leaderboard_window, text=line.strip(), font=("Times New Roman", 15), bg="#e6f2ff", fg="#002147")
            leaderboard_score_label.pack()

    def back():
        leaderboard_window.withdraw()
        scoreboard_window.deiconify()

    # Exit button to end program
    back_button = Button(leaderboard_window, text="Back to Scoreboard", command=back, width=15, borderwidth=2, font=("Times New Roman", 15), bg="#004080", fg="white")
    back_button.pack(pady=20)

def show_leaderboard():
    leaderboard_window.deiconify()
    scoreboard_window.withdraw()

    # Remove just the score labels (leave heading, bg, and buttons alone)
    for widget in leaderboard_window.winfo_children():
        if isinstance(widget, Label) and widget not in [bg_leaderboard_label, leaderboard_heading]:
            widget.destroy()

    with open("score.txt", "r") as file:
        for line in file:
            Label(leaderboard_window, text=line.strip(), font=("Times New Roman", 15), bg="#e6f2ff", fg="#002147").pack()



def restart():
    global scores, basic_question_count, advanced_question_count, expert_question_count
    global remaining_advanced_questions, remaining_expert_questions
    global remaining_advanced_questions, remaining_expert_questions

    scoreboard_window.withdraw()
    welcome_window.deiconify()
    name_entry.delete(0, 'end')

    for widget in scoreboard_window.winfo_children():
        if isinstance(widget, Label):
            widget.destroy()

    # Reset scores and counters
    scores = {"basic": 0, "advanced": 0, "expert": 0}
    basic_question_count = 0
    advanced_question_count = 0
    expert_question_count = 0

    # Reset question trackers
    remaining_advanced_questions = list(range(len(advanced_question_data)))
    remaining_expert_questions = list(range(len(expert_question_data)))


# Run the code by creating the welcome window and running the mainloop. 
main()
