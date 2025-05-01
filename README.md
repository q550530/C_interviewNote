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
  printf("v1 = %d ; v2 = %d", v1, v2); /// v1 = 10 ; v2 = 5
}   
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
  printf("v1 = %d ; v2 = %d", v1, v2); /// v1 = 10 ; v2 = 5
}   
```
Note:C語言只有call by value, 而所謂的call by address 是將記憶體位址傳入到funcion中，在function中變動完的記憶體位址中的值就可以撈到


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
