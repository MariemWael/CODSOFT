#include <iostream>
#include <vector>
#include <string>
#include <ctime>
#include <unordered_map>

class Book {
public:
    std::string title;
    std::string author;
    std::string isbn;
    bool isAvailable;

    Book(const std::string& t, const std::string& a, const std::string& i)
        : title(t), author(a), isbn(i), isAvailable(true) {}
};

class Borrower {
public:
    std::string name;
    int id;

    Borrower(const std::string& n, int i) : name(n), id(i) {}
};

class Transaction {
public:
    std::string isbn;
    int borrowerId;
    std::time_t checkoutDate;
    std::time_t returnDate;

    Transaction(const std::string& i, int bId)
        : isbn(i), borrowerId(bId), checkoutDate(std::time(nullptr)), returnDate(0) {}
};

class LibrarySystem {
private:
    std::vector<Book> books;
    std::vector<Borrower> borrowers;
    std::vector<Transaction> transactions;
    int nextBorrowerId;

public:
    LibrarySystem() : nextBorrowerId(1) {}

    void addBook(const std::string& title, const std::string& author, const std::string& isbn) {
        books.emplace_back(title, author, isbn);
        std::cout << "Book added successfully.\n";
    }

    void addBorrower(const std::string& name) {
        borrowers.emplace_back(name, nextBorrowerId++);
        std::cout << "Borrower added successfully.\n";
    }

    void searchBooks(const std::string& query, const std::string& type) {
        std::cout << "Search results:\n";
        for (const auto& book : books) {
            if ((type == "title" && book.title.find(query) != std::string::npos) ||
                (type == "author" && book.author.find(query) != std::string::npos) ||
                (type == "isbn" && book.isbn == query)) {
                std::cout << "Title: " << book.title << ", Author: " << book.author
                          << ", ISBN: " << book.isbn << ", Available: "
                          << (book.isAvailable ? "Yes" : "No") << "\n";
            }
        }
    }

    void checkoutBook(const std::string& isbn, int borrowerId) {
        for (auto& book : books) {
            if (book.isbn == isbn && book.isAvailable) {
                book.isAvailable = false;
                transactions.emplace_back(isbn, borrowerId);
                std::cout << "Book checked out successfully.\n";
                return;
            }
        }
        std::cout << "Book is not available or does not exist.\n";
    }

    void returnBook(const std::string& isbn) {
        for (auto& transaction : transactions) {
            if (transaction.isbn == isbn && transaction.returnDate == 0) {
                transaction.returnDate = std::time(nullptr);
                for (auto& book : books) {
                    if (book.isbn == isbn) {
                        book.isAvailable = true;
                        std::cout << "Book returned successfully.\n";
                        return;
                    }
                }
            }
        }
        std::cout << "Transaction not found.\n";
    }

    void calculateFine(int borrowerId) {
        const double finePerDay = 0.5; 
        const int allowedDays = 14; 
        std::time_t now = std::time(nullptr);

        for (const auto& transaction : transactions) {
            if (transaction.borrowerId == borrowerId && transaction.returnDate == 0) {
                double daysOverdue = std::difftime(now, transaction.checkoutDate) / (60 * 60 * 24) - allowedDays;
                if (daysOverdue > 0) {
                    std::cout << "Fine for ISBN " << transaction.isbn << ": $" << daysOverdue * finePerDay << "\n";
                } else {
                    std::cout << "No fine for ISBN " << transaction.isbn << "\n";
                }
            }
        }
    }

    void showMenu() {
        std::cout << "\nLibrary System Menu\n";
        std::cout << "1. Add Book\n";
        std::cout << "2. Add Borrower\n";
        std::cout << "3. Search Books\n";
        std::cout << "4. Checkout Book\n";
        std::cout << "5. Return Book\n";
        std::cout << "6. Calculate Fine\n";
        std::cout << "7. Exit\n";
        std::cout << "Enter your choice: ";
    }
};

int main() {
    LibrarySystem library;
    int choice;
    std::string title, author, isbn, name, query, type;
    int borrowerId;

    while (true) {
        library.showMenu();
        std::cin >> choice;

        switch (choice) {
            case 1:
                std::cin.ignore(); 
                std::cout << "Enter book title: ";
                std::getline(std::cin, title);
                std::cout << "Enter book author: ";
                std::getline(std::cin, author);
                std::cout << "Enter book ISBN: ";
                std::getline(std::cin, isbn);
                library.addBook(title, author, isbn);
                break;

            case 2:
                std::cin.ignore();
                std::cout << "Enter borrower name: ";
                std::getline(std::cin, name);
                library.addBorrower(name);
                break;

            case 3:
                std::cin.ignore();
                std::cout << "Enter search type (title/author/isbn): ";
                std::getline(std::cin, type);
                std::cout << "Enter search query: ";
                std::getline(std::cin, query);
                library.searchBooks(query, type);
                break;

            case 4:
                std::cin.ignore();
                std::cout << "Enter book ISBN: ";
                std::getline(std::cin, isbn);
                std::cout << "Enter borrower ID: ";
                std::cin >> borrowerId;
                library.checkoutBook(isbn, borrowerId);
                break;

            case 5:
                std::cin.ignore();
                std::cout << "Enter book ISBN: ";
                std::getline(std::cin, isbn);
                library.returnBook(isbn);
                break;

            case 6:
                std::cin.ignore();
                std::cout << "Enter borrower ID: ";
                std::cin >> borrowerId;
                library.calculateFine(borrowerId);
                break;

            case 7:
                std::cout << "Exiting the program.\n";
                return 0;

            default:
                std::cout << "Invalid choice. Please try again.\n";
        }
    }

    return 0;
}


https://www.linkedin.com/posts/mariem-wael-308883266_codsoft-activity-7206656604857925632-be6P?utm_source=share&utm_medium=member_android
