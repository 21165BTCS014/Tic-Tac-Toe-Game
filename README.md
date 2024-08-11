# Tic-Tac-Toe-Game
#include <iostream>
using namespace std;

char board[3][3] = {{'1', '2', '3'}, {'4', '5', '6'}, {'7', '8', '9'}};
char current_marker;
int current_player;

void drawBoard() {
    system("clear"); 

    cout << "Tic-Tac-Toe Game" << endl;
    cout << "Player 1 (X)  -  Player 2 (O)" << endl;
    cout << endl;

    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << board[i][j];
            if (j < 2) cout << " | ";
        }
        cout << endl;
        if (i < 2) cout << "--|---|--" << endl;
    }
    cout << endl;
}

bool placeMarker(int slot) {
    int row = (slot - 1) / 3;
    int col = (slot - 1) % 3;

    if (board[row][col] != 'X' && board[row][col] != 'O') {
        board[row][col] = current_marker;
        return true;
    }
    return false;
}

int checkWin() {
    // Rows
    for (int i = 0; i < 3; i++) {
        if (board[i][0] == board[i][1] && board[i][1] == board[i][2])
            return current_player;
    }
    // Columns
    for (int i = 0; i < 3; i++) {
        if (board[0][i] == board[1][i] && board[1][i] == board[2][i])
            return current_player;
    }
    // Diagonals
    if (board[0][0] == board[1][1] && board[1][1] == board[2][2])
        return current_player;
    if (board[0][2] == board[1][1] && board[1][1] == board[2][0])
        return current_player;

    return 0;
}

bool isDraw() {
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (board[i][j] != 'X' && board[i][j] != 'O')
                return false;
        }
    }
    return true;
}

void switchPlayer() {
    current_player = (current_player == 1) ? 2 : 1;
    current_marker = (current_marker == 'X') ? 'O' : 'X';
}

void game() {
    current_player = 1;
    current_marker = 'X';

    int player_won;
    while (true) {
        drawBoard();
        int slot;
        cout << "Player " << current_player << " (" << current_marker << ") enter your move (1-9): ";
        cin >> slot;

        if (slot < 1 || slot > 9) {
            cout << "Invalid move. Try again.\n";
            continue;
        }

        if (!placeMarker(slot)) {
            cout << "Slot already taken. Try again.\n";
            continue;
        }

        player_won = checkWin();
        if (player_won == 1 || player_won == 2) {
            drawBoard();
            cout << "Player " << current_player << " (" << current_marker << ") wins!\n";
            break;
        }

        if (isDraw()) {
            drawBoard();
            cout << "It's a draw!\n";
            break;
        }

        switchPlayer();
    }
}

void resetBoard() {
    char initial[3][3] = {{'1', '2', '3'}, {'4', '5', '6'}, {'7', '8', '9'}};
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            board[i][j] = initial[i][j];
        }
    }
}

int main() {
    char playAgain;
    do {
        resetBoard();
        game();
        cout << "Do you want to play again? (y/n): ";
        cin >> playAgain;
    } while (playAgain == 'y' || playAgain == 'Y');

    cout << "Thanks for playing!" << endl;
    return 0;
}
