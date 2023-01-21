# Student management system Project in Python with SQLite Database

**Step 1: Install Python** 

**Step 2: Install Pycharm | Create Project | Install Library**

**Step 3: Create Project** 

**Step 4: Create Python file**

**Step 5: Code Explanation**: To explain the code i have divided it different part and now i am going to explain each part one by one

**import library:** Here in this step we will import the needed library that will be used to create Student Management System.
```python
#import libraries
from tkinter import *
import tkinter.ttk as ttk
import tkinter.messagebox as tkMessageBox
import sqlite3
```
Here as you can see that there are three libraries are imported in which tkinter library is using to create a GUI window , tkinter.messagebox library is using to display message in Popup box and sqlite3 library is using to handle SQLite database.

**Create Database and Table:**  I have defined a function with name Database() to create database and table. Here is the following code to create database and table.

```python
#function to define database
def Database():
    global conn, cursor
    #creating student database
    conn = sqlite3.connect("student.db")
    cursor = conn.cursor()
    #creating STUD_REGISTRATION table
    cursor.execute(
        "CREATE TABLE IF NOT EXISTS STUD_REGISTRATION (STU_ID INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, STU_NAME TEXT, STU_CONTACT TEXT, STU_EMAIL TEXT, STU_ROLLNO TEXT, STU_BRANCH TEXT)")
```
Here in the above program database name is  student and table name is STUD_REGISTRATION. The fields of table STUD_REGISTRATION are STU_ID , STU_NAME, STU_CONTACT, STU_EMAIL, STU_ROLLNO AND STU_BRANCH

**Create GUI Window :**I have used four frame to create this Graphical user interface form in which first frame for Heading, second frame for Registration form, third frame for search window and the last frame for displaying students records. Here is the complete code to create GUI of this application.

```python
#defining function for creating GUI Layout
def DisplayForm():
    #creating window
    display_screen = Tk()
    #setting width and height for window
    display_screen.geometry("900x400")
    #setting title for window
    display_screen.title("SAMIR PAUL")
    #declaring variables
    global tree
    global SEARCH
    global name,contact,email,rollno,branch
    SEARCH = StringVar()
    name = StringVar()
    contact = StringVar()
    email = StringVar()
    rollno = StringVar()
    branch = StringVar()
    #creating frames for layout
    #topview frame for heading
    TopViewForm = Frame(display_screen, width=600, bd=1, relief=SOLID)
    TopViewForm.pack(side=TOP, fill=X)
    #first left frame for registration from
    LFrom = Frame(display_screen, width="350")
    LFrom.pack(side=LEFT, fill=Y)
    #seconf left frame for search form
    LeftViewForm = Frame(display_screen, width=500,bg="gray")
    LeftViewForm.pack(side=LEFT, fill=Y)
    #mid frame for displaying students record
    MidViewForm = Frame(display_screen, width=600)
    MidViewForm.pack(side=RIGHT)
    #label for heading
    lbl_text = Label(TopViewForm, text="Student Management System", font=('verdana', 18), width=600,bg="#1C2833",fg="white")
    lbl_text.pack(fill=X)
    #creating registration form in first left frame
    Label(LFrom, text="Name  ", font=("Arial", 12)).pack(side=TOP)
    Entry(LFrom,font=("Arial",10,"bold"),textvariable=name).pack(side=TOP, padx=10, fill=X)
    Label(LFrom, text="Contact ", font=("Arial", 12)).pack(side=TOP)
    Entry(LFrom, font=("Arial", 10, "bold"),textvariable=contact).pack(side=TOP, padx=10, fill=X)
    Label(LFrom, text="Email ", font=("Arial", 12)).pack(side=TOP)
    Entry(LFrom, font=("Arial", 10, "bold"),textvariable=email).pack(side=TOP, padx=10, fill=X)
    Label(LFrom, text="Rollno ", font=("Arial", 12)).pack(side=TOP)
    Entry(LFrom, font=("Arial", 10, "bold"),textvariable=rollno).pack(side=TOP, padx=10, fill=X)
    Label(LFrom, text="Branch ", font=("Arial", 12)).pack(side=TOP)
    Entry(LFrom, font=("Arial", 10, "bold"),textvariable=branch).pack(side=TOP, padx=10, fill=X)
    Button(LFrom,text="Submit",font=("Arial", 10, "bold"),command=register).pack(side=TOP, padx=10,pady=5, fill=X)

    #creating search label and entry in second frame
    lbl_txtsearch = Label(LeftViewForm, text="Enter name to Search", font=('verdana', 10),bg="gray")
    lbl_txtsearch.pack()
    #creating search entry
    search = Entry(LeftViewForm, textvariable=SEARCH, font=('verdana', 15), width=10)
    search.pack(side=TOP, padx=10, fill=X)
    #creating search button
    btn_search = Button(LeftViewForm, text="Search", command=SearchRecord)
    btn_search.pack(side=TOP, padx=10, pady=10, fill=X)
    #creating view button
    btn_view = Button(LeftViewForm, text="View All", command=DisplayData)
    btn_view.pack(side=TOP, padx=10, pady=10, fill=X)
    #creating reset button
    btn_reset = Button(LeftViewForm, text="Reset", command=Reset)
    btn_reset.pack(side=TOP, padx=10, pady=10, fill=X)
    #creating delete button
    btn_delete = Button(LeftViewForm, text="Delete", command=Delete)
    btn_delete.pack(side=TOP, padx=10, pady=10, fill=X)
   #setting scrollbar
    scrollbarx = Scrollbar(MidViewForm, orient=HORIZONTAL)
    scrollbary = Scrollbar(MidViewForm, orient=VERTICAL)
    tree = ttk.Treeview(MidViewForm,columns=("Student Id", "Name", "Contact", "Email","Rollno","Branch"),
                        selectmode="extended", height=100, yscrollcommand=scrollbary.set, xscrollcommand=scrollbarx.set)
    scrollbary.config(command=tree.yview)
    scrollbary.pack(side=RIGHT, fill=Y)
    scrollbarx.config(command=tree.xview)
    scrollbarx.pack(side=BOTTOM, fill=X)
    #setting headings for the columns
    tree.heading('Student Id', text="Student Id", anchor=W)
    tree.heading('Name', text="Name", anchor=W)
    tree.heading('Contact', text="Contact", anchor=W)
    tree.heading('Email', text="Email", anchor=W)
    tree.heading('Rollno', text="Rollno", anchor=W)
    tree.heading('Branch', text="Branch", anchor=W)
    #setting width of the columns
    tree.column('#0', stretch=NO, minwidth=0, width=0)
    tree.column('#1', stretch=NO, minwidth=0, width=100)
    tree.column('#2', stretch=NO, minwidth=0, width=150)
    tree.column('#3', stretch=NO, minwidth=0, width=80)
    tree.column('#4', stretch=NO, minwidth=0, width=120)
    tree.pack()
    DisplayData()
```
**Insert Data into table:** I have defined a function with name register(). First it will open database by calling Database() function. Next i have passed all the forms data into Python variable. After that i have applied a empty validation and finally at last i have applied query to insert data into table then displayed data screen by calling DisplayData() function. Here is the complete code to insert data into table.

