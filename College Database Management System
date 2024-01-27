import mysql.connector as mysql

db = mysql.connect(host="localhost",user="root",password="",database="college")
command_handler = db.cursor(buffered=True)

def teacher_session():
    while 1:
        print("")
        print("Teacher's Menu")
        print("1. Mark Attendance")
        print("2. View Attendance")
        print("3. Logout")

        user_option=input(str("Option: "))
        if user_option == "1":
            print("")
            print("Mark Student Attendance")
            command_handler.execute("SELECT username FROM users WHERE privilege='student'")
            records = command_handler.fetchall()
            date = input(str(" Date: DD/MM/YYYY: "))
            for record in records:
                record = str(record).replace("'","")
                record = str(record).replace(",","")
                record = str(record).replace("(","")
                record = str(record).replace(")","")
                #present|absent|late
                status=input(str("Status for " +str(record)+" P/A/L: "))
                query_vals=(str(record),date,status)
                command_handler.execute("INSERT INTO attendance (username, date, status) VALUES (%s,%s,%s)",query_vals)
                db.commit()
                print(record + " Marked as " +status)
        elif user_option == "2":
            print("")
            print("Viewing all student attendance")
            command_handler.execute("SELECT username, date, status FROM attendance")
            records = command_handler.fetchall()
            print("Displaying all registers")
            for record in records:
                print(record)
        elif user_option == "3":
            break
        else:
            print("No valid option was selected")

def student_session(username):
    while 1:
        print("")
        print("View Attendance")
        print("Download Attendance")
        print("Logout")

        user_option=input(str("Options: "))
        if user_option=="1":
            print(username)
            username=(str(username),)
            command_handler.execute("SELECT date, username, status FROM attendance WHERE username=%s",username)
            records=command_handler.fetchall()
            for record in records:
                print(record)
        elif user_option=="2":
            print("Downloading Register")
            username=(str(username),)
            command_handler.execute("SELECT date, username, status FROM attendance WHERE username=%s",username)
            records=command_handler.fetchall()
            for record in records:
                           #CHANGE PATH AS REQUIRED
                with open("C:/Users/KUIKO/Desktop/register.txt","w") as f: 
                    f.write(str(records)+"\n")
                f.close()
            print("All Records Saved")
        elif user_option=="3":
            break
        else:
            print("No valid option was selected")



def admin_session():
    while 1:
        print("")
        print("Admin menu")
        print("1. Register new student")
        print("2. Register new teacher")
        print("3. Delete existing student")
        print("4. Delete existing teacher")
        print("5. Logout")

        user_option=input(str("Option: "))
        if user_option == "1":
            print("")
            print("Register New Student")
            username=input(str("Student Username: "))
            password=input(str("Student Password: "))
            query_vals=(username,password)
            command_handler.execute("INSERT INTO users(username,password,privilege) VALUES(%s,%s,'student')",query_vals)
            db.commit()
            print(username+" has been registered as a student")
        elif user_option =="2":
            print("")
            print("Register New Teacher")
            username=input(str("Teacher Username: "))
            password=input(str("Teacher Password: "))
            query_vals=(username,password)
            command_handler.execute("INSERT INTO users(username,password,privilege) VALUES(%s,%s,'teacher')",query_vals)
            db.commit()
            print(username+" has been registered as a teacher")
        elif user_option == "3":
            print("")
            print("Delete Exisiting Student Account")
            username=input(str("Student Username: "))
            query_vals = (username,"student")
            command_handler.execute("DELETE FROM users WHERE username= %s AND PRIVILEGE=%s", query_vals)
            db.commit()
            if command_handler.rowcount < 1:
                print("User not found")
            else:
                print(username+ " has been deleted")
        elif user_option == "4":
            print("")
            print("Delete Exisiting Teacher Account")
            username=input(str("Teacher Username: "))
            query_vals = (username,"teacher")
            command_handler.execute("DELETE FROM users WHERE username= %s AND PRIVILEGE=%s", query_vals)
            db.commit()
            if command_handler.rowcount < 1:
                print("User not found")
            else:
                print(username+ " has been deleted")
        elif user_option == "5":
            break
        else:
            print("No valid option selected")

def auth_student():
    print("")
    print("Student's Login")
    print("")
    username=input(str("Username: "))
    password=input(str("Password: "))
    query_vals=(username,password,"student")
    command_handler.execute("SELECT username FROM users WHERE username=%s AND password=%s AND privilege=%s",query_vals)
    if command_handler.rowcount<1:
        print("Invalid Login Details")
    else:
        student_session(username)

def auth_teacher():
    print("")
    print("Teacher's Login")
    print("")
    username=input(str("Username: "))
    password=input(str("Password: "))
    query_vals=(username,password)
    command_handler.execute("SELECT*FROM users WHERE username= %s AND password= %s AND privilege= 'teacher'",query_vals)
    if command_handler.rowcount<=0:
        print("Login not recognized")
    else:
        teacher_session()
def auth_admin():
 print("")
 print("Admin login")
 print("")
 username=str(input("Username: "))
 password=str(input("Password: "))
 if username == "admin":
    if password == "password":
         admin_session()
    else:
         print("Incorrect Password")
 else: 
    print("Login not recognized")

def main():
    while True:
        print("Welcome to the college system")
        print("")
        print("1. Login as student")
        print("2. Login as teacher")
        print("3. Login as admin")

        user_option=str(input("Option: "))
        if user_option == "1":
            auth_student()
        elif user_option == "2":
            auth_teacher()
        elif user_option =="3":
            auth_admin()
        else:
            print("No valid option was selected")
main()
