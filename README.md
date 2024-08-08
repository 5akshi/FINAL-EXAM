# FINAL-EXAM
# SENG8080::SAKSHI_JATIN_TASNIM_ FINAL EXAM 

# BOOKSTORE CRUD 

```sql
## Create Table of ORM

Entity: Author

import { Entity, PrimaryGeneratedColumn, Column, OneToMany } from 'typeorm';
import { Book } from './Book';

@Entity('Authors')
export class Author {
    @PrimaryGeneratedColumn()
    id: number;

    @Column({ type: 'varchar' })
    name: string;

    @Column({ type: 'text', nullable: true })
    bio: string;

    @Column({ type: 'varchar' })
    genre: string;

    @OneToMany(() => Book, (book) => book.author)
    books: Book[];
}

### Publisher entity

Entity: Publisher

import { Entity, PrimaryGeneratedColumn, Column, OneToMany } from 'typeorm';
import { Book } from './Book';

@Entity('Publishers')
export class Publisher {
    @PrimaryGeneratedColumn()
    id: number;

    @Column({ type: 'varchar' })
    name: string;

    @Column({ type: 'varchar' })
    address: string;

    @OneToMany(() => Book, (book) => book.publisher)
    books: Book[];
}


Entity: Book

import { Entity, PrimaryGeneratedColumn, Column, ManyToOne } from 'typeorm';
import { Author } from './Author';
import { Publisher } from './Publisher';

@Entity('Books')
export class Book {
    @PrimaryGeneratedColumn()
    id: number;

    @Column({ type: 'varchar' })
    title: string;

    @Column({ type: 'varchar' })
    genre: string;

    @Column({ type: 'date' })
    publicationDate: Date;

    @Column({ type: 'varchar' })
    type: string;

    @ManyToOne(() => Author, (author) => author.books, { onDelete: 'CASCADE' })
    author: Author;

    @ManyToOne(() => Publisher, (publisher) => publisher.books, { onDelete: 'CASCADE' })
    publisher: Publisher;
}


Entity: Customer

import { Entity, PrimaryGeneratedColumn, Column, OneToMany } from 'typeorm';
import { Purchase } from './Purchase';
import { Review } from './Review';

@Entity('Customers')
export class Customer {
    @PrimaryGeneratedColumn()
    id: number;

    @Column({ type: 'varchar' })
    name: string;

    @Column({ type: 'varchar' })
    email: string;

    @Column({ type: 'numeric', default: 0 })
    totalSpent: number;

    @OneToMany(() => Purchase, (purchase) => purchase.customer)
    purchases: Purchase[];

    @OneToMany(() => Review, (review) => review.customer)
    reviews: Review[];
}


Entity: Purchase

import { Entity, PrimaryGeneratedColumn, Column, ManyToOne } from 'typeorm';
import { Customer } from './Customer';
import { Book } from './Book';

@Entity('Purchases')
export class Purchase {
    @PrimaryGeneratedColumn()
    id: number;

    @ManyToOne(() => Customer, (customer) => customer.purchases, { onDelete: 'CASCADE' })
    customer: Customer;

    @ManyToOne(() => Book, (book) => book, { onDelete: 'CASCADE' })
    book: Book;

    @Column({ type: 'date' })
    purchaseDate: Date;

    @Column({ type: 'numeric' })
    price: number;
}



Entity: Review

import { Entity, PrimaryGeneratedColumn, Column, ManyToOne } from 'typeorm';
import { Customer } from './Customer';
import { Book } from './Book';

@Entity('Reviews')
export class Review {
    @PrimaryGeneratedColumn()
    id: number;

    @ManyToOne(() => Customer, (customer) => customer.reviews, { onDelete: 'CASCADE' })
    customer: Customer;

    @ManyToOne(() => Book, (book) => book, { onDelete: 'CASCADE' })
    book: Book;

    @Column({ type: 'numeric' })
    rating: number;

    @Column({ type: 'text', nullable: true })
    reviewText: string;

    @Column({ type: 'date' })
    postedDate: Date;
}
```

# SENG8080::SAKSHI_JATIN_TASNIM_ FINAL EXAM 

# BOOKSTORE CRUD 

