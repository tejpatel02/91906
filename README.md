'''
Author: Tej Patel
Date Created: 06/06/2025
Last Date Modified: 26/06/2025
Purpose: Create a math quiz program to test the user on a variety of math questions.
'''

from tkinter import *
from tkinter import messagebox
import random


welcome_window = Tk()
welcome_window.title("Welcome to the Marvelous Math Quiz")
welcome_window.geometry("800x500")

difficulty_window = Tk()
difficulty_window.title("Select Difficulty")
difficulty_window.geometry("600x400") 
difficulty_window.withdraw()

basic_window = Tk()
basic_window.title("Basic Questions")
basic_window.geometry("800x500")
basic_window.withdraw()


def build_welcome_window():
    welcome = Label(welcome_window, text="Welcome User!", font=("Times New Roman", 40))
    welcome.pack(pady=20)
    instructions = Label(welcome_window, text="Try your best to answer the questions. Good Luck!", font=("Georgia", 20))
    instructions.pack(pady=30)
    start_button = Button(welcome_window, text="Start", command=build_difficulty_window, width=15, borderwidth=2, font=("Times New Roman", 12))
    start_button.place(relx=0.4, rely=0.8, anchor="center")
    exit_button = Button(welcome_window, text="Exit", command=end_program, width=15, borderwidth=2, font=("Times New Roman", 12))
    exit_button.place(relx=0.6, rely=0.8, anchor="center")
    name_entry = Entry(welcome_window, width=17, font=("Times New Roman", 12))
    name_entry.place(relx=0.5, rely=0.65, anchor="center")
    enter_name = Label(welcome_window, text="Please enter your name:", font=("Georgia", 15))
    enter_name.place(relx=0.5, rely=0.5, anchor="center")

def build_difficulty_window():
    difficulty_window.deiconify()
    welcome_window.withdraw()
    select_difficulty = Label(difficulty_window, text="Please a Select Difficulty!", font=("Times New Roman", 30))
    select_difficulty.pack(pady=30)
    basic_difficulty = Button(difficulty_window, text="Basic", command=basic_questions, width=20, height=2, borderwidth=2, font=("Times New Roman", 12))
    basic_difficulty.place(relx=0.5, rely=0.40, anchor="center")
    advanced_difficulty = Button(difficulty_window, text="Advanced", command=advanced_questions, width=20, height=2, borderwidth=2, font=("Times New Roman", 12))
    advanced_difficulty.place(relx=0.5, rely=0.6, anchor="center")
    expert_difficulty = Button(difficulty_window, text="Expert", command=expert_questions, width=20, height=2, borderwidth=2, font=("Times New Roman", 12))
    expert_difficulty.place(relx=0.5, rely=0.80, anchor="center")
    


def end_program():
    welcome_window.destroy()
    difficulty_window.destroy()
    basic_window.destroy()

def basic_questions():

    def generate_basic_question():
        global basic_answer
        num1 = random.randint(1,12)
        num2 = random.randint(1,12)
        basic_answer = num1 * num2
        basic_question.config(text=f"What is {num1} x {num2}?")

    def check_basic_answer():
        basic_user_answer = basic_entry.get()
        if basic_user_answer == "":
            messagebox.showerror(title="Invalid", message="You cannot leave this field blank")
        elif int(basic_user_answer) == basic_answer:
            messagebox.showinfo(title="Correct", message="Correct Answer. Good Job!")
            generate_basic_question()
        else:
            messagebox.showerror(title="Incorrect", message="Incorrect Answer. Try Again!")
        basic_entry.delete(0, 'end')

            
    basic_window.deiconify()
    difficulty_window.withdraw()

    basic_question = Label(basic_window, font=("Times New Roman", 30))
    basic_question.pack(pady=30)

    basic_entry = Entry(basic_window, font=("Times New Roman", 30))
    basic_entry.pack(pady=30)

    basic_submit = Button(basic_window, text="Submit", font=("Times New Roman", 30))
    basic_submit.pack(pady=30)

    exit_button = Button(basic_window, text="Exit", command=end_program, width=15, borderwidth=2, font=("Times New Roman", 12))

    basic_submit.config(command=check_basic_answer)

    generate_basic_question()

    
    
    
    

def advanced_questions():
    print("")

def expert_questions():
    print("")



build_welcome_window()
welcome_window.mainloop()

