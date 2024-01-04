# Text-Editor-Application
A Text Editor application is a software tool that allows users to create, edit, and manipulate plain text files. Unlike word processors, text editors are focused on handling raw text without formatting or rich media elements. Here's a detailed description of a typical Text Editor application:


import tkinter as tk
from tkinter.filedialog import askopenfilename, asksaveasfilename
import fitz
def open_file(window, text_edit):
    filepath = askopenfilename(filetypes=[("Text Files", "*.txt"), ("PDF Files", "*.pdf")])

    if not filepath:
        return

    if filepath.endswith('.pdf'):
        with fitz.open(filepath) as pdf_doc:
            content = ""
            for page_num in range(pdf_doc.page_count):
                page = pdf_doc[page_num]
                content += page.get_text()
            text_edit.delete(1.0, tk.END)
            text_edit.insert(tk.END, content)
    else:
        with open(filepath, "r") as f:
            content = f.read()
            text_edit.delete(1.0, tk.END)
            text_edit.insert(tk.END, content)

    window.title(f"Open file: {filepath}")

def save_file(window,text_edit):
    filepath = asksaveasfilename(filetypes=[("Text Files", "*.txt")])

    if not filepath:
        return

    with open(filepath, "w") as f:
        content =  text_edit.get(1.0, tk.END)
        f.write(content)
    window.title(f"save file: {filepath}")
def main():
    window = tk.Tk()
    window.title("Text Editor")
    window.rowconfigure(0, minsize=400)
    window.columnconfigure(1, minsize=500)

    text_edit = tk.Text(window, font="sans-serif 20")
    text_edit.grid(row=0, column=1)


    frame = tk.Frame(window, relief=tk.RAISED, bd=2)
    save_button = tk.Button(frame, text="Save",command=lambda: open_file(window,text_edit))
    open_button = tk.Button(frame, text="Open",command=lambda: open_file(window,text_edit))

    save_button.grid(row=0, column=0,padx = 5,pady =5,sticky = "ew")
    open_button.grid(row=1, column=0,padx = 5,pady =5,sticky = "ew")

    open_pdf_button = tk.Button(frame, text="Open PDF", command=lambda: open_file(window, text_edit))
    open_pdf_button.grid(row=2, column=0, padx=5, pady=5, sticky="ew")

    frame.grid(row=0, column=0, sticky ="ns")

    window.mainloop()


main()
