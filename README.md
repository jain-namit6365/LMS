from tkinter import *
from tkinter.ttk import Treeview
import mysql.connector
import random
from tkinter.ttk import Combobox

connection = mysql.connector.connect(
    host='localhost',
    user='root',
    password='Namit2000#',
    db='library',
    auth_plugin='mysql_native_password'
)

# admin
#    id, name, sex, loginid, password, address, city, state, pincode, phonenumber, email

# librarian
#    id, name, sex, loginid, password, address, city, state, pincode, phonenumber, email

# members
#    id, name, sex, loginid, password, address, city, state, pincode, phonenumber, email, booksissued

# issuers
#    id, bookid, dateofissue, duedate, dateofreturn, fine

# library
#    bookid, bookname, author, pulisher, category(fiction, etc.), type(journal, newspapers, etc.), language, stock, publishdate, price, purchasedate, billno


# total no of books
# no of issued books
# no of journals etc.

window1 = Tk()
window1.geometry('400x400')
window1.title('Library Management')
r1 = IntVar()
r2 = IntVar()
r3 = IntVar()
r4 = IntVar()
true = 0
random1 = random.randint(1000, 10000)
random2 = random.randint(1000, 10000)


def login():
    s = r1.get()
    try:
        if random1 == int(txt3.get()):
            if s == 1:
                true = 1
                # try:
                #     cursor = connection.cursor()
                #     sql = 'select * from admin where loginid=%s and password=%s'
                #     try:
                #         cursor.execute(sql, (txt1.get(), txt2.get()))
                #         result = cursor.fetchall()
                #         if len(result) == 1:
                #             true = 1
                #         else:
                #             l9 = Label(window1, text='Wrong Loginid or Password')
                #             l9.place(x=20, y=350)
                #             l9.configure(height=1, width=50)
                #     except ConnectionError as e:
                #         print(e)
                # finally:
                #     print()

            elif s == 2:
                try:
                    cursor = connection.cursor()
                    sql = 'select * from librarian where loginid=%s and password=%s'
                    try:
                        cursor.execute(sql, (txt1.get(), txt2.get()))
                        result = cursor.fetchall()
                        if len(result) == 1:
                            true = 1
                        else:
                            l9 = Label(window1, text='Wrong Loginid or Password')
                            l9.place(x=20, y=350)
                            l9.configure(height=1, width=50)
                    except ConnectionError as e:
                        print(e)
                finally:
                    print()

            elif s == 3:
                try:
                    cursor = connection.cursor()
                    sql = 'select * from member where loginid=%s and password=%s'
                    try:
                        cursor.execute(sql, (txt1.get(), txt2.get()))
                        result = cursor.fetchall()
                        if len(result) == 1:
                            true = 1
                        else:
                            l9 = Label(window1, text='Wrong Loginid or Password')
                            l9.place(x=20, y=350)
                            l9.configure(height=1, width=50)

                    except ConnectionError as e:
                        print(e)
                finally:
                    print()

            else:
                l5 = Label(window1, text='Please select a Category')
                l5.place(x=20, y=350)
                l5.configure(height=1, width=50)

        else:
            # random3 = random.randint(1000, 10000)
            # l12 = Label(window1, text=str(random3), fg='red', bg='yellow')
            # l12.place(x=280, y=240)
            # l12.config(height=1, width=10)
            l20 = Label(window1, text='Wrong Captcha')
            l20.place(x=20, y=350)
            l20.configure(height=1, width=50)
    except ValueError as v:
        l8 = Label(window1, text='Enter number in Captcha')
        print(v)
        l8.place(x=20, y=350)
        l8.configure(height=1, width=50)
    finally:
        if true == 1:
            window3 = Toplevel(window1)
            # window3.geometry('400x300')
            window3.state('zoomed')
            # scrollbar1 = Scrollbar(window3)
            # scrollbar1.config(command=window3.yview)
            # scrollbar1.grid(column=1, row=0, sticky='NS')
            # scrollbar1.pack(side=RIGHT, fill=Y)
            # mylist = Listbox(window3, yscrollcommand=scrollbar1.set)
            # scrollbar1.config(command=mylist.yview)
            # scrollbar2 = Scrollbar(window3)
            # scrollbar2.pack(side=BOTTOM, fill=X)
            # window3.attributes('-fullscreen', True)
            # l8 = Label(window3, text='Welcome Member')
            # l8.place(x=50, y=100)
            tree1 = Treeview(window3)

            def entry1():
                try:
                    cursor1 = connection.cursor()
                    sql1 = "select bookid,bookname,author,publisher,category,type,language,publishdate,price from books"
                    try:
                        cursor1.execute(sql1)
                        result1 = cursor1.fetchall()
                        for r in result1:
                            tree1.insert("", "end", text=r[0], values=(r[1], r[2], r[3], r[4], r[5], r[6], r[7], r[8]))

                    except ConnectionError as e:
                        print(e)
                    connection.commit()
                finally:
                    print()

            tree1["columns"] = ("1", "2", "3", "4", "5", "6", "7", "8")
            tree1.column("#0", width=120, minwidth=50, stretch=NO)
            tree1.column("1", width=220, minwidth=100, stretch=NO)
            tree1.column("2", width=170, minwidth=75, stretch=NO)
            tree1.column("3", width=170, minwidth=75, stretch=NO)
            tree1.column("4", width=170, minwidth=75, stretch=NO)
            tree1.column("5", width=170, minwidth=75, stretch=NO)
            tree1.column("6", width=170, minwidth=75, stretch=NO)
            tree1.column("7", width=170, minwidth=75, stretch=NO)
            tree1.column("8", width=120, minwidth=50, stretch=NO)
            tree1.heading("#0", text='Id', anchor=W)
            tree1.heading("1", text='Book Name', anchor=W)
            tree1.heading("2", text='Author', anchor=W)
            tree1.heading("3", text='Publisher', anchor=W)
            tree1.heading("4", text='Category', anchor=W)
            tree1.heading("5", text='Type', anchor=W)
            tree1.heading("6", text='Language', anchor=W)
            tree1.heading("7", text='Publish Date', anchor=W)
            tree1.heading("8", text='Price', anchor=W)
            tree1.place(x=20, y=180)
            entry1()

            def view():
                window4 = Toplevel(window3)
                tree3 = Treeview(window4)
                tree2 = Treeview(window4)
                window4.state('zoomed')

                def entry2():
                    try:
                        cursor1 = connection.cursor()
                        if r3.get() == 1:
                            sql1 = "select * from admin"
                        else:
                            sql1 = "select * from librarian"
                        try:
                            cursor1.execute(sql1)
                            result1 = cursor1.fetchall()
                            for r in result1:
                                tree2.insert("", "end", text=r[0], values=(r[1], r[2], r[3], r[4], r[5], r[6], r[7], r[8], r[9], r[10]))

                        except ConnectionError as e:
                            print(e)
                        connection.commit()
                    finally:
                        print()


                def entry3():
                    try:
                        cursor1 = connection.cursor()
                        if r3.get() == 4:
                            sql1 = "select * from books"
                        else:
                            sql1 = "select * from member"
                        try:
                            cursor1.execute(sql1)
                            result1 = cursor1.fetchall()
                            for r in result1:
                                tree3.insert("", "end", text=r[0], values=(r[1], r[2], r[3], r[4], r[5], r[6], r[7], r[8], r[9], r[10], r[11]))

                        except ConnectionError as e:
                            print(e)
                        connection.commit()
                    finally:
                        print()

                if int(r3.get()) == 1 or int(r3.get()) == 2:
                    tree2["columns"] = ("1", "2", "3", "4", "5", "6", "7", "8", "9", "10")
                    tree2.column("#0", width=50, minwidth=50, stretch=NO)
                    tree2.column("1", width=150, minwidth=100, stretch=NO)
                    tree2.column("2", width=50, minwidth=75, stretch=NO)
                    tree2.column("3", width=200, minwidth=75, stretch=NO)
                    tree2.column("4", width=150, minwidth=75, stretch=NO)
                    tree2.column("5", width=200, minwidth=75, stretch=NO)
                    tree2.column("6", width=100, minwidth=75, stretch=NO)
                    tree2.column("7", width=150, minwidth=50, stretch=NO)
                    tree2.column("8", width=100, minwidth=75, stretch=NO)
                    tree2.column("9", width=150, minwidth=50, stretch=NO)
                    tree2.column("10", width=200, minwidth=75, stretch=NO)
                    tree2.heading("#0", text='Id', anchor=W)
                    tree2.heading("1", text='Name', anchor=W)
                    tree2.heading("2", text='Sex', anchor=W)
                    tree2.heading("3", text='Login Id', anchor=W)
                    tree2.heading("4", text='Password', anchor=W)
                    tree2.heading("5", text='Address', anchor=W)
                    tree2.heading("6", text='City', anchor=W)
                    tree2.heading("7", text='State', anchor=W)
                    tree2.heading("8", text='Pincode', anchor=W)
                    tree2.heading("9", text='Phone Number', anchor=W)
                    tree2.heading("10", text='Email', anchor=W)
                    tree2.place(x=20, y=100)
                    entry2()

                elif int(r3.get()) == 3:
                    tree3["columns"] = ("1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11")
                    tree3.column("#0", width=50, minwidth=50, stretch=NO)
                    tree3.column("1", width=150, minwidth=100, stretch=NO)
                    tree3.column("2", width=150, minwidth=75, stretch=NO)
                    tree3.column("3", width=150, minwidth=75, stretch=NO)
                    tree3.column("4", width=150, minwidth=75, stretch=NO)
                    tree3.column("5", width=150, minwidth=75, stretch=NO)
                    tree3.column("6", width=150, minwidth=75, stretch=NO)
                    tree3.column("7", width=100, minwidth=50, stretch=NO)
                    tree3.column("8", width=120, minwidth=75, stretch=NO)
                    tree3.column("9", width=100, minwidth=50, stretch=NO)
                    tree3.column("10", width=120, minwidth=75, stretch=NO)
                    tree3.column("11", width=100, minwidth=75, stretch=NO)
                    tree3.heading("#0", text='Id', anchor=W)
                    tree3.heading("1", text='Name', anchor=W)
                    tree3.heading("2", text='Sex', anchor=W)
                    tree3.heading("3", text='Login Id', anchor=W)
                    tree3.heading("4", text='Password', anchor=W)
                    tree3.heading("5", text='Address', anchor=W)
                    tree3.heading("6", text='City', anchor=W)
                    tree3.heading("7", text='State', anchor=W)
                    tree3.heading("8", text='Pincode', anchor=W)
                    tree3.heading("9", text='Phone Number', anchor=W)
                    tree3.heading("10", text='Email', anchor=W)
                    tree3.heading("11", text='Books Issued', anchor=W)
                    tree3.place(x=20, y=100)
                    entry3()

                else:
                    tree3["columns"] = ("1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11")
                    tree3.column("#0", width=50, minwidth=50, stretch=NO)
                    tree3.column("1", width=150, minwidth=100, stretch=NO)
                    tree3.column("2", width=150, minwidth=75, stretch=NO)
                    tree3.column("3", width=150, minwidth=75, stretch=NO)
                    tree3.column("4", width=150, minwidth=75, stretch=NO)
                    tree3.column("5", width=150, minwidth=75, stretch=NO)
                    tree3.column("6", width=150, minwidth=75, stretch=NO)
                    tree3.column("7", width=100, minwidth=50, stretch=NO)
                    tree3.column("8", width=120, minwidth=75, stretch=NO)
                    tree3.column("9", width=100, minwidth=50, stretch=NO)
                    tree3.column("10", width=120, minwidth=75, stretch=NO)
                    tree3.column("11", width=100, minwidth=75, stretch=NO)
                    tree3.heading("#0", text='Id', anchor=W)
                    tree3.heading("1", text='Book Name', anchor=W)
                    tree3.heading("2", text='Author', anchor=W)
                    tree3.heading("3", text='Publisher', anchor=W)
                    tree3.heading("4", text='Category', anchor=W)
                    tree3.heading("5", text='Type', anchor=W)
                    tree3.heading("6", text='Language', anchor=W)
                    tree3.heading("7", text='Stock', anchor=W)
                    tree3.heading("8", text='Publish Date', anchor=W)
                    tree3.heading("9", text='Price', anchor=W)
                    tree3.heading("10", text='Purchase Date', anchor=W)
                    tree3.heading("11", text='Bill', anchor=W)
                    tree3.place(x=20, y=100)
                    entry3()

            def register():
                window2 = Toplevel(window1)
                window2.geometry('450x500')
                window2.title('Add')
                s2 = r3.get()

                def add():
                    if int(txt14.get()) == random2:
                        if r2.get() == 1:
                            sex = 'Male'
                        elif r2.get() == 2:
                            sex = 'Female'
                        else:
                            sex = 'Other'
                        try:
                            cursor = connection.cursor()
                            if s2 == 1:
                                sql = "insert into admin(name,sex,loginid,password,address,city,state,pincode,phonenumber,email) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)"
                            elif s2 == 2:
                                sql = "insert into librarian(name,sex,loginid,password,address,city,state,pincode,phonenumber,email) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)"
                            else:
                                sql = "insert into member(name,sex,loginid,password,address,city,state,pincode,phonenumber,email) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)"
                            try:
                                cursor.execute(sql, (txt4.get(), sex, txt6.get(), txt7.get(), txt8.get(), txt9.get(), txt10.get(), int(txt11.get()), int(txt12.get()), txt13.get()))
                                l22 = Label(window2, text='Added Successfully')
                                l22.place(x=150, y=450)
                                l22.config(width=20)
                            except ConnectionError as e:
                                print(e)
                            connection.commit()
                        finally:
                            txt4.delete(0, 'end')
                            txt6.delete(0, 'end')
                            txt7.delete(0, 'end')
                            txt8.delete(0, 'end')
                            txt9.delete(0, 'end')
                            txt10.delete(0, 'end')
                            txt11.delete(0, 'end')
                            txt12.delete(0, 'end')
                            txt13.delete(0, 'end')
                            txt14.delete(0, 'end')
                    else:
                        l23 = Label(window2, text='Wrong Captcha')
                        l23.place(x=150, y=450)
                        l23.config(width=20)


                def addbook():
                    # random3 = random.randint(1000, 10000)
                    if int(txt15.get()) == random2:
                        try:
                            cursor = connection.cursor()
                            sql = "insert into books(bookname,author,publisher,category,type,language,stock,publishdate,price,purchasedate,billno) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)"
                            try:
                                cursor.execute(sql, (txt4.get(), txt5.get(), txt6.get(), txt7.get(), txt8.get(), txt9.get(), int(txt10.get()), txt11.get(), int(txt12.get()), txt13.get(), txt14.get()))
                                l22 = Label(window2, text='Added Successfully')
                                l22.place(x=150, y=480)
                                l22.config(width=20)

                            except ConnectionError as e:
                                print(e)
                            connection.commit()
                        finally:
                            tree1.delete(*tree1.get_children())
                            entry1()
                            txt4.delete(0, 'end')
                            txt5.delete(0, 'end')
                            txt6.delete(0, 'end')
                            txt7.delete(0, 'end')
                            txt8.delete(0, 'end')
                            txt9.delete(0, 'end')
                            txt10.delete(0, 'end')
                            txt11.delete(0, 'end')
                            txt12.delete(0, 'end')
                            txt13.delete(0, 'end')
                            txt14.delete(0, 'end')
                            txt15.delete(0, 'end')
                    else:
                        l23 = Label(window2, text='Wrong Captcha')
                        l23.place(x=150, y=480)
                        l23.config(width=20)

                l18 = Label(window2, text='Enter Details', font=('bold', 16), fg='red')
                l18.place(x=160, y=20)
                if s2 != 4:
                    l8 = Label(window2, text='Name         :', font=('Consolas', 12))
                    l9 = Label(window2, text='Sex          :', font=('Consolas', 12))
                    l10 = Label(window2, text='Login Id     :', font=('Consolas', 12))
                    l11 = Label(window2, text='Password     :', font=('Consolas', 12))
                    l12 = Label(window2, text='Address      :', font=('Consolas', 12))
                    l13 = Label(window2, text='City         :', font=('Consolas', 12))
                    l14 = Label(window2, text='State        :', font=('Consolas', 12))
                    l15 = Label(window2, text='Pincode      :', font=('Consolas', 12))
                    l16 = Label(window2, text='Phone Number :', font=('Consolas', 12))
                    l17 = Label(window2, text='E-Mail       :', font=('Consolas', 12))
                    l19 = Label(window2, text='Captcha      :', font=('Consolas', 12))
                    l20 = Label(window2, text=str(random2), fg='red', bg='yellow')
                    rad4 = Radiobutton(window2, text='Male', value=1, variable=r2)
                    rad5 = Radiobutton(window2, text='Female', value=2, variable=r2)
                    rad6 = Radiobutton(window2, text='Other', value=3, variable=r2)
                    l8.place(x=20, y=80)
                    l9.place(x=20, y=110)
                    l10.place(x=20, y=140)
                    l11.place(x=20, y=170)
                    l12.place(x=20, y=200)
                    l13.place(x=20, y=230)
                    l14.place(x=20, y=260)
                    l15.place(x=20, y=290)
                    l16.place(x=20, y=320)
                    l17.place(x=20, y=350)
                    l19.place(x=20, y=380)
                    l20.place(x=320, y=380)
                    l20.config(height=1, width=10)
                    rad4.place(x=160, y=110)
                    rad5.place(x=235, y=110)
                    rad6.place(x=320, y=110)
                    rad4.select()
                    txt4 = Entry(window2, width=40)
                    txt6 = Entry(window2, width=40)
                    txt7 = Entry(window2, width=40, show="*")
                    txt8 = Entry(window2, width=40)
                    txt9 = Entry(window2, width=40)
                    txt10 = Entry(window2, width=40)
                    txt11 = Entry(window2, width=40)
                    txt12 = Entry(window2, width=40)
                    txt13 = Entry(window2, width=40)
                    txt14 = Entry(window2, width=20)
                    txt4.place(x=160, y=80)
                    txt6.place(x=160, y=140)
                    txt7.place(x=160, y=170)
                    txt8.place(x=160, y=200)
                    txt9.place(x=160, y=230)
                    txt10.place(x=160, y=260)
                    txt11.place(x=160, y=290)
                    txt12.place(x=160, y=320)
                    txt13.place(x=160, y=350)
                    txt14.place(x=160, y=380)
                    bt2 = Button(window2, text='SUBMIT', command=add)
                    bt2.place(x=180, y=420)
                    bt2.config(height=2, width=10)
                else:
                    l8 = Label(window2, text='Book Name     :', font=('Consolas', 12))
                    l9 = Label(window2, text='Author        :', font=('Consolas', 12))
                    l10 = Label(window2, text='Publisher     :', font=('Consolas', 12))
                    l11 = Label(window2, text='Category      :', font=('Consolas', 12))
                    l12 = Label(window2, text='Type          :', font=('Consolas', 12))
                    l13 = Label(window2, text='Language      :', font=('Consolas', 12))
                    l14 = Label(window2, text='Stock         :', font=('Consolas', 12))
                    l15 = Label(window2, text='Publish Date  :', font=('Consolas', 12))
                    l16 = Label(window2, text='Price         :', font=('Consolas', 12))
                    l17 = Label(window2, text='Purchase Date :', font=('Consolas', 12))
                    l19 = Label(window2, text='Bill Number   :', font=('Consolas', 12))
                    l20 = Label(window2, text=str(random2), fg='red', bg='yellow')
                    l21 = Label(window2, text='Captcha       :', font=('Consolas', 12))
                    l8.place(x=20, y=80)
                    l9.place(x=20, y=110)
                    l10.place(x=20, y=140)
                    l11.place(x=20, y=170)
                    l12.place(x=20, y=200)
                    l13.place(x=20, y=230)
                    l14.place(x=20, y=260)
                    l15.place(x=20, y=290)
                    l16.place(x=20, y=320)
                    l17.place(x=20, y=350)
                    l19.place(x=20, y=380)
                    l20.place(x=320, y=410)
                    l20.config(height=1, width=10)
                    l21.place(x=20, y=410)
                    txt4 = Entry(window2, width=40)
                    txt5 = Entry(window2, width=40)
                    txt6 = Entry(window2, width=40)
                    txt7 = Entry(window2, width=40)
                    txt8 = Entry(window2, width=40)
                    txt9 = Entry(window2, width=40)
                    txt10 = Entry(window2, width=40)
                    txt11 = Entry(window2, width=40)
                    txt11.insert(END, 'yyyy-mm-dd')
                    txt12 = Entry(window2, width=40)
                    txt13 = Entry(window2, width=40)
                    txt13.insert(END, 'yyyy-mm-dd')
                    txt14 = Entry(window2, width=40)
                    txt15 = Entry(window2, width=20)
                    txt4.place(x=160, y=80)
                    txt5.place(x=160, y=110)
                    txt6.place(x=160, y=140)
                    txt7.place(x=160, y=170)
                    txt8.place(x=160, y=200)
                    txt9.place(x=160, y=230)
                    txt10.place(x=160, y=260)
                    txt11.place(x=160, y=290)
                    txt12.place(x=160, y=320)
                    txt13.place(x=160, y=350)
                    txt14.place(x=160, y=380)
                    txt15.place(x=160, y=410)
                    bt2 = Button(window2, text='SUBMIT', command=addbook)
                    bt2.place(x=180, y=440)
                    bt2.config(height=2, width=10)

            rad4 = Radiobutton(window3, text='Admin', value=1, variable=r3)
            rad5 = Radiobutton(window3, text='Librarian', value=2, variable=r3)
            rad6 = Radiobutton(window3, text='Member', value=3, variable=r3)
            rad7 = Radiobutton(window3, text='Book', value=4, variable=r3)
            rad7.select()
            bt2 = Button(window3, text='ADD', command=register)
            # rad8.configure(state=DISABLED)
            bt3 = Button(window3, text='VIEW', command=view)
            rad4.place(x=500, y=30)
            rad5.place(x=570, y=30)
            rad6.place(x=650, y=30)
            rad7.place(x=730, y=30)
            bt2.place(x=550, y=60)
            bt2.configure(width=10, height=2)
            bt3.place(x=650, y=60)
            bt3.configure(width=10, height=2)
            l10 = Label(window3, text='Search : ')
            txt4 = Entry(window3, width=50)
            # txt4.insert(END, 'SEARCH BOX')
            l11 = Label(window3, text='in')
            combo1 = Combobox(window3)
            combo1['values'] = ("---Select---", "Id", "Book Name", "Author", "Publisher", "Category", "Type")
            combo1.current(0)
            bt4 = Button(window3, text='SEARCH')
            bt5 = Button(window3, text='SHOW ALL')
            l10.place(x=20, y=150)
            txt4.place(x=70, y=150)
            l11.place(x=380, y=150)
            combo1.place(x=410, y=150)
            bt4.place(x=570, y=140)
            bt4.configure(width=10, height=2)
            bt5.place(x=670, y=140)
            bt5.configure(width=10, height=2)
            bt6 = Button(window3, text='ISSUE BOOK')
            bt6.place(x=20, y=420)
            bt6.configure(width=105, height=2)
            bt7 = Button(window3, text='UPDATE')
            bt7.place(x=767, y=420)
            bt7.configure(width=51, height=2)
            bt8 = Button(window3, text='DELETE')
            bt8.place(x=1135, y=420)
            bt8.configure(width=51, height=2)
            if s == 2:
                rad4.configure(state=DISABLED)
                rad5.configure(state=DISABLED)
            elif s == 3:
                rad4.configure(state=DISABLED)
                rad5.configure(state=DISABLED)
                rad6.configure(state=DISABLED)
                rad7.configure(state=DISABLED)
                bt2.configure(state=DISABLED)
                bt3.configure(state=DISABLED)
                bt6.configure(state=DISABLED)
                bt7.configure(state=DISABLED)
                bt8.configure(state=DISABLED)


