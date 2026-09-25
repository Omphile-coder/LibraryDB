# Library Management System 📚

A PostgreSQL database project for managing library books, authors, and patrons. Using a sample catalog of South African literature, it demonstrates relationships, CRUD operations, array columns, and SQL queries.

##  Features

- **Relational design:** A foreign key links each book to an author.
- **CRUD operations:** Create, read, update, and delete records across the tables.
- **Advanced queries:** Filter records, match text patterns, and perform bulk updates.
- **PostgreSQL arrays:** Store multiple genres per book and borrowed book IDs per patron.

##  Tech stack

- **Database:** PostgreSQL
- **Management tools:** pgAdmin 4 or psql
- **Language:** SQL


## 🚀 Installation and setup

1. Install [PostgreSQL](https://www.postgresql.org/) and [pgAdmin 4](https://www.pgadmin.org/).
2. Open pgAdmin 4 and connect to your PostgreSQL server.
3. Right-click **Databases** → **Create** → **Database...**, enter `LibraryDB`, and save.
4. Right-click `LibraryDB` → **Query Tool**.
5. Run the schema statements below, followed by the seed data. Run the usage examples separately as needed; some examples modify or delete data.

---

## 🗄️ Database schema and initialization

### 1. Create tables

```sql
CREATE TABLE authors (
    id INT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    nationality VARCHAR(100),
    birth_year INT,
    death_year INT
);

CREATE TABLE books (
    id INT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    author_id INT NOT NULL,
    genres TEXT[],
    published_year INT,
    available BOOLEAN DEFAULT TRUE,
    
    CONSTRAINT fk_books_authors
        FOREIGN KEY (author_id) 
        REFERENCES authors(id)
);

CREATE TABLE patrons (
    id INT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    borrowed_books INT[] DEFAULT ARRAY[]::INT[]
);
```


### 2. Seed data (South African literature)

```sql
-- Insert authors
INSERT INTO authors (id, name, nationality, birth_year, death_year) VALUES
    (1, 'J.M. Coetzee', 'South African', 1940, NULL),
    (2, 'Nadine Gordimer', 'South African', 1923, 2014),
    (3, 'Zakes Mda', 'South African', 1948, NULL),
    (4, 'Alan Paton', 'South African', 1903, 1988),
    (5, 'Trevor Noah', 'South African', 1984, NULL),
    (6, 'Bessie Head', 'South African', 1937, 1986),
    (7, 'Deon Meyer', 'South African', 1958, NULL),
    (8, 'Lauren Beukes', 'South African', 1976, NULL),
    (9, 'Sol Plaatje', 'South African', 1876, 1932),
    (10, 'Athol Fugard', 'South African', 1932, NULL);

-- Insert books
INSERT INTO books (id, title, author_id, genres, published_year, available) VALUES
    (1, 'Disgrace', 1, ARRAY['Literary Fiction'], 1999, TRUE),
    (2, 'Burger''s Daughter', 2, ARRAY['Political Fiction', 'Historical'], 1979, TRUE),
    (3, 'The Heart of Redness', 3, ARRAY['Historical Fiction'], 2000, TRUE),
    (4, 'Cry, the Beloved Country', 4, ARRAY['Tragedy', 'Social Commentary'], 1948, TRUE),
    (5, 'Born a Crime', 5, ARRAY['Autobiography', 'Comedy'], 2016, TRUE),
    (6, 'Maru', 6, ARRAY['Fiction', 'African Literature'], 1971, TRUE),
    (7, 'Thirteen Hours', 7, ARRAY['Crime Thriller'], 2008, TRUE),
    (8, 'The Shining Girls', 8, ARRAY['Science Fiction', 'Thriller'], 2013, TRUE),
    (9, 'Mhudi', 9, ARRAY['Historical Fiction'], 1930, TRUE),
    (10, 'Tsotsi', 10, ARRAY['Crime Fiction', 'Drama'], 1980, TRUE);

-- Insert patrons
INSERT INTO patrons (id, name, email, borrowed_books) VALUES
    (1, 'Alice Johnson', 'alice@example.com', ARRAY[]::INT[]),
    (2, 'Bob Smith', 'bob@example.com', ARRAY[1, 2]),
    (3, 'Carol White', 'carol@example.com', ARRAY[]::INT[]),
    (4, 'David Brown', 'david@example.com', ARRAY[3]),
    (5, 'Eve Davis', 'eve@example.com', ARRAY[]::INT[]),
    (6, 'Frank Moore', 'frank@example.com', ARRAY[4, 5]),
    (7, 'Grace Miller', 'grace@example.com', ARRAY[]::INT[]),
    (8, 'Hank Wilson', 'hank@example.com', ARRAY[6]),
    (9, 'Ivy Taylor', 'ivy@example.com', ARRAY[]::INT[]),
    (10, 'Jack Anderson', 'jack@example.com', ARRAY[7, 8]);
```

---

## 💻 Usage and operations

### Read operations

```sql
-- Get all books
SELECT * FROM books;

-- Get a book by title
SELECT * FROM books WHERE title = 'Cry, the Beloved Country';

-- Get all books by a specific author
SELECT books.*
FROM books
JOIN authors ON books.author_id = authors.id
WHERE authors.name = 'Alan Paton';

-- Get all available books
SELECT * FROM books WHERE available = TRUE;
```

### Update operations

```sql
-- Mark a book as borrowed
UPDATE books SET available = FALSE WHERE title = 'Cry, the Beloved Country';

-- Add a new genre to an existing book
UPDATE books
SET genres = array_append(genres, 'Classic Literature')
WHERE title = 'Cry, the Beloved Country';

-- Add a borrowed book to a patron's record
UPDATE patrons
SET borrowed_books = array_append(borrowed_books, 4)
WHERE id = 1;
```
