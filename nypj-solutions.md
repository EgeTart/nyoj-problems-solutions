##problem-4 ASCII码排序
描述

输入三个字符（可以重复）后，按各字符的ASCII码从小到大的顺序输出这三个字符。

```c
#include <stdio.h>
#include <string.h>

void swap(char *x, char *y) {
    char tmp = *x;
    *x = *y;
    *y = tmp;
}

int main()
{
    int n;
    scanf("%d", &n);
    
    char ascii[4];
    while(n--) {
        scanf("%s", ascii);
        
        if(ascii[0] > ascii[1])
            swap(&ascii[0], &ascii[1]);
        
        if(ascii[0] > ascii[2])
            swap(&ascii[0], &ascii[2]);
        
        if(ascii[1] > ascii[2])
            swap(&ascii[1], &ascii[2]);
        
        printf("%c %c %c\n", ascii[0], ascii[1], ascii[2]);
    }
    
    return 0;
}
```

***

##problem-11 奇偶数分离
描述:

有一个整型偶数n(2<= n <=10000),你要做的是：先把1到n中的所有奇数从小到大输出，再把所有的偶数从小到大输出。

```c
#include <stdio.h>

int main()
{
    int testCount;
    scanf("%d", &testCount);
    
    int n;
    while(testCount--) {
        scanf("%d", &n);
        
        for(int i = 1; i <= n; i += 2) {
            (i == 1)? printf("%d", i) : printf(" %d", i);
        }
        printf("\n");
        
        for(int i = 2; i <= n; i += 2) {
            (i == 2)? printf("%d", i) : printf(" %d", i);
        }
        printf("\n");
    }
    
    return 0;
}
```
***

##problem-13 Fibonacci数
描述

无穷数列1，1，2，3，5，8，13，21，34，55...称为Fibonacci数列，它可以递归地定义为 
F(n)=1 ...........(n=1或n=2)   
F(n)=F(n-1)+F(n-2).....(n>2)  
现要你来求第n个斐波纳奇数。（第1个、第二个都为1)

```c
#include <stdio.h>

int main()
{
    int nums[20] = {1, 1};
    
    for(int i = 2; i < 20; ++i) {
        nums[i] = nums[i - 1] + nums[i - 2];
    }
    
    int testCount;
    scanf("%d", &testCount);
    
    int n;
    while(testCount--) {
        scanf("%d", &n);
        printf("%d\n", nums[n - 1]);
    }
    
    return 0;
}
```
***

#problem-22 素数求和问题
描述

现在给你N个数（0<N<1000），现在要求你写出一个程序，找出这N个数中的所有素数，并求和。

```c
#include <stdio.h>

int isPrime(int x) {
    int i;
    for(i = 2; i * i <= x; ++i) {
        if(x % i == 0)
            return 0;
    }
    
    return 1;
}

int main()
{
    int primes[1010] = {0};
    primes[2] = 1;
    
    int i;
    for(i = 3; i <= 1010; i += 2) {
        if(isPrime(i))
            primes[i] = 1;
    }
    
    int testCount;
    scanf("%d", &testCount);
    
    int n;
    int primeSum = 0;
    int num;
    while(testCount--) {
        
        scanf("%d", &n);
        for(i = 0; i < n; ++i) {
            scanf("%d", &num);
            if(primes[num])
                primeSum += num;
        }
        
        printf("%d\n", primeSum);
        
        primeSum = 0;
    }
    
    return 0;
}
```
***

#problem-24 素数距离问题
描述

现在给出你一些数，要求你写出一个程序，输出这些整数相邻最近的素数，并输出其相距长度。如果左右有等距离长度素数，则输出左侧的值及相应距离。如果输入的整数本身就是素数，则输出该素数本身，距离输出0

```c
#include <stdio.h>

int primeNumbers[80000];

int isPrime(int x) {
    int i;
    for(i = 2; i * i <= x; ++i) {
        if(x % i == 0)
            return 0;
    }
    
    return 1;
}

int binarySearch(int numbers[], int arrayLength, int key) {
    
    int low = 1, high = arrayLength;
    int mid = 0;
    
    while (low <= high) {
        mid = low + (high - low) / 2;
        
        if (numbers[mid] < key) {
            low = mid + 1;
        }
        else if (numbers[mid] > key) {
            high = mid - 1;
        }
        else {
            return mid;
        }
    }
    
    return mid;
}

int main() {
    
    primeNumbers[1] = 2;
    
    int primeCount = 1;
    for (int i = 3; i <= 1000100; ++i) {
        if (isPrime(i)) {
            primeCount += 1;
            primeNumbers[primeCount] = i;
        }
    }
    
    int testCount;
    scanf("%d", &testCount);
    
    int n;
    while (testCount--) {
        scanf("%d", &n);
        
        int index = binarySearch(primeNumbers, primeCount, n);
        
        int resultIndex = 0, resultDiff = 0;
        
        if (n == 1) {
            resultIndex = 1;
            resultDiff = 1;
        }
        else if (primeNumbers[index] == n) {
            resultIndex = index;
            resultDiff = 0;
        }
        else if (primeNumbers[index] < n) {
            resultIndex = index;
            resultDiff = n - primeNumbers[index];
            
            if (primeNumbers[index + 1] - n < resultDiff) {
                resultIndex = index + 1;
                resultDiff = primeNumbers[index + 1] - n;
            }
        }
        else if (primeNumbers[index] > n) {
            resultIndex = index;
            resultDiff = primeNumbers[index] - n;
            
            if (n - primeNumbers[index - 1] <= resultDiff) {
                resultIndex = index - 1;
                resultDiff = n - primeNumbers[index - 1];
            }
        }
        
        printf("%d %d\n", primeNumbers[resultIndex], resultDiff);
    }
    
    return 0;
}
```

##problem-33 蛇形填数
描述

在 n * n方阵里填入1,2,...,n*n,要求填成蛇形。例如n=4时方阵为：  
10 11 12 1  
9 16 13 2  
8 15 14 3  
7 6 5 4    

```c
#include <stdio.h>

int main() {
    
    int matrix[110][110] = {0};
    
    int n;
    scanf("%d", &n);
    
    int num = 1;
    int row = 0, col = n;
    while (num <= n * n) {
        
        while (row < n && !matrix[row+1][col]) {
            matrix[++row][col] = num;
            num += 1;
        }
        
        while (col > 1 && !matrix[row][col-1]) {
            matrix[row][--col] = num;
            num++;
        }
        
        while (row > 1 && !matrix[row-1][col]) {
            matrix[--row][col] = num;
            num++;
        }
        
        while (col < n && !matrix[row][col+1]) {
            matrix[row][++col] = num;
            num++;
        }
    }
    
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= n; ++j) {
            (j == 1) ? printf("%d", matrix[i][j]) : printf(" %d", matrix[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```