```sql
## Insert orm

import { getRepository } from 'typeorm';
import { Author } from './entities/Author';
import { Publisher } from './entities/Publisher';
import { Book } from './entities/Book';
import { Customer } from './entities/Customer';
import { Purchase } from './entities/Purchase';
import { Review } from './entities/Review';

async function seedDatabase() {
    const authorRepository = getRepository(Author);
    const publisherRepository = getRepository(Publisher);
    const bookRepository = getRepository(Book);
    const customerRepository = getRepository(Customer);
    const purchaseRepository = getRepository(Purchase);
    const reviewRepository = getRepository(Review);

    // Insert Authors
    await authorRepository.save([
        { name: 'J.K. Rowling', bio: 'British author of the Harry Potter series', genre: 'Fantasy' },
        { name: 'George R.R. Martin', bio: 'American novelist known for A Song of Ice and Fire', genre: 'Fantasy' },
        { name: 'Agatha Christie', bio: 'English writer known for her detective novels', genre: 'Mystery' },
        { name: 'J.R.R. Tolkien', bio: 'English writer known for The Lord of the Rings', genre: 'Fantasy' },
        { name: 'Stephen King', bio: 'American author of horror, supernatural fiction, and fantasy', genre: 'Horror' },
        { name: 'Isaac Asimov', bio: 'American writer and professor of biochemistry, known for science fiction', genre: 'Science Fiction' },
        { name: 'Arthur Conan Doyle', bio: 'British writer, best known for his Sherlock Holmes series', genre: 'Mystery' },
        { name: 'Brandon Sanderson', bio: 'American author of epic fantasy and science fiction', genre: 'Fantasy' },
        { name: 'Suzanne Collins', bio: 'American television writer and author, known for The Hunger Games', genre: 'Dystopian' },
        { name: 'Margaret Atwood', bio: 'Canadian poet, novelist, and literary critic', genre: 'Dystopian' }
    ]);

    // Insert Publishers
    await publisherRepository.save([
        { name: 'Bloomsbury Publishing', address: '50 Bedford Square, London' },
        { name: 'Bantam Books', address: '1745 Broadway, New York' },
        { name: 'HarperCollins', address: '195 Broadway, New York' },
        { name: 'Penguin Random House', address: '375 Hudson Street, New York' },
        { name: 'Hachette Book Group', address: '1290 Avenue of the Americas, New York' },
        { name: 'Simon & Schuster', address: '1230 Avenue of the Americas, New York' },
        { name: 'Scholastic Corporation', address: '557 Broadway, New York' },
        { name: 'Tor Books', address: '175 Fifth Avenue, New York' },
        { name: 'Gollancz', address: 'Carmelite House, London' },
        { name: 'Orbit Books', address: 'Little, Brown Book Group, London' }
    ]);

    // Insert Books
    await bookRepository.save([
        { title: 'Harry Potter and the Philosopher\'s Stone', genre: 'Fantasy', publicationDate: '1997-06-26', type: 'Physical', author: 1, publisher: 1 },
        { title: 'A Game of Thrones', genre: 'Fantasy', publicationDate: '1996-08-06', type: 'Physical', author: 2, publisher: 2 },
        { title: 'The Hobbit', genre: 'Fantasy', publicationDate: '1937-09-21', type: 'Physical', author: 4, publisher: 3 },
        { title: 'The Shining', genre: 'Horror', publicationDate: '1977-01-28', type: 'Physical', author: 5, publisher: 4 },
        { title: 'Foundation', genre: 'Science Fiction', publicationDate: '1951-05-01', type: 'Physical', author: 6, publisher: 5 },
        { title: 'Sherlock Holmes: A Study in Scarlet', genre: 'Mystery', publicationDate: '1887-11-01', type: 'Physical', author: 7, publisher: 6 },
        { title: 'Mistborn: The Final Empire', genre: 'Fantasy', publicationDate: '2006-07-17', type: 'Physical', author: 8, publisher: 7 },
        { title: 'The Hunger Games', genre: 'Dystopian', publicationDate: '2008-09-14', type: 'Physical', author: 9, publisher: 8 },
        { title: 'The Handmaid\'s Tale', genre: 'Dystopian', publicationDate: '1985-09-01', type: 'Physical', author: 10, publisher: 9 },
        { title: 'And Then There Were None', genre: 'Mystery', publicationDate: '1939-11-06', type: 'Physical', author: 3, publisher: 10 }
    ]);

    // Insert Customers
    await customerRepository.save([
        { name: 'Alice Johnson', email: 'alice.johnson@example.com', totalSpent: 150.00 },
        { name: 'Bob Smith', email: 'bob.smith@example.com', totalSpent: 200.50 },
        { name: 'Carol White', email: 'carol.white@example.com', totalSpent: 250.75 },
        { name: 'David Brown', email: 'david.brown@example.com', totalSpent: 120.30 },
        { name: 'Eva Green', email: 'eva.green@example.com', totalSpent: 175.20 },
        { name: 'Frank Black', email: 'frank.black@example.com', totalSpent: 225.60 },
        { name: 'Grace Davis', email: 'grace.davis@example.com', totalSpent: 180.40 },
        { name: 'Hank Wilson', email: 'hank.wilson@example.com', totalSpent: 220.80 },
        { name: 'Ivy Moore', email: 'ivy.moore@example.com', totalSpent: 140.00 },
        { name: 'Jack Taylor', email: 'jack.taylor@example.com', totalSpent: 190.90 }
    ]);

    // Insert Purchases
    await purchaseRepository.save([
        { customer: 1, book: 1, purchaseDate: '2023-05-15', price: 15.00 },
        { customer: 2, book: 2, purchaseDate: '2023-06-20', price: 20.50 },
        { customer: 3, book: 3, purchaseDate: '2023-07-25', price: 25.75 },
        { customer: 4, book: 4, purchaseDate: '2023-08-30', price: 12.30 },
        { customer: 5, book: 5, purchaseDate: '2023-09-10', price: 17.20 },
        { customer: 6, book: 6, purchaseDate: '2023-10-05', price: 22.60 },
        { customer: 7, book: 7, purchaseDate: '2023-11-12', price: 18.40 },
        { customer: 8, book: 8, purchaseDate: '2023-12-18', price: 22.80 },
        { customer: 9, book: 9, purchaseDate: '2023-01-25', price: 14.00 },
        { customer: 10, book: 10, purchaseDate: '2023-02-15', price: 19.90 }
    ]);

    // Insert Reviews
    await reviewRepository.save([
        { customer: 1, book: 1, rating: 5, reviewText: 'Amazing book!', postedDate: '2024-01-01' },
        { customer: 2, book: 2, rating: 4.8, reviewText: 'Fantastic storytelling.', postedDate: '2024-01-02' },
        { customer: 3, book: 3, rating: 4.5, reviewText: 'A classic!', postedDate: '2024-01-03' },
        { customer: 4, book: 4, rating: 4.7, reviewText: 'Chilling and captivating.', postedDate: '2024-01-04' },
        { customer: 5, book: 5, rating: 4.2, reviewText: 'Mind-blowing science fiction.', postedDate: '2024-01-05' },
        { customer: 6, book: 6, rating: 4.9, reviewText: 'Brilliant detective work.', postedDate: '2024-01-06' },
        { customer: 7, book: 7, rating: 4.6, reviewText: 'Epic fantasy at its best.', postedDate: '2024-01-07' },
        { customer: 8, book: 8, rating: 4.4, reviewText: 'Compelling and intense.', postedDate: '2024-01-08' },
        { customer: 9, book: 9, rating: 4.3, reviewText: 'A chilling dystopia.', postedDate: '2024-01-09' },
        { customer: 10, book: 10, rating: 4.1, reviewText: 'A must-read mystery.', postedDate: '2024-01-10' }
    ]);
}

seedDatabase().catch(error => console.log(error));
```
# SENG8080::SAKSHI_JATIN_TASNIM_ FINAL EXAM 