```python
#function to insert data into database
def register():
    Database()
    #getting form data
    name1=name.get()
    con1=contact.get()
    email1=email.get()
    rol1=rollno.get()
    branch1=branch.get()
    #applying empty validation
    if name1=='' or con1==''or email1=='' or rol1==''or branch1=='':
        tkMessageBox.showinfo("Warning","fill the empty field!!!")
    else:
        #execute query
        conn.execute('INSERT INTO STUD_REGISTRATION (STU_NAME,STU_CONTACT,STU_EMAIL,STU_ROLLNO,STU_BRANCH) \
 VALUES (?,?,?,?,?)',(name1,con1,email1,rol1,branch1));
        conn.commit()
        tkMessageBox.showinfo("Message","Stored successfully")
        #refresh table data
        DisplayData()
        conn.close()
```
**Reset Form:** I have defined a function Reset() and it will reset all the from data.
```python
def Reset():
    #clear current data from table
    tree.delete(*tree.get_children())
    #refresh table data
    DisplayData()
    #clear search text
    SEARCH.set("")
    name.set("")
    contact.set("")
    email.set("")
    rollno.set("")
    branch.set("")
```
**Delete student record:** I have defined a function Delete() that will take the selected data and will delete it from database. Here is the complete code to delete selected data from database.
```python
def Delete():
    #open database
    Database()
    if not tree.selection():
        tkMessageBox.showwarning("Warning","Select data to delete")
    else:
        result = tkMessageBox.askquestion('Confirm', 'Are you sure you want to delete this record?',
                                          icon="warning")
        if result == 'yes':
            curItem = tree.focus()
            contents = (tree.item(curItem))
            selecteditem = contents['values']
            tree.delete(curItem)
            cursor=conn.execute("DELETE FROM STUD_REGISTRATION WHERE STU_ID = %d" % selecteditem[0])
            conn.commit()
            cursor.close()
            conn.close()
```
**Search student record:** I have defined a function SearchRecord() that will take the name of student and perform query to select the records of the given student and at last will populate it into the table format.
```python
#function to search data
def SearchRecord():
    #open database
    Database()
    #checking search text is empty or not
    if SEARCH.get() != "":
        #clearing current display data
        tree.delete(*tree.get_children())
        #select query with where clause
        cursor=conn.execute("SELECT * FROM STUD_REGISTRATION WHERE STU_NAME LIKE ?", ('%' + str(SEARCH.get()) + '%',))
        #fetch all matching records
        fetch = cursor.fetchall()
        #loop for displaying all records into GUI
        for data in fetch:
            tree.insert('', 'end', values=(data))
        cursor.close()
        conn.close()
```
**Display student record:** I have defined a function DisplayData() that will fetch all the records from the database and display into the GUI in table format. Here is the complete code
```python
#defining function to access data from SQLite database
def DisplayData():
    #open database
    Database()
    #clear current data
    tree.delete(*tree.get_children())
    #select query
    cursor=conn.execute("SELECT * FROM STUD_REGISTRATION")
    #fetch all data from database
    fetch = cursor.fetchall()
    #loop for displaying all data in GUI
    for data in fetch:
        tree.insert('', 'end', values=(data))
    cursor.close()
    conn.close()
```
**Step 6: Run Code**: To run this code just right click on the coding area and click on **Run Program** and you will get a output screen like below.
<img width="821" alt="home" src="https://user-images.githubusercontent.com/77569653/213883145-88fc35b6-7596-4d92-b914-8ccb982b89dc.png">

**Insert**:Here just fill the Student details and click on Submit button you will get a screen like below
<img width="821" alt="insert" src="https://user-images.githubusercontent.com/77569653/213883162-9a30a7da-c62f-4122-b4e4-49e37d773273.png">

**Search**: If you want to search record of any student then just fill his/her name in textbox and click on search button. For example here i am going to search student Samir's details
<img width="824" alt="search" src="https://user-images.githubusercontent.com/77569653/213883189-99f5c42a-46cf-4b61-bd54-3a3cb354e26d.png">

**Delete**:To delete record of any student just click on record to select and the click on delete button.
<img width="823" alt="delete" src="https://user-images.githubusercontent.com/77569653/213883206-89694686-fadd-434a-ad0d-dd75fe3bb2ce.png">

