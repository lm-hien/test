```mermaid
sequenceDiagram
    actor User
    participant LibrarySystem
    database LibraryDB

    User ->> LibrarySystem: Request to Borrow Book (BookID, UserID)
    LibrarySystem ->> LibraryDB: Validate User (membership, fines, limits)
    LibraryDB -->> LibrarySystem: User Status (valid/invalid)

    alt User not eligible
      LibrarySystem -->> User: Borrow Request Rejected (reason)
    else User eligible
      LibrarySystem ->> LibraryDB: Check Book Availability (BookID)
      LibraryDB -->> LibrarySystem: Book Status (Available/Not Available)

      alt Book not available
        LibrarySystem -->> User: Borrow Request Rejected (book unavailable)
      else Book available
        LibrarySystem ->> LibraryDB: Create Borrow Record (TransactionID, BorrowDate, DueDate)
        LibrarySystem ->> LibraryDB: Update Book Status to "Borrowed"
        LibrarySystem -->> User: Borrow Confirmation (BookID, DueDate, TransactionID)
      end
    end
```
