#include <iostream>
#include <vector>

class TicTacToe {
private:
    std::vector<std::vector<char>> board;
    char currentPlayer;

public:
    TicTacToe() : board(3, std::vector<char>(3, ' ')), currentPlayer('X') {}

    void displayBoard() const {
        std::cout << "\nCurrent Board:\n";
        for (int row = 0; row < 3; ++row) {
            for (int col = 0; col < 3; ++col) {
                std::cout << " " << board[row][col] << " ";
                if (col < 2) std::cout << "|";
            }
            std::cout << std::endl;
            if (row < 2) std::cout << "---|---|---\n";
        }
        std::cout << std::endl;
    }

    bool isValidMove(int row, int col) const {
        return row >= 0 && row < 3 && col >= 0 && col < 3 && board[row][col] == ' ';
    }

    void makeMove(int row, int col) {
        board[row][col] = currentPlayer;
    }

    bool checkWin() const {
        
        for (int row = 0; row < 3; ++row) {
            if (board[row][0] == currentPlayer && board[row][1] == currentPlayer && board[row][2] == currentPlayer) {
                return true;
            }
        }
        
        for (int col = 0; col < 3; ++col) {
            if (board[0][col] == currentPlayer && board[1][col] == currentPlayer && board[2][col] == currentPlayer) {
                return true;
            }
        }
        // Check diagonals
        if (board[0][0] == currentPlayer && board[1][1] == currentPlayer && board[2][2] == currentPlayer) {
            return true;
        }
        if (board[0][2] == currentPlayer && board[1][1] == currentPlayer && board[2][0] == currentPlayer) {
            return true;
        }
        return false;
    }

    bool checkDraw() const {
        for (const auto& row : board) {
            for (char cell : row) {
                if (cell == ' ') {
                    return false;
                }
            }
        }
        return true;
    }

    void switchPlayer() {
        currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
    }

    void resetBoard() {
        board = std::vector<std::vector<char>>(3, std::vector<char>(3, ' '));
    }

    char getCurrentPlayer() const {
        return currentPlayer;
    }
};

int main() {
    TicTacToe game;
    int row, col;
    char playAgain;

    do {
        game.resetBoard();
        bool gameWon = false;
        bool gameDraw = false;

        while (true) {
            game.displayBoard();
            std::cout << "Player " << game.getCurrentPlayer() << ", enter your move (row and column): ";
            std::cin >> row >> col;

            while (!game.isValidMove(row - 1, col - 1)) {
                std::cout << "Invalid move. Try again: ";
                std::cin >> row >> col;
            }

            game.makeMove(row - 1, col - 1);

            if (game.checkWin()) {
                game.displayBoard();
                std::cout << "Player " << game.getCurrentPlayer() << " wins!\n";
                gameWon = true;
                break;
            }

            if (game.checkDraw()) {
                game.displayBoard();
                std::cout << "The game is a draw.\n";
                gameDraw = true;
                break;
            }

            game.switchPlayer();
        }

        std::cout << "Do you want to play again? (y/n): ";
        std::cin >> playAgain;

    } while (playAgain == 'y' || playAgain == 'Y');

    return 0;
}


https://www.linkedin.com/posts/mariem-wael-308883266_codsoft-activity-7206588558302691330-U2q8?utm_source=share&utm_medium=member_android
