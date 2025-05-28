# 91906
# This project will look at developing a computer program that solves a certain problem.
'''
Author: Tej Patel
#Date: 9th May 2025
#Purpose: To create the first draft of the Math Quiz
'''

import random
from tkinter import *

num = [1, 2, 3, 4, 5, 6, 7, 8,9 ]

def submt(var1):
    if var1.get() == str(resultPLUS()):
        correct = Label(app, text="Correct!", fg="green", font=("Courier", 16))
        correct.place(relx = 0.3, rely = 0.2)
    else:
        wrong = Label(app, text="Wrong!", fg="green", font=("Courier", 16))
        wrong.place(relx=0.3, rely=0.2)

def try_again():
    try_again.num1update = random.choice(num)
    try_again.num2update = random.choice(num)
    question = Label(app, text=f"{try_again.num1update} + {try_again.num2update}", font=("Courier", 16))
    question.place(relx=0.16, rely=0.16, relwidth=0.7, relheight=0.23)

def resultPLUS():
    try_again
    return try_again.num1update + try_again.num2update

    

app = Tk()
app.title("Maths4Minors")
app.geometry("250x300")
app.resizable(False,False)

start = Button(app, text="Begin Adventure", command=try_again)
start.place(relx=0.22, rely=0.1, relwidth = 0.54, relheight = 0.23)

solving = Entry(app)
solving.place(relx=0.35, rely=0.4, relwidth=0.34, relheight=0.23)

submit = Button(app, text="Submit", command=lambda: submt(solving))
submit.place(relx=0.35, rely=0.64, relwidth=0.34, relheight=0.23)

try_again = Button(app, text="Try Again", command=try_again)
try_again.place(relx=0.39, rely=0.9)
app.mainloop()
