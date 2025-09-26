
import sqlite3

# Database connection
conn = sqlite3.connect("library.db")
cursor = conn.cursor()

# Create tables
cursor.execute('''CREATE TABLE IF NOT EXISTS books (
                    book_id INTEGER PRIMARY KEY AUTOINCREMENT,
                    title TEXT,
                    author TEXT,
                    available INTEGER)''')

cursor.execute('''CREATE TABLE IF NOT EXISTS students (
                    student_id INTEGER PRIMARY KEY AUTOINCREMENT,
                    name TEXT,
                    roll_no TEXT)''')

cursor.execute('''CREATE TABLE IF NOT EXISTS transactions (
                    trans_id INTEGER PRIMARY KEY AUTOINCREMENT,
                    student_id INTEGER,
                    book_id INTEGER,
                    action TEXT,
                    date TIMESTAMP DEFAULT CURRENT_TIMESTAMP)''')

conn.commit()

# Add book
def add_book(title, author):
    cursor.execute("INSERT INTO books (title, author, available) VALUES (?, ?, ?)", (title, author, 1))
    conn.commit()
    print(f"Book '{title}' added successfully!")

# Add student
def add_student(name, roll_no):
    cursor.execute("INSERT INTO students (name, roll_no) VALUES (?, ?)", (name, roll_no))
    conn.commit()
    print(f"Student '{name}' added successfully!")

# Borrow book
def borrow_book(student_id, book_id):
    cursor.execute("SELECT available FROM books WHERE book_id=?", (book_id,))
    book = cursor.fetchone()
    if book and book[0] == 1:
        cursor.execute("UPDATE books SET available=0 WHERE book_id=?", (book_id,))
        cursor.execute("INSERT INTO transactions (student_id, book_id, action) VALUES (?, ?, ?)", (student_id, book_id, "Borrow"))
        conn.commit()
        print("Book borrowed successfully!")
    else:
        print("Book not available!")

# Return book
def return_book(student_id, book_id):
    cursor.execute("SELECT available FROM books WHERE book_id=?", (book_id,))
    book = cursor.fetchone()
    if book and book[0] == 0:
        cursor.execute("UPDATE books SET available=1 WHERE book_id=?", (book_id,))
        cursor.execute("INSERT INTO transactions (student_id, book_id, action) VALUES (?, ?, ?)", (student_id, book_id, "Return"))
        conn.commit()
        print("Book returned successfully!")
    else:
        print("This book was not borrowed!")

# Show all books
def show_books():
    cursor.execute("SELECT * FROM books")
    books = cursor.fetchall()
    print("\nBooks in Library:")
    for book in books:
        status = "Available" if book[3] == 1 else "Issued"
        print(f"ID: {book[0]}, Title: {book[1]}, Author: {book[2]}, Status: {status}")

# Show transactions
def show_transactions():
    cursor.execute('''SELECT t.trans_id, s.name, b.title, t.action, t.date 
                      FROM transactions t
                      JOIN students s ON t.student_id = s.student_id
                      JOIN books b ON t.book_id = b.book_id''')
    records = cursor.fetchall()
    print("\nTransaction History:")
    for row in records:
        print(row)

# Main menu
def main():
    while True:
        print("\n====== Library Management System ======")
        print("1. Add Book")
        print("2. Add Student")
        print("3. Borrow Book")
        print("4. Return Book")
        print("5. Show All Books")
        print("6. Show Transactions")
        print("7. Exit")

        choice = input("Enter choice: ")

        if choice == "1":
            title = input("Enter book title: ")
            author = input("Enter book author: ")
            add_book(title, author)

        elif choice == "2":
            name = input("Enter student name: ")
            roll = input("Enter roll number: ")
            add_student(name, roll)

        elif choice == "3":
            sid = int(input("Enter student ID: "))
            bid = int(input("Enter book ID: "))
            borrow_book(sid, bid)

        elif choice == "4":
            sid = int(input("Enter student ID: "))
            bid = int(input("Enter book ID: "))
            return_book(sid, bid)

        elif choice == "5":
            show_books()

        elif choice == "6":
            show_transactions()

        elif choice == "7":
            print("Exiting... Goodbye!")
            break

        else:
            print("Invalid choice! Try again.")

if __name__ == "__main__":
    main()
    conn.close()
