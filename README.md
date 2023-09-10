# TicTacToe-Project
#include <iostream>
#include <vector>
#include <ctime>
#include <cstdlib>

using namespace std;

// Function to print the Tic-Tac-Toe board
void printBoard(vector<vector<char>>& board) {
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << board[i][j];
            if (j < 2) cout << " | ";
        }
        cout << endl;
        if (i < 2) cout << "---------" << endl;
    }
}

// Function to check if a player has won
bool checkWin(vector<vector<char>>& board, char player) {
    for (int i = 0; i < 3; i++) {
        if (board[i][0] == player && board[i][1] == player && board[i][2] == player) return true;
        if (board[0][i] == player && board[1][i] == player && board[2][i] == player) return true;
    }
    if (board[0][0] == player && board[1][1] == player && board[2][2] == player) return true;
    if (board[0][2] == player && board[1][1] == player && board[2][0] == player) return true;
    return false;
}

// Function to check if the board is full (a tie)
bool checkTie(vector<vector<char>>& board) {
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (board[i][j] == ' ') return false;
        }
    }
    return true;
}

// Function to make a move for the AI
void makeAIMove(vector<vector<char>>& board, char aiSymbol, char playerSymbol) {
    // Simple AI: Randomly select an empty cell
    int row, col;
    do {
        row = rand() % 3;
        col = rand() % 3;
    } while (board[row][col] != ' ');

    board[row][col] = aiSymbol;
}

int main() {
    vector<vector<char>> board(3, vector<char>(3, ' '));
    char playerSymbol, aiSymbol;

    cout << "Welcome to Tic-Tac-Toe!" << endl;
    cout << "Choose your symbol (X or O): ";
    cin >> playerSymbol;

    if (playerSymbol == 'X' || playerSymbol == 'x') {
        playerSymbol = 'X';
        aiSymbol = 'O';
    } else {
        playerSymbol = 'O';
        aiSymbol = 'X';
    }

    srand(static_cast<unsigned>(time(0)));

    int currentPlayer = rand() % 2;  // Randomly decide who goes first
    int moves = 0;

    while (true) {
        cout << "Current board:" << endl;
        printBoard(board);

        if (currentPlayer == 0) {
            cout << "Your turn! Enter row (0-2) and column (0-2): ";
            int row, col;
            cin >> row >> col;

            if (row < 0 || row > 2 || col < 0 || col > 2 || board[row][col] != ' ') {
                cout << "Invalid move. Try again." << endl;
                continue;
            }

            board[row][col] = playerSymbol;
        } else {
            cout << "AI's turn..." << endl;
            makeAIMove(board, aiSymbol, playerSymbol);
        }

        if (checkWin(board, playerSymbol)) {
            cout << "You win! Congratulations!" << endl;
            break;
        } else if (checkWin(board, aiSymbol)) {
            cout << "AI wins! Better luck next time!" << endl;
            break;
        } else if (checkTie(board)) {
            cout << "It's a tie! Good game!" << endl;
            break;
        }

        currentPlayer = 1 - currentPlayer;  // Switch players
        moves++;
    }

    cout << "Final board:" << endl;
    printBoard(board);

    return 0;
}
