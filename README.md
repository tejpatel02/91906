'''
Author: Tej Patel
Date Created: 06/06/2025
Last Date Modified: 21/07/2025
Purpose: Create a math quiz program to test the user on a variety of math questions.
'''

# Import all necessary libraries for GUI and image handling.
from tkinter import *
from tkinter import messagebox
import random
from PIL import Image, ImageTk

# Set up a dictionary to keep track of scores for different difficulty levels.
scores = {
    "basic": 0,
    "advanced": 0,
    "expert": 0
}

# Set up counters for advanced and expert questions to control the question limit.
advanced_question_count = 0
expert_question_count = 0

# Create all the required windows for the GUI, with initial settings.
welcome_window = Tk()
welcome_window.title("Welcome to the Marvelous Math Quiz")
welcome_window.geometry("800x500")
welcome_window.configure(bg="#e6f2ff")
welcome_window.resizable(False, False)

# Additional windows are created but hidden at first.
difficulty_window = Toplevel(welcome_window)
difficulty_window.title("Select Difficulty")
difficulty_window.geometry("600x400")
difficulty_window.configure(bg="#e6f2ff")
difficulty_window.resizable(False, False)
difficulty_window.withdraw()

basic_window = Toplevel(welcome_window)
basic_window.title("Basic Questions")
basic_window.geometry("800x500")
basic_window.configure(bg="#e6f2ff")
basic_window.resizable(False, False)
basic_window.withdraw()

advanced_window = Toplevel(welcome_window)
advanced_window.title("Advanced Questions")
advanced_window.geometry("800x600")
advanced_window.configure(bg="#e6f2ff")
advanced_window.resizable(False, False)
advanced_window.withdraw()

expert_window = Toplevel(welcome_window)
expert_window.title("Expert Questions")
expert_window.geometry("800x600")
expert_window.configure(bg="#e6f2ff")
expert_window.resizable(False, False)
expert_window.withdraw()

scoreboard_window = Toplevel(welcome_window)
scoreboard_window.title("Scoreboard")
scoreboard_window.geometry("800x300")
scoreboard_window.configure(bg="#e6f2ff")
scoreboard_window.resizable(False, False)
scoreboard_window.withdraw()

leaderboard_window = Toplevel(welcome_window)
leaderboard_window.title("Leaderboard")
leaderboard_window.geometry("800x400")
leaderboard_window.configure(bg="#e6f2ff")
leaderboard_window.resizable(False, False)
leaderboard_window.withdraw()

def build_welcome_window():  # This function creates the welcome window and all its widgets.
    global name_entry
    
    # Display welcome messages and buttons for starting or exiting the quiz.
    welcome = Label(welcome_window, text="Welcome!", font=("Times New Roman", 40), bg="#e6f2ff", fg="#002147")
    welcome.pack(pady=20)
    instructions = Label(welcome_window, text="Try your best to answer the questions. Good Luck!", font=("Georgia", 20), bg="#e6f2ff", fg="#002147")
    instructions.pack(pady=30)
    
    # Entry field for the user's name.
    name_entry = Entry(welcome_window, width=17, font=("Times New Roman", 12))
    name_entry.place(relx=0.5, rely=0.65, anchor="center")
    enter_name = Label(welcome_window, text="Please enter your name:", font=("Georgia", 15), bg="#e6f2ff", fg="#002147")
    enter_name.place(relx=0.5, rely=0.5, anchor="center")
    
    # Buttons to start the quiz or exit the program.
    start_button = Button(welcome_window, text="Start", command=name_entry_validation, width=15, borderwidth=2, font=("Times New Roman", 12), bg="#004080", fg="white")
    start_button.place(relx=0.4, rely=0.8, anchor="center")
    exit_button = Button(welcome_window, text="Exit", command=end_program, width=15, borderwidth=2, font=("Times New Roman", 12), bg="#004080", fg="white")
    exit_button.place(relx=0.6, rely=0.8, anchor="center")

def build_difficulty_window():  # This function builds the difficulty selection screen.
    difficulty_window.deiconify()
    welcome_window.withdraw()

    # Display difficulty options for the quiz.
    select_difficulty = Label(difficulty_window, text="Please a Select Difficulty!", font=("Times New Roman", 30), bg="#e6f2ff", fg="#002147")
    select_difficulty.pack(pady=30)

    # Buttons for each difficulty level.
    basic_difficulty = Button(difficulty_window, text="Basic", command=basic_questions, width=20, height=2, borderwidth=2, font=("Times New Roman", 12), bg="#004080", fg="white")
    basic_difficulty.place(relx=0.5, rely=0.40, anchor="center")
    advanced_difficulty = Button(difficulty_window, text="Advanced", command=advanced_questions, width=20, height=2, borderwidth=2, font=("Times New Roman", 12), bg="#004080", fg="white")
    advanced_difficulty.place(relx=0.5, rely=0.6, anchor="center")
    expert_difficulty = Button(difficulty_window, text="Expert", command=expert_questions, width=20, height=2, borderwidth=2, font=("Times New Roman", 12), bg="#004080", fg="white")
    expert_difficulty.place(relx=0.5, rely=0.80, anchor="center")

