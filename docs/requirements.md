# Requirements

## User Roles
- Admin / Librarian
- Member

## Functional Requirements
### Books
- Add book
- View books
- View one book
- Update book
- Delete book

### Members
- Add member
- View members
- Update member

### Loans
- Borrow book
- Return book
- View active loans
- View loan history

## Business Rules
- A borrowed book cannot be borrowed again until returned.
- A loan must belong to an existing member.
- A loan must belong to an existing book.
- ISBN must be unique.
- Member email must be unique.
- Returning a book should make it available again.

## Non-Functional Requirements
- API should return clear errors.
- Important data must be stored in PostgreSQL.
- Redis will be used for caching.
- The application will run with Docker.
- Nginx will be the entry point in production.

## Out of Scope for MVP
- Payments
- Fines
- Multiple library branches
- Email notifications