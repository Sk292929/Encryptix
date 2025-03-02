#include <iostream>
#include <vector>

using namespace std;

// Function to display the board
void displayBoard(const vector<char>& board) {
    cout << " " << board[0] << " | " << board[1] << " | " << board[2] << endl;
    cout << "---+---+---" << endl;
    cout << " " << board[3] << " | " << board[4] << " | " << board[5] << endl;
    cout << "---+---+---" << endl;
    cout << " " << board[6] << " | " << board[7] << " | " << board[8] << endl;
}

// Function to check for a win
bool checkWin(const vector<char>& board, char player) {
    const int winPatterns[8][3] = {
        {0, 1, 2}, {3, 4, 5}, {6, 7, 8}, // Rows
        {0, 3, 6}, {1, 4, 7}, {2, 5, 8}, // Columns
        {0, 4, 8}, {2, 4, 6}             // Diagonals
    };

    for (const auto& pattern : winPatterns) {
        if (board[pattern[0]] == player && board[pattern[1]] == player && board[pattern[2]] == player) {
            return true;
        }
    }
    return false;
}

// Function to check for a draw
bool checkDraw(const vector<char>& board) {
    for (char cell : board) {
        if (cell == ' ') {
            return false;
        }
    }
    return true;
}

// Function to switch players
void switchPlayer(char& currentPlayer) {
    currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
}

int main() {
    while (true) {
        vector<char> board(9, ' ');
        char currentPlayer = 'X';
        bool game_over = false;

        while (!game_over) {
            displayBoard(board);

            // Ask the current player to make a move
            int move;
            cout << "Player " << currentPlayer << ", enter your move (1-9): ";
            cin >> move;
            move--; // Adjust for 0-based indexing

            if (move < 0 || move >= 9 || board[move] != ' ') {
                cout << "Invalid move. Try again." << endl;
                continue;
            }

            board[move] = currentPlayer;

            // Check for a win or draw
            if (checkWin(board, currentPlayer)) {
                displayBoard(board);
                cout << "Player " << currentPlayer << " wins!" << endl;
                game_over = true;
            } else if (checkDraw(board)) {
                displayBoard(board);
                cout << "It's a draw!" << endl;
                game_over = true;
            } else {
                // Switch players
                switchPlayer(currentPlayer);
            }
        }

        // Ask if the players want to play another game
        char play_again;
        cout << "Do you want to play another game? (y/n): ";
        cin >> play_again;
        if (play_again != 'y' && play_again != 'Y') {
            break;
        }
    }

    return 0;
}
