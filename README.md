'''
Author: Tej Patel
Date Created: 06/06/2025
Last Date Modified: 21/07/2025
Purpose: Create a math quiz program to test the user on a variety of math questions.
'''

from tkinter import *
from tkinter import messagebox
import random
from PIL import Image, ImageTk

scores = {
    "basic": 0,
    "advanced": 0,
    "expert": 0
}

welcome_window = Tk()
welcome_window.title("Welcome to the Marvelous Math Quiz")
welcome_window.geometry("800x500")
welcome_window.configure(bg="#e6f2ff")
welcome_window.resizable(False, False)

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


def build_welcome_window():
    global name_entry
    welcome = Label(welcome_window, text="Welcome User!", font=("Times New Roman", 40), bg="#e6f2ff", fg="#002147")
    welcome.pack(pady=20)
    instructions = Label(welcome_window, text="Try your best to answer the questions. Good Luck!", font=("Georgia", 20), bg="#e6f2ff", fg="#002147")
    instructions.pack(pady=30)
    start_button = Button(welcome_window, text="Start", command=name_entry_validation, width=15, borderwidth=2, font=("Times New Roman", 12), bg="#004080", fg="white")
    start_button.place(relx=0.4, rely=0.8, anchor="center")
    exit_button = Button(welcome_window, text="Exit", command=end_program, width=15, borderwidth=2, font=("Times New Roman", 12), bg="#004080", fg="white")
    exit_button.place(relx=0.6, rely=0.8, anchor="center")
    name_entry = Entry(welcome_window, width=17, font=("Times New Roman", 12))
    name_entry.place(relx=0.5, rely=0.65, anchor="center")
    enter_name = Label(welcome_window, text="Please enter your name:", font=("Georgia", 15), bg="#e6f2ff", fg="#002147")
    enter_name.place(relx=0.5, rely=0.5, anchor="center")

def build_difficulty_window():
    difficulty_window.deiconify()
    welcome_window.withdraw()
    select_difficulty = Label(difficulty_window, text="Please a Select Difficulty!", font=("Times New Roman", 30), bg="#e6f2ff", fg="#002147")
    select_difficulty.pack(pady=30)
    basic_difficulty = Button(difficulty_window, text="Basic", command=basic_questions, width=20, height=2, borderwidth=2, font=("Times New Roman", 12), bg="#004080", fg="white")
    basic_difficulty.place(relx=0.5, rely=0.40, anchor="center")
    advanced_difficulty = Button(difficulty_window, text="Advanced", command=advanced_questions, width=20, height=2, borderwidth=2, font=("Times New Roman", 12), bg="#004080", fg="white")
    advanced_difficulty.place(relx=0.5, rely=0.6, anchor="center")
    expert_difficulty = Button(difficulty_window, text="Expert", command=expert_questions, width=20, height=2, borderwidth=2, font=("Times New Roman", 12), bg="#004080", fg="white")
    expert_difficulty.place(relx=0.5, rely=0.80, anchor="center")

def name_entry_validation():
    name_check = name_entry.get()
    if name_check == "":
        messagebox.showerror(title="Invalid Input", message="You cannot leave your name blank. Try again.")
        name_entry.delete(0, 'end')
    elif not name_check.isalpha():
        messagebox.showerror(title="Invalid Input", message="The name entered must only contain letters. Try again.")
        name_entry.delete(0, 'end')
    else:
        build_difficulty_window()
        
            
    