# Bookstore - Requirement queries
1. Power Writers (Authors) with More Than 5 Books in the Same Genre Published Within the Last 10 Years

SELECT 
    author.id, 
    author.name, 
    COUNT(book.id) AS bookCount
FROM 
    Book book
INNER JOIN 
    Author author ON book.authorId = author.id
WHERE 
    book.genre = 'Fantasy'
    AND book.publicationDate > DATE_SUB(CURDATE(), INTERVAL 10 YEAR)
GROUP BY 
    author.id
HAVING 
    COUNT(book.id) > 5;


2. Loyal Customers Who Have Spent More Than $200 in the Last Year

SELECT 
    customer.id, 
    customer.name, 
    SUM(purchase.price) AS totalSpent
FROM 
    Purchase purchase
INNER JOIN 
    Customer customer ON purchase.customerId = customer.id
WHERE 
    purchase.purchaseDate > DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
GROUP BY 
    customer.id
HAVING 
    SUM(purchase.price) > 200;



3. Well-Reviewed Books that Have a Better User Rating Than Average

-- First get the average rating
SELECT 
    AVG(rating) AS avgRating
FROM 
    Review;

-- Then get well-reviewed books
SELECT 
    book.id, 
    book.title, 
    AVG(review.rating) AS averageRating
FROM 
    Review review
INNER JOIN 
    Book book ON review.bookId = book.id
GROUP BY 
    book.id
HAVING 
    AVG(review.rating) > (SELECT AVG(rating) FROM Review);


4. The Most Popular Genre by Sales

SELECT 
    book.genre, 
    SUM(purchase.price) AS totalSales
FROM 
    Purchase purchase
INNER JOIN 
    Book book ON purchase.bookId = book.id
GROUP BY 
    book.genre
ORDER BY 
    totalSales DESC
LIMIT 1;



5. The 10 Most Recent Posted Reviews by Customers

SELECT 
    review.*, 
    customer.*, 
    book.*
FROM 
    Review review
INNER JOIN 
    Customer customer ON review.customerId = customer.id
INNER JOIN 
    Book book ON review.bookId = book.id
ORDER BY 
    review.postedDate DESC
LIMIT 10;
```
