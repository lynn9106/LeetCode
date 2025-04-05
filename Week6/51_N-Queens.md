# 51. N-Queens

```C++
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> res;
        vector<string> board(n, string(n,'.'));
        vector<int> cols(n,0);
        vector<int> diag1(2 * n - 1, 0);
        vector<int> diag2(2 * n - 1, 0);
        putQueens(0, n, board, cols, diag1, diag2, res);
        return res;   
    }

private:
    void putQueens(int row, int n, vector<string>& board, 
                    vector<int>& cols,
                    vector<int>& diag1,
                    vector<int>& diag2,
                    vector<vector<string>>& res
                    ){
        if (row == n){
            res.push_back(board);   //add a complete board
            return;
        }

        for (int col = 0; col<n; col++){
            if (cols[col] || diag1[row + col] || diag2[row - col + n - 1])    
                // check if there's queen at column, \ diagonal1, /diagonal2
                continue;

            board[row][col] = 'Q';
            cols[col] = diag1[row + col] = diag2[row - col + n - 1] = 1;

            putQueens(row+1, n, board, cols, diag1, diag2, res);    // move on to next row

            board[row][col] = '.';      // return back the state
            cols[col] = diag1[row + col] = diag2[row - col + n - 1] = 0;
        }
    }
};
```