l1 = Label(window1, text='LIBRARY MANAGEMENT', font=24, fg="red", justify=CENTER)
l1.place(x=80, y=50)
rad1 = Radiobutton(window1, text='Administrator', value=1, variable=r1)
rad1.select()
rad2 = Radiobutton(window1, text='Librarian', value=2, variable=r1)
rad3 = Radiobutton(window1, text='Member', value=3, variable=r1)
rad1.place(x=60, y=120)
rad2.place(x=170, y=120)
rad3.place(x=260, y=120)
# l2 = Label(window1, text='Enter Login Details', font=12, fg="blue")
# l2.place(x=100, y=40)
l3 = Label(window1, text='Login Id   : ')
l4 = Label(window1, text='Password : ')
l6 = Label(window1, text='Captcha   : ')
l7 = Label(window1, text=str(random1), fg='red', bg='yellow')
l7.configure(height=1, width=10)
txt1 = Entry(window1, width=40)
txt2 = Entry(window1, width=40, show="*")
txt3 = Entry(window1, width=20)
l3.place(x=40, y=160)
l4.place(x=40, y=200)
l6.place(x=40, y=240)
l7.place(x=280, y=240)
txt1.place(x=110, y=160)
txt2.place(x=110, y=200)
txt3.place(x=110, y=240)
bt1 = Button(window1, text='Login', command=login)
bt1.place(x=140, y=300)
bt1.config(height=2, width=15)
# l5 = Label(window1, text='-------------------- OR --------------------')
# l5.place(x=84, y=380)
# bt2 = Button(window1, text='Register', command=register)
# bt2.place(x=140, y=440)
# bt2.config(height=2, width=15)

window1.mainloop()
