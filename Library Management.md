## Library Management
import matplotlib.pyplot as plt
from tkinter import *
from datetime import *
import mysql.connector as sqp
import numpy as np

dbs=sqp.connect(host="localhost",user="root",password="shreya12345678")
cur=dbs.cursor()
cur.execute("create database if not exists public")
cur.execute("use public")

##CREATING DATABASE AND REQUIRED TABLES

cur.execute("create table if not exists library(Bid int,B_name char(50),Genere char(50) ,B_price float ,Qty int)")
cur.execute("create table if not exists info(Bid int ,Author_name char(50),Demand_of_Book numeric(5,2) "\
", Date_of_issue date)")
cur.execute("create table if not exists persons(P_name char(50),Ph_Number varchar(10),Address varchar(50))")
print()
print("\t\t\t\t-------------------------\t\t\t")
print("\t\t\t\tPUBLIC LIBRARY MANAGEMENT\t\t\t")
print("\t\t\t\t--------------------------\t\t\t")
print()
while 1:
    print("\t=-=-=-=-=-=-=-=-=-=-==-=-=-=-==-=-=-==-==-=-=-==-=-=-=-==-=-==-=\t")
    print()
    print("\tMenu:\n\n\t1.For_Issuing_A_Book\n\t2.Price_structure_of_Books\n\t"\
        "3.Update_The_Details_of_Books\n\t4.For_Removing_Books\n\t"\
        "5.For_Adding_Books\n\t6.Favourable_Books\n\t"\
        "7.Various_Generes_of_Books\n\t8.Information_Of_Books\n\t"\
        "9.Information_a_specific_Book\n\t10.Exit\n\t")
    print("\t=-=-=-=-=-=-=-=-=-=-==-=-=-=-==-=-=-==-==-=-=-==-=-=-=-==-=-==-=\t")
    x=int(input("\nSelect your choice:-"))

##FOR ISSUING A BOOK
    
    if x==1:
        print()
        print("\t=-=-=-=-=-=-=-=-=-=-=-=-=-=-=\t")
        print("\tWelcome to the Public library\t")
        print("\t=-=-=-=-=-=-=-=-=-=-=-=-=-=-=\t")
        print()
        print(">>Press 1 to issue the Book")
        print("")
        print(">>Press 2 to Back to the menu")
        print("")
        ch=int(input("Enter your choice:"))
        print()
        if ch == 1:
            print("We have these Books in our library:-")
            print()
            a="select B_name from library;"
            cur.execute(a)
            t=cur.fetchall()
            p=1
            for x in t:
                print(p,".",x[0])
                p+=1
            print()
            q=input("Which Book you want to issue?")
            print()
            print("Enter Your relevant Details:")
            print()
            n=input("What's your name?")
            d=input("What's your phone number?")
            a=input("Please Enter your address also!!!!")
            o="insert into persons(P_name,Ph_Number,Address ) values ('{}','{}','{}');".format(n,d,a)
            cur.execute(o)
            dbs.commit()
            m="select * from persons;"
            cur.execute(m)
            rc=cur.fetchall()
            print()
            print("-----------------------------------------")
            for x in rc:
                print()
                print("\tYour Details are:\t")
                print("\tName:",x[0])
                print("\tPhone Number:",x[1])
                print("\tAddress:",x[2])
                print("\tIssued Book is:",q)
                print()
                print("\t=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=\t")
                print("\tBook Must Be Returned within 30 Days!!!!\t")
                print("\t=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=\t")
                print()
                print("\tThank you for Issuing Book\t")
                print()
                print("-----------------------------------------")
            o="delete from persons;"
            cur.execute(o)
            dbs.commit()
            t="select Qty from library where B_name='%s';"%(q)
            cur.execute(t)
            print()
            for x in cur:
                print("Availaible Quantity of Book",q,"is",x[0])
                print("Now,Left Quantity is:",x[0]-1)
            print()
        elif ch==2:
            continue
        else:
            print("Thank you for coming")

