# Create-a-database-Portfolio-Project
#Create the Database Schema in PostgreSQL
-- Create database
CREATE DATABASE library_management;

-- Connect to the database
\c library_management;

-- Create the Authors table
CREATE TABLE Authors (
    author_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    bio TEXT
);

-- Create the Categories table
CREATE TABLE Categories (
    category_id SERIAL PRIMARY KEY,
    category_name VARCHAR(100) NOT NULL
);

-- Create the Books table
CREATE TABLE Books (
    book_id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    author_id INT REFERENCES Authors(author_id),
    category_id INT REFERENCES Categories(category_id),
    isbn VARCHAR(20) UNIQUE,
    publication_year INT
);

-- Create the Members table
CREATE TABLE Members (
    member_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    address VARCHAR(255),
    phone_number VARCHAR(20),
    email VARCHAR(100) UNIQUE
);

-- Create the Loans table
CREATE TABLE Loans (
    loan_id SERIAL PRIMARY KEY,
    book_id INT REFERENCES Books(book_id),
    member_id INT REFERENCES Members(member_id),
    loan_date DATE NOT NULL,
    due_date DATE NOT NULL,
    return_date DATE
);

-- Create the Reservations table
CREATE TABLE Reservations (
    reservation_id SERIAL PRIMARY KEY,
    book_id INT REFERENCES Books(book_id),
    member_id INT REFERENCES Members(member_id),
    reservation_date DATE NOT NULL,
    status VARCHAR(50) NOT NULL
);

#Create the Database and Tables
-- Step 1: Create the Database
CREATE DATABASE library_management;

-- Step 2: Connect to the Database
\c library_management;

-- Step 3: Create the Tables

-- Authors Table
CREATE TABLE Authors (
    author_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    bio TEXT
);

-- Categories Table
CREATE TABLE Categories (
    category_id SERIAL PRIMARY KEY,
    category_name VARCHAR(100) NOT NULL
);

-- Books Table
CREATE TABLE Books (
    book_id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    author_id INT REFERENCES Authors(author_id),
    category_id INT REFERENCES Categories(category_id),
    isbn VARCHAR(20) UNIQUE,
    publication_year INT,
    availability_status BOOLEAN DEFAULT TRUE -- TRUE if available, FALSE if loaned out
);

-- Members Table
CREATE TABLE Members (
    member_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    address VARCHAR(255),
    phone_number VARCHAR(20),
    email VARCHAR(100) UNIQUE
);

-- Loans Table
CREATE TABLE Loans (
    loan_id SERIAL PRIMARY KEY,
    book_id INT REFERENCES Books(book_id),
    member_id INT REFERENCES Members(member_id),
    loan_date DATE NOT NULL,
    due_date DATE NOT NULL,
    return_date DATE,
    fine DECIMAL(10, 2) DEFAULT 0.00 -- Fine for overdue books
);

-- Reservations Table
CREATE TABLE Reservations (
    reservation_id SERIAL PRIMARY KEY,
    book_id INT REFERENCES Books(book_id),
    member_id INT REFERENCES Members(member_id),
    reservation_date DATE NOT NULL,
    status VARCHAR(50) NOT NULL -- Status could be 'pending', 'completed', 'cancelled'
);

#Populating the Database

-- Insert into Authors
INSERT INTO Authors (name, bio) VALUES
('J.K. Rowling', 'British author, best known for the Harry Potter series.'),
('George Orwell', 'English novelist, essayist, and critic.'),
('J.R.R. Tolkien', 'English writer, poet, philologist, and academic.');

-- Insert into Categories
INSERT INTO Categories (category_name) VALUES
('Fantasy'),
('Science Fiction'),
('Non-Fiction');

-- Insert into Books
INSERT INTO Books (title, author_id, category_id, isbn, publication_year) VALUES
('Harry Potter and the Philosopher\'s Stone', 1, 1, '9780747532699', 1997),
('1984', 2, 2, '9780451524935', 1949),
('The Hobbit', 3, 1, '9780345339683', 1937);

-- Insert into Members
INSERT INTO Members (name, address, phone_number, email) VALUES
('Alice Johnson', '123 Maple Street', '555-1234', 'alice@example.com'),
('Bob Smith', '456 Oak Avenue', '555-5678', 'bob@example.com');

-- Insert into Loans
INSERT INTO Loans (book_id, member_id, loan_date, due_date, return_date, fine) VALUES
(1, 1, '2024-08-01', '2024-08-15', '2024-08-14', 0.00),
(2, 2, '2024-08-10', '2024-08-24', NULL, 0.00);

-- Insert into Reservations
INSERT INTO Reservations (book_id, member_id, reservation_date, status) VALUES
(3, 1, '2024-08-20', 'pending');

