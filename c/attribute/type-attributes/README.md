# Type attributes  
Khi ta khai báo một struct thì nó sẽ được cấp phát tự động một vùng nhớ có độ dài mà ta khó có thể biết chính xác. Ở phần lớn các bài toán ta không cần quan tâm xem struct mà mình khai báo sẽ được cấp phát như thế nào và nó được cấp bao nhiêu byte. Tuy nhiên ở một vài bài toán đặc thù thì ta phải kiểm soát, biết chắc chắn struct mà mình khai báo sẽ được cấp phát như thế nào. Trong struct sẽ cung cấp cho ta một số thuộc tính để làm việc này.  
Ví dụ: như struct bên dưới tốn 12 byte chứ không phải 9 byte.
```c
#include <stdio.h>

typedef struct
{
    char cdata; //4
    int idata; //4
    float fdata; //4
} Normal_t;//12

int main(int argc, char const *argv[])
{
    Normal_t n;
    
    printf("Kich thuoc cac kieu du lieu trong C:\n");
    printf("\tsizeof(char): %d\n", (int)sizeof(char));
    printf("\tsizeof(int): %d\n", (int)sizeof(int));
    printf("\tsizeof(float): %d\n", (int)sizeof(float));
    printf("\tsizeof(long): %d\n", (int)sizeof(long));
    printf("\tsizeof(double): %d\n", (int)sizeof(double));
    printf("\tsizeof(void*): %d\n", (int)sizeof(void*));
    printf("\tsizeof(wchar_t): %d\n", (int)sizeof(wchar_t));
    printf("\n\n");

    printf("sizeof(Normal_t): %d\n", (int)sizeof(n));

    return 0;
}
```

## \_\_attribute__((packed))
Dùng để minimum độ dài vùng nhớ của struct, mục đích tiết kiệm bộ nhớ.  
Lúc này kiểu struct bên dưới dưới sẽ tốn 9 byte.

```c
#include <stdio.h>

typedef struct 
{  
    char cdata; //1  
    int idata; //4  
    float fdata; //4  
}__attribute__ ((packed)) Packed_t;// == minimum ==> 9  

int main(int argc, char const *argv[])
{
    Packed_t p;  

    printf("sizeof(Packed_t): %d\n", (int)sizeof(p)); 

    return 0;
}
```

## \_\_attribute__((aligned)) 
Thuộc tính aligned(n) dùng để cho biết struct sẽ được cấp vùng nhớ có độ dài
sizeof struct = n*x, trong đó n là số byte để aligne, giá trị n = 2^m, tức là bằng cấp số mũ của 2; x là số nguyên dương với điều kiện x >= 1.

```c
typeder struct 
{  
    char cdata;  
    int idata;
}__attribute__ ((aligned (4))) Aligned4_t; // 5 == align 4 ==> 8

typedef struct  
{  
    char cdata;  
    int idata;  
    float fdata;  
}__attribute__ ((aligned (8))) Aligned_t; // 12 == align 8 ==> 16
```

## \_\_attribute__ ((__transparent_union__))
Trước tiên cần nói đến sự khác nhau giữa **struct** và **union**  

| **struct** | **union** |
| --- | --- |
| Size của struct ít nhất bằng tổng size của các thành phần của struct. Mình sử dụng từ *ít nhất* là vì size struct còn phụ thuộc vào alignment struct. | Size của union bằng size của thành phần có size lớn nhất trong union. |
| Tại cùng 1 thời điểm run-time, có thể truy cập vào tất cả các thành phần của struct | Tại cùng 1 thời điểm run-time, chỉ có thể truy cập 1 thành phần của union |

Ví dụ về union :
```c
#include <stdio.h>
 
union date
{
    int d;
    int m;
    int y;
};
 
void main()
{
    date dat;
 
    printf("\nSize of union: %d", sizeof(date));
    dat.d = 24;
    dat.m = 9;
    dat.y = 2014;
 
    printf("\ndate = %d", dat.d);
    printf("\nmonth = %d", dat.m);
    printf("\nyear = %d", dat.y);
 
}
/* Kết quả: date = month = year = 2014 */
```
Thêm 1 ví dụ nữa 

```c
#include <stdio.h>
#include <conio.h>

union date
{
    int d;
    int m;
    int y;
};

void main()
{
    date dat;
 
    printf("\nSize of union: %d", sizeof(date));
     
    dat.d = 24;
    printf("\ndate = %d", dat.d);
 
    dat.m = 9;
    printf("\nmonth = %d", dat.m);
 
    dat.y = 2014;
    printf("\nyear = %d", dat.y);
}
/*
Kết quả in ra là:
    date = 24
    month = 9
    year = 2014
*/
```
