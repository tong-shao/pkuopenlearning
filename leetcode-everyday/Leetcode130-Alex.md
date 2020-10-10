#  Leetcode 130

Leetcode130：被围绕的区域

给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。

找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。

示例:

X X X X
X O O X
X X O X
X O X X
运行你的函数后，矩阵变为：

X X X X
X X X X
X X X X
X O X X
注：被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。

```
class Solution {
    public int n;
    public int m;
    public void dfs(char[][]board,int x,int y){
            if(x<0||x>=m||y<0||y>=n||board[x][y]!='O'){//当时X时，不用管，直接返回
                return;
            }
            else{//只对边界上的O进行dfs，并把所有相连的O标记为A，后续在总体遍历一次，标
            //记为A的变为O，其余的O变为X
                board[x][y]='A';
                   dfs(board, x + 1, y);
                   dfs(board, x - 1, y);
                   dfs(board, x, y + 1);
                   dfs(board, x, y - 1);
            }
    }
    public void solve(char[][] board) {
        m=board.length;//总行数
        n=board[0].length;//总列数
      //对二维矩阵边界上的点进行dfs，因为在dfs里写了，若点不是O，直接返回，若是O才进行遍历
      //当然，也可以另一种写法是对边界上的点进行判断，若是O才进行dfs，是X就不进行dfs了
        for(int i=0;i<m;++i){//遍历二维矩阵上下两行
            dfs(board,i,0);
            dfs(board,i,n-1);
        }
        for(int i=1;i<n-1;++i){//遍历二维矩阵左右两列
              dfs(board,0,i);
             dfs(board,m-1,i);
            //dfs(board,i,0);
            //dfs(board,i,n-1);
        }
        for(int i=0;i<m;++i){
            for(int j=0;j<n;++j){
                if(board[i][j]=='A'){
                    board[i][j]='O';
                }
                if(board[i][j]=='O'){
                    board[i][j]='X';
                }
            }
        }
    }
}
```



#### 