#include <stdio.h>

char board[9] = {'1','2','3','4','5','6','7','8','9'};

// Print board
void printBoard() {
    printf("\n");
    printf(" %c | %c | %c \n", board[0], board[1], board[2]);
    printf("---|---|---\n");
    printf(" %c | %c | %c \n", board[3], board[4], board[5]);
    printf("---|---|---\n");
    printf(" %c | %c | %c \n", board[6], board[7], board[8]);
    printf("\n");
}

// Check winner
int checkWinner() {
    int wins[8][3] = {
        {0,1,2},{3,4,5},{6,7,8},
        {0,3,6},{1,4,7},{2,5,8},
        {0,4,8},{2,4,6}
    };

    for(int i=0;i<8;i++) {
        int a=wins[i][0];
        int b=wins[i][1];
        int c=wins[i][2];

        if(board[a]==board[b] && board[b]==board[c]) {
            if(board[a]=='O') return 1;   // AI wins
            if(board[a]=='X') return -1;  // Human wins
        }
    }

    return 0;
}

// Check draw
int isDraw() {
    for(int i=0;i<9;i++) {
        if(board[i]!='X' && board[i]!='O')
            return 0;
    }
    return 1;
}

// Minimax
int minimax(int isAI) {
    int score = checkWinner();

    if(score == 1) return 1;
    if(score == -1) return -1;
    if(isDraw()) return 0;

    if(isAI) {
        int best = -100;

        for(int i=0;i<9;i++) {
            if(board[i] != 'X' && board[i] != 'O') {
                char temp = board[i];
                board[i] = 'O';

                int value = minimax(0);

                board[i] = temp;

                if(value > best)
                    best = value;
            }
        }
        return best;
    }
    else {
        int best = 100;

        for(int i=0;i<9;i++) {
            if(board[i] != 'X' && board[i] != 'O') {
                char temp = board[i];
                board[i] = 'X';

                int value = minimax(1);

                board[i] = temp;

                if(value < best)
                    best = value;
            }
        }
        return best;
    }
}

// AI move
void aiMove() {
    int bestScore = -100;
    int move = -1;

    for(int i=0;i<9;i++) {
        if(board[i] != 'X' && board[i] != 'O') {
            char temp = board[i];
            board[i] = 'O';

            int score = minimax(0);

            board[i] = temp;

            if(score > bestScore) {
                bestScore = score;
                move = i;
            }
        }
    }

    board[move] = 'O';
    printf("AI chooses position %d\n", move + 1);
}

// Main
int main() {
    int pos;

    printf("TIC TAC TOE\n");
    printf("You = X\nAI = O\n");

    while(1) {
        printBoard();

        printf("Enter position (1-9): ");
        scanf("%d", &pos);

        if(pos < 1 || pos > 9 ||
           board[pos-1]=='X' ||
           board[pos-1]=='O') {
            printf("Invalid move!\n");
            continue;
        }

        board[pos-1] = 'X';

        if(checkWinner() == -1) {
            printBoard();
            printf("You Win!\n");
            break;
        }

        if(isDraw()) {
            printBoard();
            printf("Draw!\n");
            break;
        }

        aiMove();

        if(checkWinner() == 1) {
            printBoard();
            printf("AI Wins!\n");
            break;
        }

        if(isDraw()) {
            printBoard();
            printf("Draw!\n");
            break;
        }
    }

    return 0;
}