def name_entry_validation():  # This function validates the name input before starting the quiz.
    name_check = name_entry.get()
    if name_check == "":
        messagebox.showerror(title="Invalid Input", message="You cannot leave your name blank. Try again.")
        name_entry.delete(0, 'end')
    elif not name_check.replace(" ", "").replace("-", "").isalpha():
        messagebox.showerror(title="Invalid Input", message="The name entered must only contain letters. Try again.")
        name_entry.delete(0, 'end')
    else:
        build_difficulty_window()

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
    save_basic_score()
    basic_score_label = Label(scoreboard_window, text=f"Congratulations {name_entry.get()}! You completed the Marvelous Math Quiz", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    basic_score_label.pack(pady=20)
    basic_score_display = Label(scoreboard_window, text=f"Score: {scores['basic']}", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    basic_score_display.pack(pady=20)
    basic_exit = Button(scoreboard_window, text="Exit", command=end_program, font=("Times New Roman", 20), bg="#004080", fg="white")
    basic_exit.pack(pady=20)
    leaderboard_button = Button(scoreboard_window, text="Leaderboard", command=show_leaderboard, font=("Times New Roman", 15), bg="#009933", fg="white")
    leaderboard_button.pack(pady=10)

def show_advanced_scoreboard():
    advanced_window.withdraw()
    scoreboard_window.deiconify()
    save_advanced_score()
    advanced_score_label = Label(scoreboard_window, text=f"Congratulations {name_entry.get()}! You completed the Marvelous Math Quiz", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    advanced_score_label.pack(pady=20)
    advanced_score_display = Label(scoreboard_window, text=f"Score: {scores['advanced']}", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    advanced_score_display.pack(pady=20)
    advanced_exit = Button(scoreboard_window, text="Exit",  command=end_program, font=("Times New Roman", 20), bg="#004080", fg="white")
    advanced_exit.pack(pady=20)
    leaderboard_button = Button(scoreboard_window, text="Leaderboard", command=show_leaderboard, font=("Times New Roman", 15), bg="#009933", fg="white")
    leaderboard_button.pack(pady=10)

    
def show_expert_scoreboard():
    expert_window.withdraw()
    scoreboard_window.deiconify()
    save_expert_score()
    expert_score_label = Label(scoreboard_window, text=f"Congratulations {name_entry.get()}! You completed the Marvelous Math Quiz", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    expert_score_label.pack(pady=20)
    expert_score_display = Label(scoreboard_window, text=f"Score: {scores['expert']}", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    expert_score_display.pack(pady=20)
    expert_exit = Button(scoreboard_window, text="Exit",  command=end_program, font=("Times New Roman", 20), bg="#004080", fg="white")
    expert_exit.pack(pady=20)
    leaderboard_button = Button(scoreboard_window, text="Leaderboard", command=show_leaderboard, font=("Times New Roman", 15), bg="#009933", fg="white")
    leaderboard_button.pack(pady=10)

    
def end_program():
    # Ends the entire program by closing the welcome window and exiting the application.
    welcome_window.destroy()

def basic_questions():
    # Initialize counter for number of basic questions answered/skipped.
    global basic_question_count
    basic_question_count = 0

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
                messagebox.showinfo(title="Correct", message="Correct Answer. Good Job!")
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
                messagebox.showerror(title="Incorrect", message="Incorrect Answer. Try Again!")
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
            
    # Prepare the basic questions window and widgets.
    basic_window.deiconify()
    difficulty_window.withdraw()

    # Setup GUI widgets for basic questions window.
    basic_question = Label(basic_window, font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    basic_question.pack(pady=30)

    basic_entry = Entry(basic_window, font=("Times New Roman", 30))
    basic_entry.pack(pady=30)
    
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

    # List of advanced-level question dictionaries with question, answer, and image.
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

    # Create a list to track which questions have not been asked yet.
    global remaining_advanced_questions
    remaining_advanced_questions = list(range(len(advanced_question_data)))

    def generate_advanced_question():
        # Randomly select an advanced question from remaining questions.
        global current_advanced_question
        if not remaining_advanced_questions:  # Check if list is empty
            show_advanced_scoreboard()         # Show scoreboard if no questions left
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
                messagebox.showinfo(title="Correct", message="Correct Answer. Good Job!")
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
                messagebox.showerror(title="Incorrect", message="Incorrect Answer. Try again.")
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

    # Create a list to track which questions have not been asked yet.
    global remaining_expert_questions
    remaining_expert_questions = list(range(len(expert_question_data)))

    def generate_expert_question():
        # Randomly select an expert question from remaining questions.
        global current_expert_question
        if not remaining_expert_questions:  # Check if list is empty
            show_expert_scoreboard()         # Show scoreboard if no questions left
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
                messagebox.showinfo(title="Correct", message="Correct Answer. Good Job!")
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
                messagebox.showerror(title="Incorrect", message="Incorrect Answer. Try again.")
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

    expert_entry = Entry(expert_window, font=("Times New Roman", 15))
    expert_entry.pack(pady=15)

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

def show_leaderboard():
    # Show leaderboard window and hide the scoreboard
    leaderboard_window.deiconify()
    scoreboard_window.withdraw()

    # Heading label
    heading = Label(leaderboard_window, text="Marvelous Math Quiz Leaderboard", font=("Times New Roman", 25), bg="#e6f2ff", fg="#002147")
    heading.pack(pady=20)

    # Read scores from the file and display them
    with open("score.txt", "r") as file:
        scores = file.readlines()
        for line in scores:
            score_label = Label(leaderboard_window, text=line.strip(), font=("Times New Roman", 15), bg="#e6f2ff", fg="#002147")
            score_label.pack()

    # Exit button to end program
    exit_button = Button(leaderboard_window, text="Exit", command=end_program, font=("Times New Roman", 15), bg="#004080", fg="white")
    exit_button.pack(pady=20)



# Run the code by creating the welcome window and running the mainloop. 
build_welcome_window()
welcome_window.mainloop()