def show_basic_scoreboard():
    basic_window.withdraw()
    scoreboard_window.deiconify()

    basic_score_label = Label(scoreboard_window, text="Congratulations User! You completed the Marvelous Math Quiz", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    basic_score_label.pack(pady=20)

    basic_score_display = Label(scoreboard_window, text=f"Score: {scores['basic']}", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    basic_score_display.pack(pady=20)

    basic_exit = Button(scoreboard_window, text="Exit", command=end_program, font=("Times New Roman", 20), bg="#004080", fg="white")
    basic_exit.pack(pady=20)
    
def show_advanced_scoreboard():
    advanced_window.withdraw()
    scoreboard_window.deiconify()
    
    advanced_score_label = Label(scoreboard_window, text="Congratulations User! You completed the Marvelous Math Quiz", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    advanced_score_label.pack(pady=20)

    advanced_score_display = Label(scoreboard_window, text=f"Score: {scores['advanced']}", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    advanced_score_display.pack(pady=20)

    advanced_exit = Button(scoreboard_window, text="Exit",  command=end_program, font=("Times New Roman", 20), bg="#004080", fg="white")
    advanced_exit.pack(pady=20)

def show_expert_scoreboard():
    expert_window.withdraw()
    scoreboard_window.deiconify()

    expert_score_label = Label(scoreboard_window, text="Congratulations User! You completed the Marvelous Math Quiz", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    expert_score_label.pack(pady=20)

    expert_score_display = Label(scoreboard_window, text=f"Score: {scores['expert']}", font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    expert_score_display.pack(pady=20)

    expert_exit = Button(scoreboard_window, text="Exit",  command=end_program, font=("Times New Roman", 20), bg="#004080", fg="white")
    expert_exit.pack(pady=20)
    
def end_program():
    welcome_window.destroy()

def basic_questions():
    global basic_question_count
    basic_question_count = 0

    def generate_basic_question():
        global basic_answer
        num1 = random.randint(1,12)
        num2 = random.randint(1,12)
        basic_answer = num1 * num2
        basic_question.config(text=f"What is {num1} x {num2}?")

    def check_basic_answer():
        global basic_question_count
        basic_user_answer = basic_entry.get()
        if basic_user_answer == "":
            messagebox.showerror(title="Invalid Input", message="You cannot leave your answer blank. Try again.")
            basic_entry.delete(0, 'end')
        elif not basic_user_answer.isdigit():
            messagebox.showerror(title="Invalid Input", message="Your answer must only contain numbers. Try again.")
            basic_entry.delete(0, 'end')
        else: 
            if int(basic_user_answer) == basic_answer:
                messagebox.showinfo(title="Correct", message="Correct Answer. Good Job!")
                basic_question_count += 1
                scores["basic"] += 10
                current_basic_score = scores["basic"]
                basic_score.config(text=f"Score: {current_basic_score}")
                basic_entry.delete(0, 'end')

                if basic_question_count >= 10:
                    show_basic_scoreboard()

                else:
                    generate_basic_question()
            else:
                messagebox.showerror(title="Incorrect", message="Incorrect Answer. Try Again!")
                basic_entry.delete(0, 'end')

            
    basic_window.deiconify()
    difficulty_window.withdraw()

    basic_question = Label(basic_window, font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    basic_question.pack(pady=30)

    basic_entry = Entry(basic_window, font=("Times New Roman", 30))
    basic_entry.pack(pady=30)

    basic_submit = Button(basic_window, text="Submit", width=15, height=1, borderwidth=2, font=("Times New Roman", 15), bg="#004080", fg="white")
    basic_submit.pack(pady=30)

    basic_score = Label(basic_window, text="Score: 0", font=("Times New Roman", 15), bg="#e6f2ff", fg="#002147")
    basic_score.pack(pady=15)

    basic_submit.config(command=check_basic_answer)
    
    generate_basic_question()

    
    
    
def advanced_questions():
    advanced_window.deiconify()
    difficulty_window.withdraw()

    advanced_question_data = [
        {"question": "What is the area of this shape?", "answer": "24", "image": "q1.png"},
        {"question": "What is the length of side x?", "answer": "5", "image": "q2.png"},
        {"question": "What is x-intercept of this parabola?", "answer": "4", "image": "q3.png"},
        {"question": "What is x-coordinate of the point of inflexion?", "answer": "2", "image": "q4.png"},
        ]

    global remaining_advanced_questions
    remaining_advanced_questions = list(range(len(advanced_question_data)))

    def generate_advanced_question():
        global current_advanced_question

        current_advanced_question = random.choice(remaining_advanced_questions)
        remaining_advanced_questions.remove(current_advanced_question)

        advanced_question_number = advanced_question_data[current_advanced_question]
        advanced_question.config(text=advanced_question_number["question"])

        img = Image.open(advanced_question_number["image"])
        img = img.resize((400, 300))
        photo = ImageTk.PhotoImage(img)
        advanced_image.config(image=photo)
        advanced_image.image = photo

    def check_advanced_answer():
        advanced_question_number = advanced_question_data[current_advanced_question]
        advanced_user_answer = advanced_entry.get()

        if advanced_user_answer == "":
            messagebox.showerror(title="Invalid Input", message="You cannot leave your answer blank. Try again.")
            advanced_entry.delete(0, 'end')
        elif not advanced_user_answer.isdigit():
            messagebox.showerror(title="Invalid Input", message="Your answer must only contain numbers. Try again.")
            advanced_entry.delete(0, 'end')
        else: 
            if advanced_user_answer == advanced_question_number["answer"]:
                messagebox.showinfo(title="Correct", message="Correct Answer. Good Job!")
                scores["advanced"] += 20
                current_advanced_score = scores["advanced"]
                advanced_score.config(text=f"Score: {current_advanced_score}")
                advanced_entry.delete(0, 'end')

                if remaining_advanced_questions:
                    generate_advanced_question()
                else:
                    show_advanced_scoreboard()
                    
            else:
                messagebox.showerror(title="Incorrect", message="Incorrect Answer. Try again.")
                advanced_entry.delete(0, 'end')
        

    advanced_question = Label(advanced_window, font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    advanced_question.pack(pady=20)

    advanced_image = Label(advanced_window)
    advanced_image.pack(pady=15)

    advanced_entry = Entry(advanced_window, font=("Times New Roman", 15), bg="#e6f2ff", fg="#002147")
    advanced_entry.pack(pady=15)

    advanced_submit = Button(advanced_window, text="Submit", width=15, height=1, borderwidth=2, font=("Times New Roman", 15), bg="#004080", fg="white")
    advanced_submit.pack(pady=15)

    advanced_score = Label(advanced_window, text="Score: 0", font=("Times New Roman", 15), bg="#e6f2ff", fg="#002147")
    advanced_score.pack(pady=15)

    advanced_submit.config(command=check_advanced_answer)
    
    generate_advanced_question()

    

def expert_questions():
    expert_window.deiconify()
    difficulty_window.withdraw()

    expert_question_data = [
        {"question": "thht", "answer": "1", "image": "q1.png"},
        {"question": "thht", "answer": "1", "image": "q2.png"}
        ]

    global remaining_expert_questions
    remaining_expert_questions = list(range(len(expert_question_data)))

    def generate_expert_question():
        global current_expert_question

        current_expert_question = random.choice(remaining_expert_questions)
        remaining_expert_questions.remove(current_expert_question)

        expert_question_number = expert_question_data[current_expert_question]
        expert_question.config(text=expert_question_number["question"])

        img = Image.open(expert_question_number["image"])
        img = img.resize((400, 300))
        photo = ImageTk.PhotoImage(img)
        expert_image.config(image=photo)
        expert_image.image = photo

    def check_expert_answer():
        expert_question_number = expert_question_data[current_expert_question]
        expert_user_answer = expert_entry.get()
        if expert_user_answer == "":
            messagebox.showerror(title="Invalid Input", message="You cannot leave your answer blank. Try again.")
            expert_entry.delete(0, 'end')
        elif not expert_user_answer.isdigit():
            messagebox.showerror(title="Invalid Input", message="Your answer must only contain numbers. Try again.")
            expert_entry.delete(0, 'end')
        else: 
            if expert_user_answer == expert_question_number["answer"]:
                messagebox.showinfo(title="Correct", message="Correct Answer. Good Job!")
                scores["expert"] += 30
                current_expert_score = scores["expert"]
                expert_score.config(text=f"Score: {current_expert_score}")
                expert_entry.delete(0, 'end')

                if remaining_expert_questions:
                    generate_expert_question()
                else:
                    show_expert_scoreboard()

            else:
                messagebox.showerror(title="Incorrect", message="Incorrect Answer. Try again.")
                expert_entry.delete(0, 'end')
                

    expert_question = Label(expert_window, font=("Times New Roman", 20), bg="#e6f2ff", fg="#002147")
    expert_question.pack(pady=15)

    expert_image = Label(expert_window)
    expert_image.pack(pady=15)

    expert_entry = Entry(expert_window, font=("Times New Roman", 15))
    expert_entry.pack(pady=15)

    expert_submit = Button(expert_window, text="Submit", width=15, height=1, borderwidth=2, font=("Times New Roman", 15), bg="#004080", fg="white")
    expert_submit.pack(pady=15)

    expert_score = Label(expert_window, text="Score: 0", font=("Times New Roman", 15), bg="#e6f2ff", fg="#002147")
    expert_score.pack(pady=15)

    expert_submit.config(command=check_expert_answer)
    
    generate_expert_question()



build_welcome_window()
welcome_window.mainloop()
