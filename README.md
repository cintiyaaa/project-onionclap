from tkinter import *
from tkinter import messagebox, ttk
from PIL import Image, ImageTk
import pandas as pd

class ONIONCLAP:
    def _init_(self, root):
        self.root = root
        self.root.title('Login')
        self.root.geometry('1920x1080')
        self.root.configure(bg='#fff')
        
        self.img = Image.open('images.png')
        self.img = self.img.resize((500,450))
        self.img = ImageTk.PhotoImage(self.img)
        self.frame()

    def frame(self):
        Label(self.root, image=self.img, bg='white').place(x=100, y=150)

        self.frame = Frame(self.root, width=350, height=350, bg='white')
        self.frame.place(x=650, y=200)

        heading = Label(self.frame, text='Login', fg='#57a1f8', bg='white', font=('Microsoft YaHei UI Light', 23, 'bold'))
        heading.place(x=120, y=4)

        title_label = Label(self.root, text='ONIONCLAP', fg='#57a1f8', bg='white', font=('Arial Black', 40, 'bold'))
        title_label.place(x=500, y=5 )

        self.create_username()
        self.create_password()

        Button(self.frame, width=39, pady=7, text='Login', bg='#57a1f8', fg='white', border=0, command=self.login).place(x=35, y=204)

    def create_username(self):
        self.user = Entry(self.frame, fg='black', border=0, bg='white', font=('Microsoft YaHei UI Light', 11))
        self.user.place(x=50, y=80)
        self.user.insert(0, 'Username: ')
        self.user.bind('<FocusIn>', self.on_enter_user) 
        self.user.bind('<FocusOut>', self.on_leave_user)
        Frame(self.frame, width=295, height=2, bg='black').place(x=25, y=107)

    def create_password(self):
        self.code = Entry(self.frame, fg='black', border=0, bg='white', font=('Microsoft YaHei UI Light', 11))
        self.code.place(x=50, y=150)
        self.code.insert(0, 'Password: ')
        self.code.bind('<FocusIn>', self.on_enter_password)
        self.code.bind('<FocusOut>', self.on_leave_password)
        Frame(self.frame, width=295, height=2, bg='black').place(x=25, y=177)

    def on_enter_user(self, e):
        self.user.delete(0, 'end')

    def on_leave_user(self, e):
        name = self.user.get()
        if name == '':
            self.user.insert(0, 'Username')

    def on_enter_password(self, e):
        self.code.delete(0, 'end')

    def on_leave_password(self, e):
        name = self.code.get()
        if name == '':
            self.code.insert(0, 'Password')

    def login(self):
        username = self.user.get()
        password = self.code.get()
        if username == 'onion' and password == '1234':
            self.open_window()
        else:
            messagebox.showerror('Password salah', 'Incorrect username or password')

    def open_window(self):
        new_window = Toplevel(self.root)
        new_window.title('HOME')
        new_window.geometry('1920x1080')

        frame = Frame(new_window)
        frame.config (bg = 'white')
        frame.pack(fill='both', expand=True)

        title_label = Label(frame, text='ONIONCLAP', fg='#70cb78', bg='white', font=('Arial Black', 40, 'bold'))
        title_label.pack(pady=30) 

        cara_penanaman = Button(frame, text="Cara Penanaman Bawang Bombay", bg='#70cb78', command=self.cara_menanam,  width = 30, height = 2, font =('Arial', 14))
        cara_penanaman.pack(side = 'left', pady = 20)
        cara_penanaman.place(x= 230, y = 180)

        prosedur_penanaman = Button(frame, text="Prosedur Penanaman", bg='#70cb78', command=self.prosedur_penanaman, width = 30, height = 2, font =('Arial', 14))
        prosedur_penanaman.pack(side='right', pady=15)
        prosedur_penanaman.place(x= 750, y = 180)

        informasi = Button(frame, text="Informasi Pupuk dan Nutrisi", bg='#70cb78', command=self.informasi,  width = 30, height = 2, font =('Arial', 14))
        informasi.pack(side='right', pady=20)
        informasi.place(x= 230, y = 280)

        pengendalian_hama = Button(frame, text="Pengendalian Hama", bg='#70cb78', command=self.pengendalian_hama,  width = 30, height = 2, font =('Arial', 14))
        pengendalian_hama.pack(side='right', pady=20)
        pengendalian_hama.place(x= 750, y = 280)

        progress = Button(frame, text="Progress Tanaman", bg='#70cb78', command=self.progress,  width = 30, height = 2, font =('Arial', 14))
        progress.pack(side='right', pady=20)
        progress.place(x= 480, y = 380)

        exit_button = Button(frame, text="Exit", bg='red', fg='white', command=self.exit_app,  width = 3, height = 1, font =('Arial', 12))
        exit_button.pack(side = 'bottom', pady=150)
    
    def cara_menanam(self):
        new_window = Toplevel(self.root)
        new_window.title('Cara Penanaman Bawang Bombay')
        new_window.geometry('1920x1080')

        notebook = ttk.Notebook(new_window)
        notebook.pack(fill='both', expand=True)

        penanaman = Frame(notebook)
        notebook.add(penanaman, text='File Content')

        try:
            with open('Informasi penanaman bawang bombay.txt', 'r') as file:
                content = file.read()
        except FileNotFoundError:
            content = "File not found."

        text_widget = Text(penanaman, wrap=WORD)
        text_widget.insert('1.0', content)
        text_widget.pack(fill='both', expand=True)
        text_widget.config(state=DISABLED)  

        exit_button = Button(penanaman, text="Exit", bg='red', fg='white', command=self.exit_app)
        exit_button.pack(pady=20)
    
    def prosedur_penanaman(self):
        try:
            df = pd.read_csv('penanaman_bombay.csv')
            content = df.to_string() 
        except FileNotFoundError:
            content = "File not found."
        self.show_csv_content(content)

    def show_csv_content(self, content):
        new_window = Toplevel(self.root)
        new_window.title('Prosedur Penanaman Bawang Bombay')
        new_window.geometry('1920x1080')

        notebook = ttk.Notebook(new_window)
        notebook.pack(fill='both', expand=True)

        prosedur = Frame(notebook)
        notebook.add(prosedur, text='Prosedur Penanaman Bawang Bombay')

        try:
            df = pd.read_csv('tabel_bombay.csv') 
        except FileNotFoundError:
            messagebox.showerror("Error", "File not found!")
            return

        style = ttk.Style()
        style.theme_use("clam")  
        style.configure("Treeview", rowheight=25)  
        style.configure("Treeview.Heading", font=('Arial', 9))  

        style.map("Treeview", background=[('selected', 'lightblue')], foreground=[('selected', 'black')])
        style.configure("Treeview", background="white", fieldbackground="white", borderwidth=1)
        style.layout('Treeview', [('Treeview.treearea', {'sticky': 'nswe'})])

        tree = ttk.Treeview(prosedur, columns=list(df.columns), show='headings', style="Treeview")
        tree.pack(fill='both', expand=True)

        scrollbar = ttk.Scrollbar(prosedur, orient="vertical", command=tree.yview)
        tree.configure(yscroll=scrollbar.set)
        scrollbar.pack(side="right", fill="y")

        scrollbar = ttk.Scrollbar(prosedur, orient="horizontal", command=tree.xview) 
        tree.configure(yscroll=scrollbar.set)
        scrollbar.pack(side="top", fill="x")

        for col in df.columns:
            max_width = max(df[col].astype(str).map(len).max(), len(col)) * 8
            tree.heading(col, text=col, anchor='center')  
            tree.column(col, width=max_width, anchor='center', stretch=False)  

        for i, row in df.iterrows():
            row_tags = 'evenrow' if i % 2 == 0 else 'oddrow'
            tree.insert("", "end", values=list(row), tags=(row_tags,))

        tree.tag_configure('evenrow', background='lightgrey')  
        tree.tag_configure('oddrow', background='white')       

        exit_button = Button(prosedur, text="Exit", bg='red', fg='white', command=self.exit_app)
        exit_button.pack(pady=20)

    def informasi(self):
        try:
            df = pd.read_csv('pupuk_nutrisi.csv')
            content = df.to_string() 
        except FileNotFoundError:
            content = "File not found."

        self.show_prosedur(content)

    def show_prosedur(self, content):
        new_window = Toplevel(self.root)
        new_window.title('Informasi Pupuk dan Nutrisi')
        new_window.geometry('1920x1080')

        notebook = ttk.Notebook(new_window)
        notebook.pack(fill='both', expand=True)

        info = Frame(notebook)
        notebook.add(info, text='Informasi Pupuk dan Nutrisi')

        try:
            df = pd.read_csv('pupuk_nutrisi.csv') 
        except FileNotFoundError:
            messagebox.showerror("Error", "File not found!")
            return

        style = ttk.Style()
        style.theme_use("clam") 
        style.configure("Treeview", rowheight=25) 
        style.configure("Treeview.Heading", font=('Arial', 9)) 

        style.map("Treeview", background=[('selected', 'lightblue')], foreground=[('selected', 'black')])
        style.configure("Treeview", background="white", fieldbackground="white", borderwidth=1)
        style.layout('Treeview', [('Treeview.treearea', {'sticky': 'nswe'})])

        tree = ttk.Treeview(info, columns=list(df.columns), show='headings', style="Treeview")
        tree.pack(fill='both', expand=True)

        scrollbar = ttk.Scrollbar(info, orient="horizontal", command=tree.xview) 
        tree.configure(xscroll=scrollbar.set)
        scrollbar.pack(side="top", fill="x")

        for col in df.columns:
            max_width = max(df[col].astype(str).map(len).max(), len(col)) * 8
            tree.heading(col, text=col, anchor='center') 
            tree.column(col, width=max_width, anchor='center', stretch=False) 

        for i, row in df.iterrows():
            row_tags = 'evenrow' if i % 2 == 0 else 'oddrow'
            tree.insert("", "end", values=list(row), tags=(row_tags,))

        tree.tag_configure('evenrow', background='lightgrey')  
        tree.tag_configure('oddrow', background='white')       

        exit_button = Button(info, text="Exit", bg='red', fg='white', command=self.exit_app)
        exit_button.pack(pady=20)
    
    def pengendalian_hama(self):
        new_window = Toplevel(self.root)
        new_window.title('Cara Mengendalikan Hama')
        new_window.geometry('1920x1080')

        frame = Frame(new_window)
        frame.config (bg = 'white')
        frame.pack(fill='both', expand=True)

        title_label = Label(frame, text='Hama & Penyakit', fg='#70cb78', bg='white', font=('Arial Black', 36, 'bold'))
        title_label.place(x=450, y=2 )

        hama1 = Button(frame, text="Ulat Bawang", bg='#70cb78', width = 20, height = 1, font =('Arial', 14))
        hama1.pack(side = 'left', pady = 15)
        hama1.place(x= 100, y = 180)

        hama2 = Button(frame, text="Kutu Daun Bawang", bg='#70cb78',width = 20, height = 1, font =('Arial', 14))
        hama2.pack(side='right', pady=15)
        hama2.place(x= 380, y = 180)

        hama3 = Button(frame, text="Ulat Grayak", bg='#70cb78', width = 20, height = 1, font =('Arial', 14))
        hama3.pack(side = 'left', pady = 15)
        hama3.place(x= 660, y = 180)

        hama4 = Button(frame, text="Liryomi", bg='#70cb78',width = 20, height = 1, font =('Arial', 14))
        hama4.pack(side='right', pady=15)
        hama4.place(x= 940, y = 180)

        hama5 = Button(frame, text="BELUM", bg='#70cb78', width = 20, height = 1, font =('Arial', 14))
        hama5.pack(side = 'left', pady = 15)
        hama5.place(x= 100, y = 350)

        hama6 = Button(frame, text="BELUM", bg='#70cb78',width = 20, height = 1, font =('Arial', 14))
        hama6.pack(side='right', pady=15)
        hama6.place(x= 380, y = 350)

        hama7 = Button(frame, text="BELUM", bg='#70cb78', width = 20, height = 1, font =('Arial', 14))
        hama7.pack(side = 'left', pady = 15)
        hama7.place(x= 660, y = 350)

        hama8 = Button(frame, text="BELUM", bg='#70cb78',width = 20, height = 1, font =('Arial', 14))
        hama8.pack(side='right', pady=15)
        hama8.place(x= 940, y = 350)

        hama9 = Button(frame, text="BELUM", bg='#70cb78', width = 20, height = 1, font =('Arial', 14))
        hama9.pack(side = 'left', pady = 15)
        hama9.place(x= 100, y = 520)

        hama10 = Button(frame, text="BELUM", bg='#70cb78',width = 20, height = 1, font =('Arial', 14))
        hama10.pack(side='right', pady=15)
        hama10.place(x= 380, y = 520)

        hama11 = Button(frame, text="BELUM", bg='#70cb78', width = 20, height = 1, font =('Arial', 14))
        hama11.pack(side = 'left', pady = 15)
        hama11.place(x= 660, y = 520)

        hama12 = Button(frame, text="BELUM", bg='#70cb78',width = 20, height = 1, font =('Arial', 14))
        hama12.pack(side='right', pady=15)
        hama12.place(x= 940, y = 520)

        exit_button = Button(frame, text="Exit", bg='red', fg='white', command=self.exit_app)
        exit_button.pack(side = 'bottom', pady=20)

    def progress(self):
        messagebox.showinfo("Button 4", "Button 4 clicked!")

    def exit_app(self):
        if messagebox.askyesno("Exit", "Are you sure you want to exit?"):
            self.root.quit()  

if _name_ == "_main_":
    root = Tk()
    app = ONIONCLAP(root)
    root.mainloop()