##PRICE STRUCTURE OF BOOKS
            
    elif x==2:
        print()
        print("\t---------------------------\t")
        print("\tThis is Our Price Structure\t")
        print("\t---------------------------\t")
        print()
        que=int(input("1.In_The_Range_of_your_choice\n"\
            "2.Expensive_Than_your_choice\n"\
            "3.Cheaper_Than_your_choice"\
            "\n\nSelect your choice:-"))
        if que==1:
            print()
            h=int(input("Enter The Higher price:-"))
            l=int(input("Enter The lower price:-"))
            w="select B_name,B_price from library where B_price>={} and B_price<={}".format(l,h)
            cur.execute(w)
            rec=cur.fetchall()
            print()
            count=cur.rowcount
            if count ==0:
                 print("Books are unavailable in this range..!")
                 print()
            else:
                print("\tBooks Availaible are..\t")
                print()
                for x in rec:
                    print("Book Name:",x[0])
                    print("Price:",x[1])
                    print()
            print()
        elif que==2:
            print()
            h=int(input("Enter The price:-"))
            w="select B_name,B_price from library where B_price>{}".format(h)
            cur.execute(w)
            rec=cur.fetchall()
            print()
            count=cur.rowcount
            if count ==0:
                 print("Books are unavailable of your choice..!")
                 print()
            else:
                print("\tBooks Availaible are..\t")
                print()
                for x in rec:
                    print("Book Name:",x[0])
                    print("Price:",x[1])
                    print()
            print()
            print()
        elif que==3:
            print()
            h=int(input("Enter The price:-"))
            w="select B_name,B_price from library where B_price<{}".format(h)
            cur.execute(w)
            rec=cur.fetchall()
            print()
            count=cur.rowcount
            if count ==0:
                 print("Books are unavailable of your choice..!")
                 print()
            else:
                print("\tBooks Availaible are..\t")
                print()
                for x in rec:
                    print("Book Name:",x[0])
                    print("Price:",x[1])
                    print()
            print()
            print()
        else:
            print("Invalid Choice!!")
            print()

##UPDATE THE INFORMATION OF BOOKS

    elif x==3:
        print()
        print(">>Press 1 to continue..")
        print("")
        print(">>Press 2 to Back to the Menu..")
        print("")
        ch=int(input("Enter your choice:"))
        print()
        if ch==1:
            print()
            print(">>Press 1 to Update Price of books")
            print("")
            print(">>Press 2 to Update Quantity of books")
            print("")
            print(">>Press 3 to Update  Author name")
            print("")
            print(">>Press 4 to Update Demand of Book")
            print("")
            que=int(input("Enter your choice:"))
            if que==1:
                print()
                s=float(input("New Book Price:"))
                t=float(input("Give existing Book Price:"))
                cur.execute("update library set B_price={} where B_price={}".format(s,t))
                dbs.commit()
                cur.execute("select bid,B_name,B_price from library")
                print()
                print(">>>>>Recordes updated<<<<<")
                print()
                for x in cur :
                    print("Book ID:",x[0])
                    print("Book Name:",x[1])
                    print("Price:",x[2])
                    print()
                print()
            elif que==2:
                print()
                s=int(input("New Book Quantity:"))
                t=int(input("Give existing Book Quantity:"))
                cur.execute("update library set Qty={} where Qty={}".format(s,t))
                dbs.commit()
                cur.execute("select bid,B_name,Qty from library")
                print()
                print(">>>>>Recordes updated<<<<<")
                print()
                for x in cur :
                    print("Book ID:",x[0])
                    print("Book Name:",x[1])
                    print("Quantity:",x[2])
                    print()
                print()
            elif que==3:
                print()
                p=input("New Author name issue:")
                k=input("Give existing Author name issue:")
                cur.execute("update info set Author_name='{}'where Author_name='{}'".format(p,k))
                dbs.commit()
                cur.execute("select bid,Author_name from info")
                print()
                print(">>>>>Recordes updated<<<<<")
                print()
                for x in cur:
                    print("Book Id:",x[0])
                    print("Author's Name",x[1])
                    print()
            elif que==4:
                print()
                p=input("New Demand of Book ")
                k=input("Give existing  Demand of Book:")
                cur.execute("update info set Demand_of_book='{}'where Demand_of_book='{}'".format(p,k))
                dbs.commit()
                cur.execute("select bid,Demand_of_book from info")
                print()
                print(">>>>>Recordes updated<<<<<")
                print()
                for x in cur :
                    print("Book Id:",x[0])
                    print("Demand of Book:",x[1])
                    print()
                    print()
            elif ch==2:
                continue
            else:
                print("Invalid choice")
                print()

##FOR REMOVING BOOKS
                     
    elif x==4:
        print()
        que=input("Which Genere you want to Remove?")
        n=que
        cur.execute("Delete from library where Genere='%s';"%(n))
        dbs.commit()
        cur.execute("select B_name,Genere from library;")
        re=cur.fetchall()
        print("\t_________________________\t")
        print("\tAvailable Books are:\t")
        print("\t_________________________\t")
        for x in re:
            print()
            print("Book name:",x[0])
            print("Genere:",x[1])
            print()
        print()
        print("~~~~~~Books removed~~~~~~")
        print()



