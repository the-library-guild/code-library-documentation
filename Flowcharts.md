# Flowcharts

# Book Life-cycle

```mermaid
graph LR
	subgraph New
		StateBuy[<b>Buy new Book]
	end
	subgraph Shelf
		StateAvailable[<b>Available]
	end
	subgraph User
		StateAvailable --> Borrowing(Borrowing) --> StateBorrowed
		StateBorrowed[<b>Borrowed]
		StateBorrowed --> Returning(Returning)
	end
	subgraph Librarian
		StateBuy -.-> Adding(Adding new Book) -.-> StateProcessing
		Returning(Returning) --> StateProcessing[<b>Processing]
		StateProcessing --> Sorting(Sorting) --> StateAvailable
	end
	StateAvailable -.Stolen.-> StateMissing[<b>Missing]
	StateBorrowed -.Never Returned.-> StateMissing
	Sorting -.Damaged or marked as returned but not in book box.-> StateMissing
```

# User Website

```mermaid
graph TD
    ScanCode(Scan QR-Code on Book) -->|Opens Website| BookPage[<b>Book Page</b><br/>Title<br/>Author<br/>]
    BookPage --> State{Book State}
    State --> StateAvailable[Available] & StateRented[Rented] & StateProcessing[Processing]
    StateAvailable --> BRent(Button: Rent)
    subgraph Rent
        BRent --> IsSignedIn{Is user signed in?}
        IsSignedIn-- No -->  SignInPage[<b>SignIn Page</b><br/>Sign in using CODE google account.]
        IsSignedIn-- Yes --> BookRented
        SignInPage --> isSignInSuccessful{isSuccessful?}
        isSignInSuccessful-- Yes --> BookRented
				BookRented[Rented] --> SuccessPageRent[<b>Success Page</b><br/>Book was successfully rented.]
        isSignInSuccessful-- No --> ErrorPage[<b>Error Page</b>]
    end
		BookRented -. Change Book Status .-> Database

    StateRented --> BReturn(Button: Return)
    subgraph Return
        BReturn --> ReturnPage[<b>Return Page</b><br/>Please put the book in the return box.]
        ReturnPage --> Bok(Button: Ok) --> SuccessPageRteurn[<b>Success Page</b><br/>Book was successfully returned.]
    end
		Bok -. Change Book Status .-> Database

		StateProcessing --> isLibrarian
		subgraph Processing
			isLibrarian{is user librarian?}
			isLibrarian -- Yes --> ButtonProccessed(Button: Proccessed)
			ButtonProccessed --> SuccessPageProccessed[<b>Success Page</b><br/>Book is available again]
		end
		ButtonProccessed -. Change Book Status .-> Database

		Database[(Database)]
```

# Librarian Website

```mermaid
graph TD
    StartPage[<b>Start Page<b>] --> IsSignedIn
    subgraph Sign In
        IsSignedIn{is user signed in?}
        IsSignedIn -- No --> SignInPage[<b>SignIn Page</b>]
        SignInPage --> isSuccess{is SignIn successful?}
        isSuccess -- No --> ErrorPage[<b>Error Page</b>]
				IsSignedIn -- Yes --> isLibrarian{is user a librarian?}
				isSuccess -- Yes --> isLibrarian{is user a librarian?}
				isLibrarian -- No -->ErrorPage
    end
		isLibrarian -- Yes --> HomePage
    HomePage[<b>Home Page</b>]
		Database[(Database)]

    HomePage --> ButtonManageBooks(Button: Manage Books) -->ManageBooksPage
    subgraph Manage Books
        ManageBooksPage[<b>Manage Books Page</b><br>List of all books.]
        ManageBooksPage --> ButtonAddBook(Button: Add Book) --> AddBookPage[<b>Add Book Page<b><br/>Enter Information]
        AddBookPage --> ButtonAdd(Button: Add) --> ManageBooksPage
        ManageBooksPage --> ButtonEditBook(Button: Edit Book) --> EditBookPage[<b>Edit Book Page</b><br/>Title<br/>Author]
        EditBookPage --> ButtonSave(Button: Save) --> ManageBooksPage
        ManageBooksPage --> ButtonRemoveBook(Button: Remove Book) --> ManageBooksPage

				ManageBooksPage --> ButtonProcessed(Button: Processed) --> ManageBooksPage
    end
    ButtonAdd -. Insert Book .-> Database
    ButtonSave -. Update Book .-> Database
    ButtonRemoveBook -. Delete Book .-> Database
		ButtonProcessed -. Update Book Status = Available .-> Database

    HomePage --> ButtonShelveCheck(Button: Shelve Check) --> ShelveCheckPage
    subgraph Shelve Check
        ShelveCheckPage[<b>Shelve Check Page</b>]
    end
```

- last returned
- last borrowed book
- how many books need to be processed
- books that are overdue
- extend the borrowing time of a book

# Server

```mermaid
graph TD
	RegularChecks[<b>Regular Checks<b>]
	RegularChecks --> isBookDueInADay{is book due in one day?}
	isBookDueInADay -- Yes --> notifyUser(Notify user)
	RegularChecks --> isBookOverdue{is book overdue?}
	isBookOverdue -- Yes --> blockUser(Block user from renting new books)
	isBookOverdue -- Yes --> notifyLibrarian(Notify librarian)
	RegularChecks --> newBookReturn{is there a new book return?}
	newBookReturn -- Yes --> notifyLibrarian(Notify librarian)
```