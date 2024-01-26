#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>
#include <windows.h>
using namespace std;

const int Size = 8;  // Kích thước bảng
const int MINES = 10;  // Số lượng mìn

struct Cell
{
    char value;  // Giá trị của ô (mìn hoặc số ô lân cận)
    bool revealed;  // Trạng thái hiển thị của ô
};

vector<vector<Cell>> board(Size, vector<Cell>(Size, { ' ', false }));

void initializeBoard()
{
    // Khởi tạo bảng với khoảng trắng và trạng thái ẩn
    for (int i = 0; i < Size; ++i)
    {
        for (int j = 0; j < Size; ++j)
        {
            board[i][j] = { ' ', false };
        }
    }
}

void placeMines()
{
    // Đặt mìn ngẫu nhiên trên bảng
    srand(time(nullptr));
    int minesPlaced = 0;
    while (minesPlaced < MINES)
    {
        int row = rand() % Size;
        int col = rand() % Size;
        if (board[row][col].value != '*')
        {
            board[row][col].value = '*';
            ++minesPlaced;
        }
    }
}

void printBoard(bool revealAll)
{
    cout << "   ";
    for (int i = 0; i < Size; ++i)
    {
        cout << i << " ";
    }
    cout << endl;

    for (int i = 0; i < Size; ++i)
    {
        cout << i << " |";
        for (int j = 0; j < Size; ++j)
        {
            if (board[i][j].revealed || revealAll)
            {
                cout << board[i][j].value << "|";
            }
            else
            {
                cout << " |";
            }
        }
        cout << endl;
    }
}

bool isValid(int row, int col)
{
    return row >= 0 && row < Size && col >= 0 && col < Size;
}

int countAdjacentMines(int row, int col)
{
    int count = 0;
    for (int i = -1; i <= 1; ++i)
    {
        for (int j = -1; j <= 1; ++j)
        {
            int newRow = row + i;
            int newCol = col + j;
            if (isValid(newRow, newCol) && board[newRow][newCol].value == '*')
            {
                ++count;
            }
        }
    }
    return count;
}

void revealCell(int row, int col)
{
    // Hàm đệ quy để hiển thị ô và các ô lân cận
    if (!isValid(row, col) || board[row][col].revealed)
    {
        return;
    }

    board[row][col].revealed = true;

    if (board[row][col].value == ' ')
    {
        int mines = countAdjacentMines(row, col);
        if (mines == 0)
        {
            for (int i = -1; i <= 1; ++i)
            {
                for (int j = -1; j <= 1; ++j)
                {
                    revealCell(row + i, col + j);
                }
            }
        }
        else
        {
            board[row][col].value = '0' + mines;
        }
    }
}

bool checkWin()
{
    for (int i = 0; i < Size; ++i)
    {
        for (int j = 0; j < Size; ++j)
        {
            if (!board[i][j].revealed && board[i][j].value != '*')
            {
                return false;
            }
        }
    }
    return true;
}

int main()
{
    int cd;
    do {
        initializeBoard();
        placeMines();
        while (true)
        {
            printBoard(false);

            int row, col;
            cout << "NHAP HANG VA COT (Cach nhau bang dau space ) ";
            cin >> row >> col;
            system("cls");
            if (!isValid(row, col))
            {
                cout << "KHONG HOP LE , HAY THU LAI" << endl;
                continue;
            }

            if (board[row][col].value == '*')
            {
                printBoard(true);
                Sleep(1000);
                system("cls");
                cout << ". o0000000. . . . o . . . 0o. . . .o0 . .o0 0 0 0 . . . .o000o. . o . . o . .o0 0 0 0 . o00000o . .\n"; Sleep(60);
                cout << ".O. . . . . . . .0.0. . . 0 0o. .o0 0 . 0 . . . . . . . 0 . . 0 . 0 . . 0 . 0 . . . . . 0 . . .0. .\n"; Sleep(60);
                cout << "0 . . . . . . . 0 . 0 . . 0 .o0 0o. 0 . .o. . . . . . .0. . . .0. 0 . . 0 . .o. . . . . 0 . . .0. .\n"; Sleep(60);
                cout << "0 . .0000o. . .0. . .0. . 0 . .0. . 0 . .o0 o o . . . .0. . . .0. o . . o . .o0 o o . . 0oooooo . .\n"; Sleep(60);
                cout << "0 . . . . 0 . ooooooooo . 0 . . . . 0 . .o. . . . . . .0. . . .0. .0. .0. . .o. . . . . 0 . . .o. .\n"; Sleep(60);
                cout << ".O. . . . 0 . 0 . . . 0 . 0 . . . . 0 . 0 . . . . . . . 0 . . 0 . . 0 0 . . 0 . . . . . 0 . . . o .\n"; Sleep(60);
                cout << ". 00000000. . 0 . . . 0 . 0 . . . . 0 . .o0 0 0 0 . . . .o000o. . . .0. . . .o0 0 0 0 . 0 . . . .0.\n"; Sleep(60);
                break;
            }
            else {
                revealCell(row, col);
                if (checkWin())
                {
                    printBoard(true);
                    Sleep(1000);
                    cout << ". . . . . . . . . oO. . . . . . . . . . .Oo . .oO . .oOo. . .oO . . . . . . . . . . .\n"; Sleep(60);
                    cout << ". . . . . . . . . .Oo . . . . . . . . . oO. . .Oo . .OooO . .Oo . . . . . . . . . . .\n"; Sleep(60);
                    cout << ". . . . . . . . . . oO. . . .OoO. . . .Oo . . .oO . .oO Oo. .oO . . . . . . . . . . .\n"; Sleep(60);
                    cout << ". . . . . . . . . . .Oo . . oO.Oo . . oO. . . .Oo . .Oo .oO .Oo . . . . . . . . . . .\n"; Sleep(60);
                    cout << ". . . . . . . . . . . oO. .Oo . oO. .Oo . . . .oO . .oO . Oo.oO . . . . . . . . . . .\n"; Sleep(60);
                    cout << ". . . . . . . . . . . .Oo oO. . .Oo oO. . . . .Oo . .Oo . .oOOo . . . . . . . . . . .\n"; Sleep(60);
                    cout << ". . . . . . . . . . . . oOo . . . oOo . . . . .oO . .oO . . OoO . . . . . . . . . . .\n"; Sleep(60);
                    break;
                }
            }
        }
        cout << " \n nhan 1 de tiep tuc "; cin >> cd; system("cls");
    } while (cd == 1);
    return 0;
}
