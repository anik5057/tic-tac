# tic-tac
import java.util.Scanner;

public class TicTacToe {
    static char[][] board = {
        {' ', '|', ' ', '|', ' '},
        {'-', '+', '-', '+', '-'},
        {' ', '|', ' ', '|', ' '},
        {'-', '+', '-', '+', '-'},
        {' ', '|', ' ', '|', ' '}
    };

    static char currentPlayer = 'X';

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        boolean gameEnded = false;

        printBoard();

        while (!gameEnded) {
            System.out.println("Player " + currentPlayer + ", enter your move (1-9): ");
            int move = scanner.nextInt();

            if (isValidMove(move)) {
                placeMove(move);
                printBoard();

                if (checkWinner()) {
                    System.out.println("Player " + currentPlayer + " wins!");
                    gameEnded = true;
                } else if (isBoardFull()) {
                    System.out.println("The game is a draw!");
                    gameEnded = true;
                } else {
                    currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
                }
            } else {
                System.out.println("Invalid move! Try again.");
            }
        }

        scanner.close();
    }

    static void printBoard() {
        for (char[] row : board) {
            for (char c : row) {
                System.out.print(c);
            }
            System.out.println();
        }
    }

    static boolean isValidMove(int pos) {
        int[] indices = getBoardIndices(pos);
        return board[indices[0]][indices[1]] == ' ';
    }

    static void placeMove(int pos) {
        int[] indices = getBoardIndices(pos);
        board[indices[0]][indices[1]] = currentPlayer;
    }

    static int[] getBoardIndices(int pos) {
        switch (pos) {
            case 1: return new int[]{0, 0};
            case 2: return new int[]{0, 2};
            case 3: return new int[]{0, 4};
            case 4: return new int[]{2, 0};
            case 5: return new int[]{2, 2};
            case 6: return new int[]{2, 4};
            case 7: return new int[]{4, 0};
            case 8: return new int[]{4, 2};
            case 9: return new int[]{4, 4};
            default: return new int[]{-1, -1};
        }
    }

    static boolean checkWinner() {
        // Check rows, columns, and diagonals
        return (checkLine(0, 0, 0, 2, 0, 4) || // row 1
                checkLine(2, 0, 2, 2, 2, 4) || // row 2
                checkLine(4, 0, 4, 2, 4, 4) || // row 3
                checkLine(0, 0, 2, 0, 4, 0) || // col 1
                checkLine(0, 2, 2, 2, 4, 2) || // col 2
                checkLine(0, 4, 2, 4, 4, 4) || // col 3
                checkLine(0, 0, 2, 2, 4, 4) || // diag 1
                checkLine(0, 4, 2, 2, 4, 0));  // diag 2
    }

    static boolean checkLine(int r1, int c1, int r2, int c2, int r3, int c3) {
        return (board[r1][c1] == currentPlayer &&
                board[r2][c2] == currentPlayer &&
                board[r3][c3] == currentPlayer);
    }

    static boolean isBoardFull() {
        for (int pos = 1; pos <= 9; pos++) {
            int[] indices = getBoardIndices(pos);
            if (board[indices[0]][indices[1]] == ' ') {
                return false;
            }
        }
        return true;
    }
}
