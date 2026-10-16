# API Plan

## Base URL

/api


# 1. Authentication

## POST /api/auth/login

Purpose:
- Admin/Librarian login.

Access:
- Public

Input:
- email
- password

Success:
- Returns authenticated user information and authentication token.

Possible Errors:
- 400 — Missing information
- 401 — Invalid email or password


## GET /api/auth/me

Purpose:
- Get the currently logged-in user.

Access:
- Authenticated Admin/Librarian

Success:
- Returns current user information.

Possible Errors:
- 401 — Not authenticated


# 2. Books

## GET /api/books

Purpose:
- Get all books.

Access:
- Admin/Librarian

Optional Filters:
- search by title
- search by author
- availability

Success:
- Returns a list of books.


## GET /api/books/:id

Purpose:
- Get one book by ID.

Access:
- Admin/Librarian

Success:
- Returns the requested book.

Possible Errors:
- 404 — Book not found


## POST /api/books

Purpose:
- Add a new book.

Access:
- Admin/Librarian

Input:
- title
- author
- isbn

Business Rules:
- Title is required.
- Author is required.
- ISBN is required.
- ISBN must be unique.
- New book is available by default.

Success:
- Returns the created book.

Possible Errors:
- 400 — Invalid or missing data
- 409 — ISBN already exists


## PUT /api/books/:id

Purpose:
- Update an existing book.

Access:
- Admin/Librarian

Possible Input:
- title
- author
- isbn

Business Rules:
- Book must exist.
- ISBN must remain unique.

Success:
- Returns the updated book.

Possible Errors:
- 400 — Invalid data
- 404 — Book not found
- 409 — ISBN already exists


## DELETE /api/books/:id

Purpose:
- Delete a book.

Access:
- Admin/Librarian

Business Rules:
- Book must exist.
- A book with an active loan should not be deleted.

Success:
- Confirms that the book was deleted.

Possible Errors:
- 404 — Book not found
- 409 — Book currently has an active loan


# 3. Members

## GET /api/members

Purpose:
- Get all library members.

Access:
- Admin/Librarian

Optional Filters:
- search by name
- search by email

Success:
- Returns a list of members.


## GET /api/members/:id

Purpose:
- Get one member.

Access:
- Admin/Librarian

Success:
- Returns member information.

Possible Errors:
- 404 — Member not found


## POST /api/members

Purpose:
- Register a new library member.

Access:
- Admin/Librarian

Input:
- name
- email
- phone (optional)

Business Rules:
- Name is required.
- Email is required.
- Email must be unique.

Success:
- Returns the created member.

Possible Errors:
- 400 — Invalid or missing data
- 409 — Email already exists


## PUT /api/members/:id

Purpose:
- Update member information.

Access:
- Admin/Librarian

Possible Input:
- name
- email
- phone

Business Rules:
- Member must exist.
- Email must remain unique.

Success:
- Returns the updated member.

Possible Errors:
- 400 — Invalid data
- 404 — Member not found
- 409 — Email already exists


# 4. Loans

## GET /api/loans

Purpose:
- Get loan history.

Access:
- Admin/Librarian

Success:
- Returns all loans including returned and active loans.


## GET /api/loans/active

Purpose:
- Get books that are currently borrowed.

Access:
- Admin/Librarian

Business Rule:
- Active loan means returned_at is empty.

Success:
- Returns active loans.


## GET /api/loans/:id

Purpose:
- Get one loan.

Access:
- Admin/Librarian

Success:
- Returns loan information together with its member and book.

Possible Errors:
- 404 — Loan not found


## POST /api/loans

Purpose:
- Borrow a book.

Access:
- Admin/Librarian

Input:
- book_id
- member_id
- due_at

Business Rules:
- Member must exist.
- Book must exist.
- Book must currently be available.
- Book cannot have another active loan.
- A new loan must be created.
- Book becomes unavailable.

Important:
- Creating the loan and changing book availability should happen together.

Success:
- Returns the created loan.

Possible Errors:
- 400 — Invalid data
- 404 — Member or book not found
- 409 — Book is already borrowed


## PUT /api/loans/:id/return

Purpose:
- Return a borrowed book.

Access:
- Admin/Librarian

Business Rules:
- Loan must exist.
- Loan must still be active.
- Set returned_at.
- Book becomes available again.

Important:
- Updating the loan and book should happen together.

Success:
- Returns the completed loan.

Possible Errors:
- 404 — Loan not found
- 409 — Book has already been returned


# 5. Dashboard

## GET /api/dashboard

Purpose:
- Return summary information for the frontend dashboard.

Access:
- Admin/Librarian

Expected Data:
- total books
- available books
- borrowed books
- total members
- active loans
- overdue loans


# 6. General API Rules

Authentication:
- Protected endpoints require a logged-in Admin/Librarian.

Validation:
- Validate incoming request data before database operations.

Errors:
- API should return clear and consistent error responses.

Database:
- PostgreSQL is the source of truth.

Transactions:
- Borrowing and returning books should use database transactions.

Redis:
- Redis caching will be added later to read-heavy endpoints.

Socket.IO:
- Real-time notifications will be added later.

Version 1 Scope:
- One library only.
- No payments.
- No fines.
- No email notifications.
- Members do not log in during the MVP.