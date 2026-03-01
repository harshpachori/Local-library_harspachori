Project Local Library
README — Detailed Project Documentation

1. Project Plan
   
This section describes the functions, features, algorithms, and key coding decisions made across the three source files: accounts.js, books.js, and home.js. Each file contains functions that operate on shared datasets representing library accounts, authors, and books.

1.1 Functions Implemented

The following eight functions were written to complete the project. Each function is described along with the method or feature it relies on.
•	findAccountById() in accounts.js accepts an array of account objects and a string ID, and returns the single matching account object. It uses the find() native array method to locate the first element whose id property matches the provided ID.
•	sortAccountsByLastName() in accounts.js accepts an array of account objects and returns them sorted alphabetically by last name. It uses sort() with a ternary comparator that compares lowercased last name strings to avoid case-sensitivity issues.
•	getAccountFullNames() in accounts.js accepts an array of account objects and returns an array of full name strings in the format 'First Last'. It uses map() combined with nested object destructuring to extract first and last names, joined with a template literal.
•	findAuthorById() in books.js accepts an array of author objects and an integer ID, and returns the matching author. It uses find() and is careful to match against integer IDs, since author IDs are numbers rather than strings.
•	findBookById() in books.js accepts an array of book objects and a string ID, and returns the matching book object. It also uses find() and follows the same pattern as findAccountById().
•	getTotalBooksCount() in home.js accepts an array of book objects and returns the total count. It uses the .length property directly on the array.
•	getTotalAccountsCount() in home.js accepts an array of account objects and returns the total count using .length.
•	getBooksBorrowedCount() in home.js accepts an array of books and returns the number of books currently checked out. It uses filter() to select books where the first element of the borrows array has a returned value of false, then returns the .length of the filtered result.

1.2 Key Features and Algorithms

Several JavaScript features were intentionally used across the implemented functions to satisfy the project's coding requirements and to write clean, modern code.
•	Arrow functions are used throughout every function as callback arguments to find(), filter(), map(), and sort(). This keeps the syntax concise and semantically clear.
•	The find() native array method is used in findAccountById(), findAuthorById(), and findBookById() because it short-circuits on the first match, making it more semantically appropriate than filter() when only one result is expected.
•	The filter() native array method is used in getBooksBorrowedCount() to produce a subset of books that match a condition, after which .length provides the count.
•	The ternary operator is used in the sort() comparator inside sortAccountsByLastName() to return 1, -1, or 0 based on string comparison. A subtraction approach would not work here since last names are strings, not numbers.
•	Object destructuring is used in getAccountFullNames() to unpack nested name properties directly in the function parameter, removing the need for intermediate variables.
•	Template literals are used in getAccountFullNames() to construct the 'First Last' string format cleanly.

2. Implementation Plan

The implementation followed a structured, incremental approach divided into three phases. Each phase built on the previous one and was validated before moving forward.
2.1 Phase 1 — Environment Setup

Before writing any code, the development environment was confirmed and the project was oriented.
•	The Node version was verified using node -v and switched to v18 with nvm use v18 to match the project's requirements.
•	Dependencies were installed with npm install to set up mocha and chai for testing.
•	npm test was run immediately to review the initial failing test output. This established which functions were expected, what arguments they accepted, and what return shapes were required.
•	npm start was run to open the live site preview and observe how the functions would connect to the rendered dashboard UI.

2.2 Phase 2 — Implementation Order

Functions were implemented in order of increasing complexity, starting with the simplest to build confidence and establish patterns before tackling more nuanced logic.
•	findAccountById(), findBookById(), and findAuthorById() were written first as straightforward applications of find().
•	getTotalBooksCount() and getTotalAccountsCount() were completed next as single-line .length returns.
•	getAccountFullNames() followed, introducing map() and destructuring for the first time in the project.
•	sortAccountsByLastName() was written next, requiring careful thought about the sort comparator and case normalization.
•	getBooksBorrowedCount() was completed last, as it required understanding the borrows[0].returned convention established by the pre-written partitionBooksByBorrowedStatus() function.

2.3 Phase 3 — Verification

After each function was written, it was verified before moving on to the next.
•	npm test was run after each individual function to confirm the specific test passed without breaking previously passing tests.
•	The live site was checked via npm start to visually confirm that dashboard statistics rendered correctly after each implementation.
•	The pre-written functions — getTotalNumberOfBorrows(), getBooksPossessedByAccount(), partitionBooksByBorrowedStatus(), and getBorrowersForBook() — were read carefully as reference implementations to align style and approach.

3. Example Navigation Structure

The application consists of three pages. An acceptable navigation structure for this library dashboard would present each page as a distinct section of the site, accessible from a shared top-level navigation bar. A user would encounter the pages in the following natural order.
•	The Home page (public/index.html) serves as the dashboard landing page. It displays high-level statistics including the total number of books in the library, the total number of registered accounts, the number of books currently checked out, the five most common genres, the five most popular books by borrow count, and the five most popular authors by total borrows. This page gives administrators a quick overview of library activity.
•	The Books page (public/books.html) presents the full catalog of books. Books are separated into two groups — those currently checked out and those that have been returned. Each book entry shows its title, genre, author, and a list of up to ten borrowers along with whether each borrower has returned the book. This allows staff to see at a glance which books are available and who has previously borrowed each title.
•	The Accounts page (public/accounts.html) lists all registered library members, sorted alphabetically by last name. Each account displays the patron's full name and the books they currently have checked out, including the author of each book. This page allows staff to look up a patron quickly and see their borrowing activity.
Together, these three pages cover all major administrative needs: system-wide statistics, book-level detail, and patron-level detail. The navigation bar linking them allows staff to move between views without losing context.

