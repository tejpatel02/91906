'''
Author: Tej Patel
#Date: 9th May 2025
#Purpose: To create the first draft of the Math Quiz

ver.1.3.0
 - Images
 
ver.1.2.0
- Pop Up Windows


ver.1.1.0
- Validity Checking
    - Special Letters (Symbol)
    - Blanks
    - Boundaries
    - Alphabet
'''

import random
from tkinter import *
from tkinter import PhotoImage

num = [1, 2, 3, 4, 5, 6, 7, 8, 9]

def create_intro_window(): # Not done yet
    intro_window = Tk()
    intro_window.title("Welcome!")
    intro_window.geometry("400x400")

def create_error_window(error_message):
    error_window = Toplevel(app)
    error_window.title("Warning")
    error_window.geometry("200x150")
    error_window.resizable(True, True)
    error_window_warning = Label (error_window, text = error_message, font = ("Times New Roman", 16))
    error_window_warning.place (relx=0.3, rely=0.1)

    error_window_warning.config(text=error_message)
    

def input_validator(user_input):

    # Blanks
    if user_input == "":
        create_error_window("Blank!")
        blank = Label(app, text = "Blank!", fg = "red", font = ("Times New Roman", 16))
        blank.place(relx = 0.4, rely = 0.1)
       
    # Alphabet, Whitespace, and Symbols
    elif user_input.isdigit() == False:
        if user_input.isalnum() == True:
            create_error_window("No Alphabet!")
            alphabet = Label(app, text = "No Alphabet!", fg = "red", font = ("Times New Roman", 16))
            alphabet.place(relx = 0.4, rely = 0.1)
           
        elif " " in user_input:
            create_error_window("Whitespace!")
            whitespace = Label(app, text = "Whitespace!", fg = "red", font = ("Times New Roman", 16))
            whitespace.place(relx = 0.4, rely = 0.1)
           
        else:
            create_error_window("Symbols!")
            symbols = Label(app, text = "Symbols!", fg = "red", font = ("Times New Roman", 16))
            symbols.place(relx = 0.4, rely = 0.1)
           
    # Boundaries
    elif len(user_input) > 6:
        upper_limit = Label(app, text = "Character limit has been reached!", fg = "red", font = ("Times New Roman", 16))
        upper_limit.place(relx = 0.2, rely = 0.1)
    else:
        return True
    return False

def submt(user_entry):
    user_input = user_entry.get()

    validity = input_validator(user_input)

    if validity == True:
    
        if user_input == str(resultPlus()):
            correct = Label(app, text = "Correct!", fg = "green", font = ("Times New Roman", 16))
            correct.place(relx = 0.4, rely = 0.2)
        else:
            wrong = Label(app, text = "Wrong!", fg = "red", font = ("Times New Roman", 16))
            wrong.place(relx = 0.4, rely = 0.2)
    print("Checks Done!")

def try_again():
    try_again.num1update = random.choice(num)
    try_again.num2update = random.choice(num)
    question = Label(
        app, text = f"{try_again.num1update}+{try_again.num2update}", font = ("Times New Roman", 16)
    )
    question.place(relx = 0.16, rely = 0.14, relwidth = 0.7, relheight = 0.23)

def resultPlus():
    try_again

    return try_again.num1update + try_again.num2update

app = Tk()
app.title("Maths Quiz")
app.geometry("900x500")
app.resizable(True, True)
button_img = PhotoImage(file="Start_Button.png")
cool_button = Button(app, image = button_img)
cool_button.place(relx=0.33, rely=0.2, relwidth=2, relheight=2)


#start = Button(app, text = "Begin Adventure", bg = "blue", fg = "white", font = "50", command = try_again)
#start.place(relx = 0.33, rely = 0.2, relwidth = 0.4, relheight = 0.15)

solving = Entry(app)
solving.place(relx = 0.35, rely = 0.4, relwidth = 0.34, relheight = 0.23)

submit = Button(app, text = "Submit", command = lambda: submt(solving))
submit.place(relx = 0.35, rely = 0.64, relwidth = 0.34, relheight = 0.23)

try_again = Button(app, text = "Try Again", command = try_again)
try_again.place(relx = 0.43, rely = 0.9)

app.mainloop()
