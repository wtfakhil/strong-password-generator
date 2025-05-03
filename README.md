# Password Generator

This Python script creates a simple GUI application using Tkinter to generate strong passwords and copy them to the clipboard.

## Dependencies

* **tkinter:** For creating the graphical user interface. (Usually comes pre-installed with Python)
* **string:** Provides access to string constants (e.g., uppercase letters, lowercase letters, digits, punctuation). (Part of the Python standard library)
* **secrets:** Generates cryptographically strong random numbers for password generation. (Part of the Python standard library)
* **random:** Generates pseudo-random numbers (Not used with secrets for security-critical password generation, but present in the code).
* **pyperclip:** Allows the script to copy the generated password to the clipboard. You can install it using pip:

    ```bash
    pip install pyperclip
    ```

## Functionality

1.  **Password Generation (`generator()` function):**
    * Defines character sets: uppercase letters, lowercase letters, digits, and punctuation.
    * Gets the desired password length from the user input (a `Spinbox`).
    * Calculates the number of characters to use from each set (approximately 30% uppercase/digits and 20% punctuation/lowercase).
    * Generates the password by randomly choosing characters from the defined sets using `secrets.choice()`.
    * Inserts the generated password into an `Entry` field for display.
2.  **Copy to Clipboard (`copy()` function):**
    * Retrieves the generated password from the `Entry` field.
    * Copies the password to the clipboard using `pyperclip.copy()`.
3.  **GUI Creation:**
    * Creates the main window (`root`) using `tkinter.Tk()`.
    * Sets the window size and background color.
    * Creates labels to display text ("Password Generator," "Password Length").
    * Creates a `Spinbox` to allow the user to select the desired password length (from 8 to 32 characters).
    * Creates a "Generate" button that calls the `generator()` function when clicked.
    * Creates an `Entry` field to display the generated password.
    * Creates a "Copy to Clipboard" button that calls the `copy()` function when clicked.
    * Starts the main event loop (`root.mainloop()`) to display the GUI and respond to user interactions.

## Code Explanation

```python
from tkinter import* # Import all classes from the tkinter module
import string
import secrets
import random  # Note: secrets is preferred for security, random is also present
import pyperclip

def generator():
    upper = list(string.ascii_uppercase)
    lower = list(string.ascii_lowercase)
    digits = list(string.digits)
    punctuation = list(string.punctuation)

    all = upper + lower + digits + punctuation
    password_len = int(length_Box.get())
    part1 = round(password_len * (30 / 100))  # Letters ~60% (in combination with digits)
    part2 = round(password_len * (20 / 100))  # Digits + Punc ~40% (in combination with letters)
    password = ""
    for i in range(part1):
        password += secrets.choice(upper)  # Use secrets for strong randomness
        password += secrets.choice(digits)

    for i in range(part2):
        password += secrets.choice(punctuation)
        password += secrets.choice(lower)

    passwordField.insert(0, password)

def copy():
    random_password = passwordField.get()
    pyperclip.copy(random_password)

# Build Window
root = Tk()
root.geometry("220x220")
mycolor = '#FFF4E6'
fontcolor = '#1D4C6B'
root.config(bg=mycolor)
choice = IntVar()
Font = ('arial', 13, 'bold')

# Create labels, buttons, entry fields, etc.
passwordlabel = Label(root, text='Password Generator', font=('Times', 18, 'bold'), bg=mycolor, fg=fontcolor)
passwordlabel.grid(pady=10)

lengthlabel = Label(root, text="Password Length", font=('cairo', 13, 'bold'), bg=mycolor, fg=fontcolor)
lengthlabel.grid()

length_Box = Spinbox(root, from_=8, to_=32, font=Font, width=5, wrap=True)
length_Box.grid()

generateButton = Button(root, text='Generate', font=(Font, 10, 'bold'), bg=mycolor, command=generator)
generateButton.grid(pady=5)

passwordField = Entry(root, width=20, bd=2, font=Font)
passwordField.grid()

copyButton = Button(root, text='Copy to Clipboard', font=(Font, 10, 'bold'), bg=mycolor, command=copy)
copyButton.grid(pady=5)

root.mainloop()
