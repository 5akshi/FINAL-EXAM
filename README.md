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
