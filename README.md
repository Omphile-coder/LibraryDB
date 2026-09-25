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
