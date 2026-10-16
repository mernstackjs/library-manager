# Database Design

## User

Represents an Admin or Librarian who can log in and manage the system.

### Fields

id
- type: integer
- required: yes
- unique: yes
- primary key: yes

name
- type: string
- required: yes

email
- type: string
- required: yes
- unique: yes

password_hash
- type: string
- required: yes

role
- type: string
- required: yes
- default: librarian

created_at
- type: timestamp
- required: yes
- default: current time


## Member

Represents a library member who can borrow books.

### Fields

id
- type: integer
- required: yes
- unique: yes
- primary key: yes

name
- type: string
- required: yes

email
- type: string
- required: yes
- unique: yes

phone
- type: string
- required: no

created_at
- type: timestamp
- required: yes
- default: current time


## Book

Represents a book stored in the library.

### Fields

id
- type: integer
- required: yes
- unique: yes
- primary key: yes

title
- type: string
- required: yes

author
- type: string
- required: yes

isbn
- type: string
- required: yes
- unique: yes

available
- type: boolean
- required: yes
- default: true

created_at
- type: timestamp
- required: yes
- default: current time


## Loan

Represents a book borrowed by a member.

### Fields

id
- type: integer
- required: yes
- unique: yes
- primary key: yes

book_id
- type: integer
- required: yes
- foreign key: Book

member_id
- type: integer
- required: yes
- foreign key: Member

borrowed_at
- type: timestamp
- required: yes
- default: current time

due_at
- type: timestamp
- required: yes

returned_at
- type: timestamp
- required: no


## Relationships

Member
- One Member can have many Loans.
- Each Loan belongs to one Member.

Book
- One Book can have many Loans over time.
- Each Loan belongs to one Book.

User
- Users manage the Library Manager system.
- Users are separate from Members.


## Constraints

- User email must be unique.
- Member email must be unique.
- Book ISBN must be unique.
- Loan must reference an existing Book.
- Loan must reference an existing Member.
- Required fields cannot be empty.


## Business Rules

- A book can only have one active loan at a time.
- A book cannot be borrowed if it is already borrowed.
- A member must exist before a loan can be created.
- A book must exist before a loan can be created.
- Borrowing a book makes it unavailable.
- Returning a book makes it available again.
- returned_at is empty while a loan is active.
- returned_at is set when the book is returned.
- Loan history should not be deleted when a book is returned.