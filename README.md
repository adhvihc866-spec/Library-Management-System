# Library-Management-System
A   C programming code on Library Management System

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_BOOKS 100

// Defining the Book structure
struct Book {
    int id;
    char title[100];
    char author[50];
    float price;
};

// Global array to store books and a counter to keep track of total books
struct Book library[MAX_BOOKS];
int bookCount = 0;

// Function declarations
void addBook();
void displayBooks();
void searchBookByTitle();
void totalBooksCount();

int main() {
    int choice;

    while (1) {
        printf("\n=================================");
        printf("\n    LIBRARY MANAGEMENT SYSTEM    ");
        printf("\n=================================");
        printf("\n1. Add New Book Information");
        printf("\n2. Display All Books Detailed Records");
        printf("\n3. Search Book by Title");
        printf("\n4. Display Total Number of Books");
        printf("\n5. Exit System");
        printf("\n=================================");
        printf("\nEnter your choice (1-5): ");
        
        if (scanf("%d", &choice) != 1) {
            printf("\nInvalid input. Please enter a valid number.\n");
            while (getchar() != '\n'); // Clear input buffer
            continue;
        }
        
        getchar(); // Clean the newline character left in the buffer by scanf

        switch (choice) {
            case 1:
                addBook();
                break;
            case 2:
                displayBooks();
                break;
            case 3:
                searchBookByTitle();
                break;
            case 4:
                totalBooksCount();
                break;
            case 5:
                printf("\nExiting the system. Thank you!\n");
                exit(0);
            default:
                printf("\nInvalid selection! Please choose an option between 1 and 5.\n");
        }
    }
    return 0;
}

// Function to add a book to the library catalogue
void addBook() {
    if (bookCount >= MAX_BOOKS) {
        printf("\nError: Library catalog capacity reached (%d books maximum).\n", MAX_BOOKS);
        return;
    }

    printf("\n--- Add Book Details ---");
    printf("\nEnter Book ID (Integer): ");
    scanf("%d", &library[bookCount].id);
    getchar(); // Clear trailing newline from buffer

    printf("Enter Book Title: ");
    fgets(library[bookCount].title, sizeof(library[bookCount].title), stdin);
    library[bookCount].title[strcspn(library[bookCount].title, "\n")] = 0; // Strip newline character

    printf("Enter Author Name: ");
    fgets(library[bookCount].author, sizeof(library[bookCount].author), stdin);
    library[bookCount].author[strcspn(library[bookCount].author, "\n")] = 0; // Strip newline character

    printf("Enter Price: ");
    scanf("%f", &library[bookCount].price);

    bookCount++;
    printf("\nSuccess: Book recorded successfully!\n");
}

// Function to view all records currently stored in memory
void displayBooks() {
    if (bookCount == 0) {
        printf("\nNotification: The library inventory is currently empty.\n");
        return;
    }

    printf("\n----------------------------------------------------------------------------");
    printf("\n%-10s %-30s %-25s %-10s", "ID", "Title", "Author", "Price");
    printf("\n----------------------------------------------------------------------------");
    for (int i = 0; i < bookCount; i++) {
        printf("\n%-10d %-30s %-25s $%-9.2f", 
               library[i].id, library[i].title, library[i].author, library[i].price);
    }
    printf("\n----------------------------------------------------------------------------\n");
}

// Function to lookup details of a specific book by its title string
void searchBookByTitle() {
    if (bookCount == 0) {
        printf("\nNotification: The library inventory is currently empty.\n");
        return;
    }

    char searchTitle[100];
    int found = 0;

    printf("\nEnter the exact Title of the book to search: ");
    fgets(searchTitle, sizeof(searchTitle), stdin);
    searchTitle[strcspn(searchTitle, "\n")] = 0; // Strip newline character

    for (int i = 0; i < bookCount; i++) {
        // Case-sensitive exact matching
        if (strcmp(library[i].title, searchTitle) == 0) {
            if (!found) {
                printf("\n--- Matching Record Found ---");
                printf("\n%-10s %-30s %-25s %-10s", "ID", "Title", "Author", "Price");
                printf("\n----------------------------------------------------------------------------");
            }
            printf("\n%-10d %-30s %-25s $%-9.2f", 
                   library[i].id, library[i].title, library[i].author, library[i].price);
            found = 1;
        }
    }
    
    if (found) {
        printf("\n----------------------------------------------------------------------------\n");
    } else {
        printf("\nResult: No book matching the title \"%s\" was found.\n", searchTitle);
    }
}

// Function to print the absolute size of the catalog array
void totalBooksCount() {
    printf("\nTotal inventory tracking: There are currently %d book(s) registered in the system.\n", bookCount);
}