##FOR ADDING BOOKS
        
    elif x==5:
        print()
        q=int(input("In Which Table you want to Add Records?\n1.library\n2.info"\
           "\nSelect your choice:"))
        if q== 1:
           print()
           i=int(input("Enter no. of records:"))
           print()
           for b in range(i):
               s=int(input("Enter book id:"))
               n=input("Enter book name:")
               z=input("Enter Genere of the book:")
               p=float(input("Enter book price:"))
               q=int(input("Enter number of books:"))
               k="insert into library(Bid,B_name,Genere,B_price,Qty) values({},'{}','{}',{},{});".format(s,n,z,p,q)
               cur.execute(k)
               dbs.commit()
               print() 
               print("~~~~~Recordes added~~~~~") 
               print()
        elif q==2:
            print()
            j=int(input("enter no.of records:"))
            print()
            for b in range(j):
                w=int(input("Enter book id:"))
                a=input("Enter Author of the Book:")
                z=input("Demand of the Book is:")
                l=input("enter issue date(yy/mm/dd):")
                t="insert into info values({},'{}','{}','{}')".format(w,a,z,l)
                cur.execute(t)
                dbs.commit()
            print()
            print("~~~~~Recordes added~~~~~")
            print()
        else:
            print("Invalid choice")
            print()
        print()

##FAVOURABLE BOOKS
        
    elif x==6:
        print()
        cur.execute("select B_name from library,info where library.Bid=info.Bid;")
        a=[]
        rc=cur.fetchall()
        for x in rc:
            a.append(x[0])
        print(a)
        cur.execute("select Demand_of_book from library,info where library.Bid=info.Bid;")
        b=[]
        re=cur.fetchall()
        for x in re:
            b.append(x[0])
        print(b)
        plt.axis("equal")
        plt.title(" Most Read Books")
        plt.pie(b,labels=a,autopct="%1.1f%%")
        plt.show()

##Genere OF BOOKS

    elif x==7:
        print()
        print("\t~~~~~Various Generes of Books ~~~~~\t")
        v="select count(*),Genere from library group by Genere;"
        cur.execute(v)
        for x in cur:
            print()
            print("Genere:",x[1])
            print("Books Availaible:",x[0])
            print()
        print()

##DISPLAYING INFORMATION OF ALL BOOKS
            
    elif x==8:
        print()
        print("*****Displaying All Books from  Library with all Infomation*****")
        print()
        t="select library.Bid,B_name,Genere,B_price,Qty,Author_name,Demand_of_Book,Date_of_issue from library,info where library.Bid=info.Bid;"
        cur.execute(t)
        rec=cur.fetchall()
        z=0
        for x in rec:
            z+=1
            print(">>Record",z,":~")
            print()
            print("ID:",x[0])
            print("Book Name:",x[1])
            print("Author Name:",x[5])
            print("Genere:",x[2])
            print("costs:",x[3],"Rs")
            print("Quantity in library:",x[4])
            print("Demand of the Book:",x[6])
            print("Date of issue:",x[7])
            print()
        print()

##DISPLAYING INFORMATION OF A SPECIFIC BOOK
        
    elif x==9:
        print()
        print("*****Displaying specific Book from  Library with all Infomation*****")
        qu="select B_name from library,info where library.Bid=info.Bid;"
        cur.execute(qu)
        rec=cur.fetchall()
        print()
        print("Book availaible are:")
        print()
        for a in rec:
            print(">>",a[0])
        print()
        root=Tk()
        root.title("DISPLAYING ALL INFORMATION FROM LIBRARY")
        label1=Label(root,text="Enter Book Name")
        label1.grid(row=10,column=20)
        txt1=Entry(root)
        txt1.grid(row=20,column=20)
        ans=Label(root,text="Number_of_books")
        ans.grid(row=30,column=20)
        ans1=Label(root,text="Book_Price")
        ans1.grid(row=40,column=20)
        def db():
            a=txt1.get()
            k="select Qty from library where B_name='%s';"%(a)
            cur.execute(k)
            rec=cur.fetchall()
            for x in rec:
                q=x[0]
            i="select B_price from library where B_name='%s';"%(a)
            cur.execute(i)
            rec2=cur.fetchall()
            for x in rec2:
                p=x[0]
            ans1.configure(text=p)
            ans.configure(text=q)
            j="select Date_of_issue from info,library where B_name='%s' and library.Bid=info.Bid;"%(a)
            cur.execute(j)
            rec1=cur.fetchall()
            status_label.configure(text=rec1)
        b1=Button(root,text="Display",command=db)
        b1.grid(row=50,column=20,columnspan=2)
        status_label=Label(root,height=10,width=60,bg="white",fg="blue",text="Date_of_issue")
        status_label.grid(row=60,column=20)
        root.mainloop()
        print()
        
##TO EXIT THE PROGRAM

    elif x==10:
        print()
        print("\t-----------------\t")
        print("\t<<<<<Thank you>>>>>\t")
        print("\t------------------\t")
        print()
        dbs.commit()
    break
