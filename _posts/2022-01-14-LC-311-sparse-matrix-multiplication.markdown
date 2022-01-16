---
layout: post
title:  "LC 311. Sparse Matrix Multiplication"
date:   2022-01-14 21:23:04 -0800
category: algorithm
---
# LC 311. Sparse Matrix Multiplication

2D数组相乘公式 `A[i][k] * B[k][j] = C[i][j]`

## Method 1
根据上面公式最直接的做法，对Metrix A每个元素做for循环，找到相乘的数，再更新结果
```java
public int[][] multiply(int[][] A, int[][] B) {
    int rowA = A.length, colA = A[0].length;
    int rowB = B.length, colB = B[0].length;
    if (colA != rowB) return null;
    int[][] res = new int[rowA][colB];
    for (int i = 0; i < rowA; i++) {
        for (int k = 0; k < colA; k++) {
            if (A[i][k] != 0)
            for (int j = 0; j < colB; j++) {
                if (B[k][j] != 0) 
                    res[i][j] += A[i][k] * B[k][j];
            }
        }
    }
    return res;
}
```
Time complexity: `O(n^3)`

## Method 2
首先对sparse metrix 做处理，把sparse metrix 用row[], col[], val[]的三个list来表示，再根据上述公式进行计算
```java
public int[][] multiply(int[][] A, int[][] B) {
    List<Integer> rowA = new ArrayList<>();
    List<Integer> colA = new ArrayList<>();
    List<Integer> valA = new ArrayList<>();
    List<Integer> rowB = new ArrayList<>();
    List<Integer> colB = new ArrayList<>();
    List<Integer> valB = new ArrayList<>();
    buildMatrix(rowA, colA, valA, A);
    buildMatrix(rowB, colB, valB, B);
    int[][] res = new int[A.length][B[0].length];
    for (int i = 0; i < rowA.size(); i++) {
        int col = colA.get(i);
        for (int j = 0; j < rowB.size(); j++) {
            if (col == rowB.get(j)) {
                res[rowA.get(i)][colB.get(j)] += valA.get(i) * valB.get(j);
                
            }
        }
    }
    return res;
}

private void buildMatrix(List<Integer> row,
    List<Integer> col,
    List<Integer> val,
    int[][] m) {
      for (int i = 0; i < m.length; i++) {
        for (int j = 0; j < m[0].length; j++) {
            if (m[i][j] == 0) {
                continue;
            }
            row.add(i);
            col.add(j);
            val.add(m[i][j]);
        }
    }
}
```
Time complexity: `O(n+n^2)`