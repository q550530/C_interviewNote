# 面試筆記

近期又開始了一系列網通廠的韌體面試，記錄一些自己讀的東西

**指標** 

1.指標 (pointer)：一個指向某個儲存位址的變數
 ```
 int *ptr = &var;
 ```
其中  
  & : 取變數位址  
  ** : 表示為指標變數  

也可用於函數變為函式指標 (function pointer)，語法為  
```
typedef int (*FPTR)(int, int);

int add(int a, int b){
  return a+b;
}

int mult(int a, int b){
  return a*b;
}

int main (){

int a = 5, b = 4;
//// typedef
FPTR fptr = add;
printf("add = %d", fptr(a, b));  // 9
fptr = mult;
printf("add = %d", fptr(a, b));  // 20

//// fun pointer
int (*ffptr)(int a, int b);
ffptr = add;
printf("add = %d", ffptr(a, b));  // 9
ffptr = mult;
printf("add = %d", ffptr(a, b));  // 20

}

```
2. 基礎指標判讀  

指標判讀大原則為「從右讀到左」，例如：  
```
int a; // 一個整型數
int *a; // 一個指向整數的指標
int **a; // 一個指向指標的指標，它指向的指標是指向一個整型數
int a[10]; // 一個有10個整數型的陣列
int *a[10]; // 一個有10個指標的陣列，該指標是指向一個整數型的
int (*a)[10]; // 一個指向有10個整數型陣列的指標
int (*a)(int); // 一個指向函數的指標，該函數有一個整數型參數並返回一個整數
int (*a[10])(int); // 一個有10個指標的陣列，該指標指向一個函數，該函數有一個整數型參數並返回一個整數
```
3. 指標與其他關鍵字混用  
一樣右讀到左，例如： 
```
const int * foo; // 一個 pointer，指向 const int 變數。
int const * foo; // 一個 pointer，指向 const int 變數。
int * const foo; // 一個 const pointer，指向 int 變數。
int const * const foo; // 一個 const pointer，指向 const int 變數。
```
4. Call by Value, Call by Reference, Call by Addres

C 理論實際上只有call by address, 但民間為了教學方便所以有了call by address 一說

#swap

call by value
```
void swap(int a, int b){
    int tmp = a;
    a = b;
    b = tmp;
}

int main(){
    int v1 = 5;
    int v2 = 10;
    
    swap(v1, v2);
    printf("v1 = %d ; v2 = %d", v1, v2); /// v1 = 5 ; v2 = 10
}   
```
```
Ans:v1 = 5 ; v2 = 10
```
Note:因為在傳值後 swap function做完後memory會直接被釋放，而傳入的值v1, v2的記憶體為值中的值沒有被變動

call by reference
```
void swap(int &a, int &b){
    int tmp = a;
    a = b;
    b = tmp;
}

int main(){
    int v1 = 5;
    int v2 = 10;
    
    swap(v1, v2);
    printf("v1 = %d ; v2 = %d", v1, v2); 
}   
```
```
Ans:v1 = 10 ; v2 = 5
```
Note:call by reference 是只有C++才有的

call by address
```
void swap(int *a, int *b){
    int tmp = *a;
    *a = *b;
    *b = tmp;
}

int main(){
    int v1 = 5;
    int v2 = 10;
    
    swap(&v1, &v2);
    printf("v1 = %d ; v2 = %d", v1, v2); 
}   
```
```
Ans:v1 = 10 ; v2 = 5
```

Note:C語言只有call by value, 而所謂的call by address 是將記憶體位址傳入到funcion中，在function中變動完的記憶體位址中的值就可以撈到

5. 指標常見問題
   
*The content of array a
```
int a[ ]={6,7,8,9,10};
int *p= a;
*(p++) +=123;
*(++p) +=123;
a=?
```
```
Ans:{129, 7, 131, 9, 10}
```
Note:只要記住pointer在前是先取值在運算或位移, 在後就是先運算或位移再取值

*ask: the value of *(a+1) and *(p-1)??
```
int a[5] = {1, 2, 3, 4, 5};
int *p = (int*)(&a+1);

```
```
Ans:*(p-1)=5 ; *(a+1)=2 
```
Note: (&a+1) 是指整個整個int array記憶體位子往後挪一位 
a            &a+1
 |1|2|3|4|5| |Unknow|Unknow|Unknow|Unknow|Unknow|

*ask: the value of ques1 and ques2??
```
int a = 25;
int b = 30;
int ques1 = a++ + b++;
int ques2 = ++a + ++b;
printf("%d, %d", ques1, ques2);
```
```
Ans:55 , 59 
```
Note:入上述pointer在前是先取值在運算或位移, 在後就是先運算或位移再取值



-----------------------------
**變數範圍和生命周期**

local 變數 : local 變數僅活在該函式內，存放位置在 stack 記憶體中。
```
void func(void){
    int i = 0 ;
    i++ ;
    printf("%d" , i ) ;
}

int main(){
    for(int i = 0; i < 5; i++){
        func();
    }
}

```
```
Ans:11111
```

static 變數 : static 變數生命周期(life time)跟程式一樣長，而範圍(scope)則維持不變，即在宣告的函式之外仍無法存取 static 變數。
```
void func(void){
    static int i = 0 ;
    i++ ;
    printf("%d" , i ) ;
}

int main(){
    for(int i = 0; i < 5; i++){
        func();
    }
}

```
```
Ans:12345
```
Note: 因為static在記憶體的位置只會做一次init

global 變數 : 所有區段皆可使用此變數

-----------------------------
**巨集 #define**
```
#define PI 3.1415926    //常數巨集
#define A(x) x    //函數巨集
#define MIN(A，B)  ( (A)  <= (B) ? (A) : (B))
```
#常見問題

括號
```
#define SUM(a,b)  a+b
SUM(2,5)*10;


#define MUX(a, b) a*b
MUX(10+5, 10-5) = ?

```
```
Ans:52 55
```
Note: Macro 中沒有括號故展開後為 2+5*10 先乘除後加減，得輸出為 52

```
#define INC(x) x*=2; x+=1

int main()
{                       
    int i, j;
    for (i = 0, j = 1; i < 5; i++)
        INC(j);
    printf("j = %d\n", j);
}
```
```
Ans:33

///展開後為
int main()
{                       
    int i, j;
    for (i = 0, j = 1; i < 5; i++)
        j*=2;
    j+=1;
    printf("j = %d\n", j);
}
```
Note: Macro 中沒有括號，故展開後如上第1次：j = 2 ;第2次：j = 4;第3次：j = 8;第4次：j = 16;第5次：j = 32

 ---------------------------- 
**bitwise operator**  
 邏輯上的運算子在 C 中的語法分別如下：  
AND （&）  
OR（|）  
NOT（!）  
XOR（^）// bit值不一樣為 1  
complement（~）  
shift (<<, >>)  

bitwise 的操作常與 "0x" 這種 16 進位表示法，方便轉換操作。  
 
* Normal operator  
``` 
unsigned long num_a = 0x00001111;
unsigned long num_b = 0x00000202;
unsigned long num_c;

num_c = num_a & (~num_b);
num_c = num_c | num_b;

cout<<(num_c); // 00001313
 ```
* mask method and bitwise opreator
 ```
a = a | 7    // 最右側 3 位設為 1，其餘不變。
a = a & (~7) // 最右側 3 位設為 0，其餘不變。
a = a ^ 7    // 最右側 3 位執行 NOT operator，其餘不變。
 ```
 
----------------------------- 
