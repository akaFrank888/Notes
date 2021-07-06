# Java递归实现一个数的全排列



## 1. 数学分析

- **一个数的全排列**：如排列[1]
  - 只有[1]一种情况
- **两个数的全排列**：如排列[1,2]
  - 第一步：将[1]放入第零个位置，对剩下的[2]进行全排列，结果为[1,2]
  - 第二步：将[2]放入第零个位置，对剩下的[1]进行全排列，结果为[2,1]
  - （共两种情况）
- **三个数的全排列**：如排列[1,2,3]
  - 第一步：将[1]放入第零个位置，对剩下的[2,3]进行全排列，结果为[1,2,3],[1,3,2]
  - 第二步：将[2]放入第零个位置，对剩下的[1,3]进行全排列，结果为[2,1,3],[2,3,1]
  - 第一步：将[3]放入第零个位置，对剩下的[1,2]进行全排列，结果为[3,1,2],[3,2,1]
  - （共六种情况）
- ==m个数的全排列：将m个数分别放在第零个位置，对剩下m-1个数进行全排序。当标记数组位置的n指向最后一个数时，为递归的出口。==



## 2. 找递归出口和递归部分



## 3. 代码实现

```java
    public static void fullPermutation(int[] a, int n) {

        int length = a.length;
        // 递归出口 ———— 当n指向最后一个数时，输出数组，并结束
        if (n == length - 1) {
            System.out.println(Arrays.toString(a));
            return;
        }

        // 将m个元素分别放到第零个位置（包括本身），再对剩下m-1个元素进行全排序
        for (int i = n; i < length; i++) {
            swap(a, n, i);
            fullPermutation(a, n + 1);
            swap(a, n, i);
        }

    }

    public static void swap(int[] a, int i, int j) {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
    public static void main(String[] args) {
        int[] a = {1, 2, 3};
        fullPermutation(a, 0);
        System.out.println(Arrays.toString(a));
    }
}
```



## 4. 反思：

1. 不太理解为什么再swap一次。
2. 