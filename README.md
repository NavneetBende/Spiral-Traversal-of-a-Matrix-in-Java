Spiral Traversal of a Matrix in Java
Here, in this page we will discuss the program to print the spiral traversal of the matrix in Java programming language. We are given with the elements of the array in two-dimensional form and we need to traverse the entire matrix in spiral form and print the corresponding element.

Spiral Traversal of a Matrix in Java
Method discussed :
Method 1 : Using Brute force.
Method 2 : Using DFS
Method 1 :
Create a recursive function that takes a matrix and some variables.
Check the base cases (starting index is less than or equal to ending index) and print the boundary elements in clockwise manner
Print the top row, i.e. Print the elements of kth row from column index l to n, and increase the count of k.
Print the right column, i.e. Print the last column or n-1th column from row index k to m and decrease the count of n.
Print the bottom row, i.e. if k > m, then print the elements of m-1th row from column n-1 to l and decrease the count of m
Print the left column, i.e. if l < n, then print the elements of lth column from m-1th row to k and increase the count of l.
Call the function recursively.
Method 1 : Code in Java
Run
import java.util.*;
 
class Main{
    static int R = 4;
    static int C = 4;
 
    static void print(int arr[][], int i, int j, int m, int n)
    {
        
        if (i >= m || j >= n) {
            return;
        }
 
        for (int p = i; p < n; p++) {
            System.out.print(arr[i][p] + " ");
        }
 
        for (int p = i + 1; p < m; p++) {
            System.out.print(arr[p][n - 1] + " ");
        }
 
        if ((m - 1) != i) {
            for (int p = n - 2; p >= j; p--) {
                System.out.print(arr[m - 1][p] + " ");
            }
        }
 
        if ((n - 1) != j) {
            for (int p = m - 2; p > i; p--) {
                System.out.print(arr[p][j] + " ");
            }
        }
        print(arr, i + 1, j + 1, m - 1, n - 1);
    }
 

    public static void main(String[] args)
    {
        int a[][] = { { 1, 2, 3, 4 },
                      { 5, 6, 7, 8 },
                      { 9, 10, 11, 12 },
                      { 13, 14, 15, 16 } };
 
        
        print(a, 0, 0, R, C);
    }
}
Output
1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10
Method 2  :
Create a DFS function which takes matrix, cell indices and direction.
Now, we will check the cell indices pointing to a valid cell (that is, not visited and in bounds),if not, skip this cell
Print the value of the cell.
And Mark matrix cell pointed by indicates as visited by changing it to a value not supported in the matrix
Now, check the surrounding cells valid. If not stop algorithm, else continue.
If direction given is right then check, is the cell to the right valid. If so, DFS to the right cell given the steps above, else change the direction to down and DFS downwards given the steps above.
Else, if the direction given is down then check, is the cell to the down valid. If so, DFS to the cell below given the steps above else, change the direction to left and DFS leftwards given the steps above.
Else, if the direction given is left then check, is the cell to the left valid. If so, DFS to the left cell given the steps above,      else change the direction to up and DFS upwards given the steps above.
Else, if the direction given is up then check, is the cell to the up valid. If so, DFS to the upper cell given the steps above,        else, change the direction to right and DFS rightwards given the steps above.
Method 2 : Code in Java
Run
import java.io.*;
import java.util.*;
 
class Main {
    public static int R = 4, C = 4;
    public static boolean isInBounds(int i, int j)
    {
        if (i < 0 || i >= R || j < 0 || j >= C)
            return false;
        return true;
    }
 

    public static boolean isBlocked(int[][] matrix, int i,  int j)
    {
        if (!isInBounds(i, j))
            return true;
        if (matrix[i][j] == -1)
            return true;
        return false;
    }
 
    
    public static void spirallyDFSTravserse(int[][] matrix, int i, int j,int dir, ArrayList res)
    {
        if (isBlocked(matrix, i, j))
            return;
        boolean allBlocked = true;
        for (int k = -1; k <= 1; k += 2) {
            allBlocked = allBlocked && isBlocked(matrix, k + i, j)  && isBlocked(matrix, i, j + k);
        }
        res.add(matrix[i][j]);
        matrix[i][j] = -1;
        if (allBlocked) {
            return;
        }
 
        
        int nxt_i = i;
        int nxt_j = j;
        int nxt_dir = dir;
        if (dir == 0) {
            if (!isBlocked(matrix, i, j + 1)) {
                nxt_j++;
            }
            else {
                nxt_dir = 1;
                nxt_i++;
            }
        }
        else if (dir == 1) {
            if (!isBlocked(matrix, i + 1, j)) {
                nxt_i++;
            }
            else {
                nxt_dir = 2;
                nxt_j--;
            }
        }
        else if (dir == 2) {
            if (!isBlocked(matrix, i, j - 1)) {
                nxt_j--;
            }
            else {
                nxt_dir = 3;
                nxt_i--;
            }
        }
        else if (dir == 3) {
            if (!isBlocked(matrix, i - 1, j)) {
                nxt_i--;
            }
            else {
                nxt_dir = 0;
                nxt_j++;
            }
        }
        spirallyDFSTravserse(matrix, nxt_i, nxt_j, nxt_dir,
                             res);
    }
 
    // to traverse spirally
    public static ArrayList
    spirallyTraverse(int[][] matrix)
    {
        ArrayList res = new ArrayList();
        spirallyDFSTravserse(matrix, 0, 0, 0, res);
        return res;
    }
 
    // Driver code
    public static void main(String[] args)
    {
        int a[][] = { { 1, 2, 3, 4 },
                      { 5, 6, 7, 8 },
                      { 9, 10, 11, 12 },
                      { 13, 14, 15, 16 } };
 
        
        ArrayList res = spirallyTraverse(a);
        int size = res.size();
        for (int i = 0; i < size; ++i)
            System.out.print(res.get(i) + " ");
        System.out.println();
    }
}
Output
1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10
