
#include <iostream>
#include <cstdlib>
#include <ctime>

int main() {

    std::srand(static_cast<unsigned int>(std::time(0)));

    int randomNumber = std::rand() % 100 + 1;
    int userGuess = 0;

    std::cout << "I have generated a random number between 1 and 100." << std::endl;
    std::cout << "Can you guess what it is?" << std::endl;

    while (userGuess != randomNumber) {
        std::cout << "Enter your guess: ";
        std::cin >> userGuess;

        if (userGuess < randomNumber) {
            std::cout << "Too low. Try again." << std::endl;
        } else if (userGuess > randomNumber) {
            std::cout << "Too high. Try again." << std::endl;
        } else {
            std::cout << "Congratulations! You guessed the correct number!" << std::endl;
        }
    }

    return 0;
}


https://www.linkedin.com/posts/mariem-wael-308883266_codsoft-activity-7206462411820707840-pZgI?utm_source=share&utm_medium=member_android
