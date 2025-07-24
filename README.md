# c-program-proj-3#include <iostream>
using namespace std;

char board[9]; // 1D array for the 3x3 board
char currentPlayer;

// Initialize the board with numbers 1–9
void initializeBoard() {
    for (int i = 0; i < 9; i++)
        board[i] = '1' + i;
}

// Display the board
void displayBoard() {
    cout << "\n";
    for (int i = 0; i < 9; i++) {
        cout << " " << board[i] << " ";
        if ((i + 1) % 3 == 0) {
            cout << "\n";
            if (i < 8) cout << "---+---+---\n";
        } else {
            cout << "|";
        }
    }
    cout << "\n";
}

// Switch current player
void switchPlayer() {
    currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
}

// Check if the player has won
bool checkWin() {
    int wins[8][3] = {
        {0, 1, 2}, {3, 4, 5}, {6, 7, 8}, // rows
        {0, 3, 6}, {1, 4, 7}, {2, 5, 8}, // cols
        {0, 4, 8}, {2, 4, 6}             // diagonals
    };
    for (auto &w : wins) {
        if (board[w[0]] == currentPlayer &&
            board[w[1]] == currentPlayer &&
            board[w[2]] == currentPlayer)
            return true;
    }
    return false;
}

// Check if the board is full
bool isDraw() {
    for (int i = 0; i < 9; i++)
        if (board[i] != 'X' && board[i] != 'O')
            return false;
    return true;
}

// Handle a player's move
void playerMove() {
    int choice;
    while (true) {
        cout << "Player " << currentPlayer << ", choose a cell (1–9): ";
        cin >> choice;
        if (choice >= 1 && choice <= 9 && board[choice - 1] != 'X' && board[choice - 1] != 'O') {
            board[choice - 1] = currentPlayer;
            break;
        } else {
            cout << "Invalid move. Try again.\n";
        }
    }
}

void playGame() {
    initializeBoard();
    currentPlayer = 'X';

    while (true) {
        displayBoard();
        playerMove();

        if (checkWin()) {
            displayBoard();
            cout << "Player " << currentPlayer << " wins!\n";
            break;
        }

        if (isDraw()) {
            displayBoard();
            cout << "It's a draw!\n";
            break;
        }

        switchPlayer();
    }
}

int main() {
    char playAgain;

    do {
        playGame();
        cout << "Play again? (y/n): ";
        cin >> playAgain;
    } while (playAgain == 'y' || playAgain == 'Y');

    cout << "Thanks for playing!\n";
    return 0;
}