4. Coding Trade-offs

Several decisions involved choosing between two reasonable approaches. The trade-offs below reflect deliberate choices rather than oversights.

4.1 Readability versus Conciseness
Arrow functions and chained array methods improve conciseness but can be harder to read for developers who are less familiar with functional programming patterns. For example, filter().length is compact but less explicit than a for loop with a counter. The decision was made to favour conciseness throughout, since the codebase is small, the patterns are standard JavaScript idioms, and the project's learning objectives explicitly call for the use of these native methods.
4.2 Mutating sort() versus Copying First
The native sort() method mutates the original array in place. In sortAccountsByLastName(), this means the input array is reordered directly rather than a copy being returned. A safer approach would be to spread the array first — for example, [...accounts].sort(...) — and return the new array, leaving the original intact. However, since the project specification does not require immutability and the tests pass with the direct sort, the simpler approach was retained. This is a trade-off worth reconsidering in a production codebase where the original data order may matter elsewhere.
4.3 Relying on borrows[0] as the Current Status
getBooksBorrowedCount() assumes that borrows[0] always represents the most recent transaction, and therefore that its returned value accurately reflects whether the book is currently checked out. This convention is consistent with the pre-written partitionBooksByBorrowedStatus() function, which uses the same assumption. However, the approach is brittle — if the borrows array were ever stored in a different order, this logic would produce incorrect results. A more defensive implementation would sort by a timestamp, but the dataset does not include one. The convention-based approach was kept for consistency with the rest of the codebase.
4.4 Native Methods over Loops
All implemented functions use native array methods rather than explicit for or while loops. This aligns with the project's stated learning objectives and results in more declarative, expressive code. The minor trade-off is that deeply chained method calls can be harder to step through in a debugger than a simple loop. For this project's scale, that trade-off is not a concern.

5. Justification, Challenges, and Debugging

5.1 Justification of Key Choices
•	find() was chosen over filter()[0] for single-item lookups because it short-circuits on the first match, making intent clear — the caller expects exactly one result. Using filter() and taking index 0 would imply that multiple matches are possible, which is misleading.
•	Nested destructuring in getAccountFullNames() — written as ({ name: { first, last } }) — was chosen to demonstrate a modern JavaScript feature explicitly and to avoid creating unnecessary intermediate variables inside the function body.
•	The ternary comparator in sort() — returning 1, -1, or 0 — was chosen over a subtraction approach because string comparison cannot use arithmetic. Subtracting two strings returns NaN, which causes sort() to produce unpredictable results.
•	toLowerCase() was applied to both sides of the sort comparator to ensure that capitalisation differences do not affect alphabetical ordering. Without it, uppercase letters sort before lowercase ones due to ASCII character codes, which would cause names like 'adams' to appear after 'Zimmerman'.
5.2 Challenges Encountered
•	Understanding the borrows[0].returned convention required careful reading of the pre-written partitionBooksByBorrowedStatus() function, since the docs described the expected output but did not explicitly state how the returned field should be interpreted.
•	Recognising that author IDs are integers rather than strings — unlike account and book IDs — required attention when writing findAuthorById(). A type mismatch between the integer authorId stored in a book and a string passed to find() would cause the function to silently return undefined, which is difficult to debug since it produces no error.
5.3 Debugging Moments
•	During initial testing of sortAccountsByLastName(), names were not sorting correctly. The root cause was the absence of .toLowerCase() on the comparison strings. After adding case normalisation, the sort order matched the expected test output.
•	Running npm test after each individual function — rather than all at once at the end — made it straightforward to isolate failures to specific functions and resolve them before introducing additional complexity.

6. AI Tools Used

The following AI tool was used during this project, with justification for how and why it was used.
•	Claude by Anthropic was used as the primary AI assistant throughout the project. It was used to implement and review all eight required functions, confirm that the code satisfied the project's style requirements — including no single-letter variables, correct use of arrow functions, destructuring, the ternary operator, and the find() and filter() methods — and to generate this README documentation in a structured Word document format.
•	Claude was also used to cross-check the trade-off analysis and debugging narrative for completeness and accuracy, and to produce a formatted .docx version of the README using the docx npm library.
In all cases, AI assistance was used as a coding and documentation partner rather than as a replacement for understanding the material. Each function was reviewed, understood, and validated manually via the test suite before being considered complete. The AI did not run the tests or interact with the live site — those steps were performed independently to confirm correctness.

7. Project Summary
   
•	The Local Library project involved building eight JavaScript functions across three files — accounts.js, books.js, and home.js — to power a neighbourhood library dashboard. The functions handle searching, sorting, filtering, and counting operations across interrelated datasets of books, accounts, and authors.
•	Every function was implemented using modern, declarative JavaScript patterns including find(), filter(), map(), sort(), arrow functions, template literals, and destructuring. All functions were validated with the provided mocha and chai test suite and confirmed visually on the live preview site.
•	The project reinforced that choosing the right native array method for each task — and understanding the data conventions that underpin the broader codebase — produces code that is both readable and easy to debug, even when working across multiple linked datasets.
<img width="468" height="630" alt="image" src="https://github.com/user-attachments/assets/bb5a21cf-dd6e-4fb8-949a-912831bb67bd" />
