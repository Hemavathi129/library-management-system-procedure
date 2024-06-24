# library-management-system-procedure

CREATE PROC dbo.LibraryManagementSystemProcedure
AS
BEGIN
    -- Create database if it does not exist
    IF NOT EXISTS (SELECT * FROM sys.databases WHERE name = 'db_LibraryManagement')
    BEGIN
        CREATE DATABASE db_LibraryManagement;
        PRINT 'Database db_LibraryManagement created.';
    END
    ELSE
    BEGIN
        PRINT 'Database db_LibraryManagement already exists.';
    END

    -- Use the created database
    USE db_LibraryManagement;

    -- Create tbl_publisher table
    IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'tbl_publisher')
    BEGIN
        CREATE TABLE tbl_publisher (
            publisher_PublisherID INT PRIMARY KEY NOT NULL IDENTITY,
            publisher_PublisherName VARCHAR(100) NOT NULL,
            publisher_PublisherAddress VARCHAR(200) NOT NULL,
            publisher_PublisherPhone VARCHAR(50) NOT NULL
        );
        PRINT 'Table tbl_publisher created.';
    END
    ELSE
    BEGIN
        PRINT 'Table tbl_publisher already exists.';
    END

    -- Insert data into tbl_publisher
    INSERT INTO tbl_publisher (publisher_PublisherName, publisher_PublisherAddress, publisher_PublisherPhone)
    VALUES
        ('DAW Books', '375 Hudson Street, New York, NY 10014', '212-366-2000'),
        ('Viking', '375 Hudson Street, New York, NY 10014', '212-366-2000'),
        ('Signet Books', '375 Hudson Street, New York, NY 10014', '212-366-2000'),
        ('Chilton Books', 'Not Available', 'Not Available'),
        ('George Allen & Unwin', '83 Alexander Ln, Crows Nest NSW 2065, Australia', '+61-2-8425-0100'),
        ('Alfred A. Knopf', 'The Knopf Doubleday Group Domestic Rights, 1745 Broadway, New York, NY 10019', '212-940-7390'),
        ('Bloomsbury', 'Bloomsbury Publishing Inc., 1385 Broadway, 5th Floor, New York, NY 10018', '212-419-5300'),
        ('Shinchosa', 'Oga Bldg. 8, 2-5-4 Sarugaku-cho, Chiyoda-ku, Tokyo 101-0064 Japan', '+81-3-5577-6507'),
        ('Harper and Row', 'HarperCollins Publishers, 195 Broadway, New York, NY 10007', '212-207-7000'),
        ('Pan Books', '175 Fifth Avenue, New York, NY 10010', '646-307-5745'),
        ('Chalto & Windus', '375 Hudson Street, New York, NY 10014', '212-366-2000'),
        ('Harcourt Brace Jovanovich', '3 Park Ave, New York, NY 10016', '212-420-5800'),
        ('W.W. Norton', 'W. W. Norton & Company, Inc., 500 Fifth Avenue, New York, New York 10110', '212-354-5500'),
        ('Scholastic', '557 Broadway, New York, NY 10012', '800-724-6527'),
        ('Bantam', '375 Hudson Street, New York, NY 10014', '212-366-2000'),
        ('Picador USA', '175 Fifth Avenue, New York, NY 10010', '646-307-5745');

    PRINT 'Data inserted into tbl_publisher.';

    -- Create tbl_book table
    IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'tbl_book')
    BEGIN
        CREATE TABLE tbl_book (
            book_BookID INT PRIMARY KEY NOT NULL IDENTITY,
            book_Title VARCHAR(100) NOT NULL,
            book_PublisherID INT NOT NULL,
            CONSTRAINT fk_publisher_id FOREIGN KEY (book_PublisherID) REFERENCES tbl_publisher(publisher_PublisherID) ON UPDATE CASCADE ON DELETE CASCADE
        );
        PRINT 'Table tbl_book created.';
    END
    ELSE
    BEGIN
        PRINT 'Table tbl_book already exists.';
    END

    -- Insert data into tbl_book
    INSERT INTO tbl_book (book_Title, book_PublisherID)
    VALUES
        ('The Name of the Wind', 1),
        ('It', 2),
        ('The Green Mile', 3),
        ('Dune', 4),
        ('The Hobbit', 5),
        ('Eragon', 6),
        ('A Wise Man''s Fear', 1),
        ('Harry Potter and the Philosopher''s Stone', 7),
        ('Hard-Boiled Wonderland and The End of the World', 8),
        ('The Giving Tree', 9),
        ('The Hitchhiker''s Guide to the Galaxy', 10),
        ('Brave New World', 11),
        ('The Princess Bride', 12),
        ('Fight Club', 13),
        ('Holes', 14),
        ('Harry Potter and the Chamber of Secrets', 7),
        ('Harry Potter and the Prisoner of Azkaban', 7),
        ('The Fellowship of the Ring', 5),
        ('A Game of Thrones', 15),
        ('The Lost Tribe', 16);

    PRINT 'Data inserted into tbl_book.';

    -- Create tbl_library_branch table
    IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'tbl_library_branch')
    BEGIN
        CREATE TABLE tbl_library_branch (
            library_branch_BranchID INT PRIMARY KEY NOT NULL IDENTITY,
            library_branch_BranchName VARCHAR(100) NOT NULL,
            library_branch_BranchAddress VARCHAR(200) NOT NULL
        );
        PRINT 'Table tbl_library_branch created.';
    END
    ELSE
    BEGIN
        PRINT 'Table tbl_library_branch already exists.';
    END

    -- Insert data into tbl_library_branch
    INSERT INTO tbl_library_branch (library_branch_BranchName, library_branch_BranchAddress)
    VALUES
        ('Sharpstown', '32 Corner Road, New York, NY 10012'),
        ('Central', '491 3rd Street, New York, NY 10014'),
        ('Saline', '40 State Street, Saline, MI 48176'),
        ('Ann Arbor', '101 South University, Ann Arbor, MI 48104');

    PRINT 'Data inserted into tbl_library_branch.';

    -- Create tbl_borrower table
    IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'tbl_borrower')
    BEGIN
        CREATE TABLE tbl_borrower (
            borrower_CardNo INT PRIMARY KEY NOT NULL IDENTITY(100,1),
            borrower_BorrowerName VARCHAR(100) NOT NULL,
            borrower_BorrowerAddress VARCHAR(200) NOT NULL,
            borrower_BorrowerPhone VARCHAR(50) NOT NULL
        );
        PRINT 'Table tbl_borrower created.';
    END
    ELSE
    BEGIN
        PRINT 'Table tbl_borrower already exists.';
    END

    -- Insert data into tbl_borrower
    INSERT INTO tbl_borrower (borrower_BorrowerName, borrower_BorrowerAddress, borrower_BorrowerPhone)
    VALUES
        ('Joe Smith', '1321 4th Street, New York, NY 10014', '212-312-1234'),
        ('Jane Smith', '1321 4th Street, New York, NY 10014', '212-931-4124'),
        ('Tom Li', '981 Main Street, Ann Arbor, MI 48104', '734-902-7455'),
        ('Angela Thompson', '2212 Green Avenue, Ann Arbor, MI 48104', '313-591-2122'),
        ('Harry Emnace', '121 Park Drive, Ann Arbor, MI 48104', '412-512-5522'),
        ('Tom Haverford', '23 75th Street, New York, NY 10014', '212-631-3418'),
        ('Haley Jackson', '231 52nd Avenue New York, NY 10014', '212-419-9935'),
        ('Michael Horford', '653 Glen Avenue, Ann Arbor, MI 48104', '734-998-1513');

    PRINT 'Data inserted into tbl_borrower.';

    -- Create tbl_book_loans table
    IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'tbl_book_loans')
    BEGIN
        CREATE TABLE tbl_book_loans (
            book_loans_LoansID INT PRIMARY KEY NOT NULL IDENTITY,
            book_loans_BookID INT NOT NULL,
            book_loans_BranchID INT NOT NULL,
            book_loans_CardNo INT NOT NULL,
            book_loans_DateOut DATE NOT NULL DEFAULT GETDATE(),
            book_loans_DueDate DATE NOT NULL,
            CONSTRAINT fk_book_id FOREIGN KEY (book_loans_BookID) REFERENCES tbl_book(book_BookID) ON UPDATE CASCADE ON DELETE CASCADE,
            CONSTRAINT fk_branch_id FOREIGN KEY (book_loans_BranchID) REFERENCES tbl_library_branch(library_branch_BranchID) ON UPDATE CASCADE ON DELETE CASCADE,
            CONSTRAINT fk_cardno FOREIGN KEY (book_loans_CardNo) REFERENCES tbl_borrower(borrower_CardNo) ON UPDATE CASCADE ON DELETE CASCADE
        );
        PRINT 'Table tbl_book_loans created.';
    END
    ELSE
    BEGIN
        PRINT 'Table tbl_book_loans already exists.';
    END
