```cpp
#include <iostream>
#include <vector>
#include <limits>

using namespace std;

const int HUMAN = -1;
const int AI = 1;    
const int EMPTY = 0;  

void display(const vector<vector<int>>& board) {
    for (const auto& row : board) {
        for (int cell : row) {
            if (cell == HUMAN) cout << "O ";
            else if (cell == AI) cout << "X ";
            else cout << ". ";
        }
        cout << '\n';
    }
    cout << '\n';
}

int evaluate(const vector<vector<int>>& board) {
    for (int i = 0; i < 3; ++i) {
        if (board[i][0] == board[i][1] && board[i][1] == board[i][2] && board[i][0] != EMPTY)
            return board[i][0];
    }
    for (int i = 0; i < 3; ++i) {
        if (board[0][i] == board[1][i] && board[1][i] == board[2][i] && board[0][i] != EMPTY)
            return board[0][i];
    }
    if (board[0][0] == board[1][1] && board[1][1] == board[2][2] && board[0][0] != EMPTY)
        return board[0][0];
    if (board[0][2] == board[1][1] && board[1][1] == board[2][0] && board[0][2] != EMPTY)
        return board[0][2];

    return 0;
}

bool is_moves_left(const vector<vector<int>>& board) {
    for (const auto& row : board) {
        for (int cell : row) {
            if (cell == EMPTY) return true;
        }
    }
    return false;
}

int minimax(vector<vector<int>>& board, int depth, bool is_maximizing, int alpha, int beta) {
    int score = evaluate(board);

    if (score == AI) return 10 - depth;    
    if (score == HUMAN) return depth - 10; 
    if (!is_moves_left(board)) return 0;    

    if (is_maximizing) {
        int best = numeric_limits<int>::min();

        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                if (board[i][j] == EMPTY) {
                    board[i][j] = AI;
                    best = max(best, minimax(board, depth + 1, false, alpha, beta));
                    board[i][j] = EMPTY;
                    alpha = max(alpha, best);
                    if (beta <= alpha) break;
                }
            }
        }
        return best;
    } else {
        int best = INT_MAX;

        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < 3; ++j) {
                if (board[i][j] == EMPTY) {
                    board[i][j] = HUMAN;
                    best = min(best, minimax(board, depth + 1, true, alpha, beta));
                    board[i][j] = EMPTY;
                    beta = min(beta, best);
                    if (beta <= alpha) break;
                }
            }
        }
        return best;
    }
}

pair<int, int> find_best_move(vector<vector<int>>& board) {
    int bestVal = numeric_limits<int>::min();
    pair<int, int> bestMove = {-1, -1};

    for (int i = 0; i < 3; ++i) {
        for (int j = 0; j < 3; ++j) {
            if (board[i][j] == EMPTY) {
                board[i][j] = AI;
                int moveVal = minimax(board, 0, false, INT_MIN, INT_MAX);
                board[i][j] = EMPTY;

                if (moveVal > bestVal) {
                    bestVal = moveVal;
                    bestMove = {i, j};
                }
            }
        }
    }

    return bestMove;
}

int main() {
    vector<vector<int>> board(3, vector<int>(3, EMPTY));
    while (true) {

        cout << "AI's playing...\n";
        pair<int, int> bestMove = find_best_move(board);
        board[bestMove.first][bestMove.second] = AI;
        display(board);

        int result = evaluate(board);
        if (result == HUMAN) {
            cout << "You Win!\n";
            break;
        } else if (result == AI) {
            cout << "You Lose!\n";
            break;
        } else if (!is_moves_left(board)) {
            cout << "It's a draw!\n";
            break;
        }

        if (is_moves_left(board)) {
            int x, y;
            cout << "Chose (x, y): ";
            cin >> x >> y;
            x--; y--;

            if (board[x][y] == EMPTY) {
                board[x][y] = HUMAN;
            } else {
                cout << "Invalid move\n";
                continue;
            }
        }

        display(board);
        result = evaluate(board);
        if (result == HUMAN) {
            cout << "You Win!\n";
            break;
        } else if (result == AI) {
            cout << "You lose!\n";
            break;
        } else if (!is_moves_left(board)) {
            cout << "It's a draw!\n";
            break;
        }

        
    }

    return 0;
}

```