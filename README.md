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

*ask: the value ??
```
int main(){
	int arr[] = {10,20,30,40,50};
	int *ptr1 = arr;
	int *ptr2 = arr + 5;
	printf("%d", (ptr2-ptr1));
	printf("%d", (char*)ptr2 - (char*)ptr1);
}
```
```
Ans:5 20
```
Note: ptr1 指向 arr[0] ptr2 指向 arr[5] 已經超出arr有值的範圍
ptr2 - ptr1 → 「ptr2 比 ptr1 多移動了幾個 int？」所以是 5
(char*)ptr2 - (char*)ptr1 → 差了幾個 byte（結果是 20，因為一個 int 是 4 bytes）

*ask: the value ??
```
int main(){
	int arr1[] = {10,20};
	int arr2[] = {10,20};
	int arr3[] = {10,20};
	int *p = arr1;
	int *q = arr2;
	int *r = arr3;
	++*p;
	*q++;
	*++r;
	printf("%d %d %d\n", arr1[0], arr1[1], *p);
	printf("%d %d %d\n", arr2[0], arr2[1], *q);
	printf("%d %d %d\n", arr3[0], arr3[1], *r);
}
```
```
Ans:
11 20 11
10 20 10
10 20 20
```
Note:如上述pointer在前是先取值在運算或位移, 在後就是先運算或位移再取值
++*p or ++(*p) 是直接取p值做運算而非位移

*ask: the value ??
```
void f(int *p, int *q){
	p = q;
	*p = 2;
}

int i=0, j=1;
int main(){
	f(&i, &j);
	printf("%d %d\n", i, j);
}
```
```
Ans:0 2
```
Note:function f中的p 指向位置改成的q

*ask: the value ??
```
int main(){
	char s[ ]="0113256";
	char *p = s;
	
	printf("%c", *p++);
	printf("%c", *(p++));
	printf("%c", (*p)++);
	printf("%c", *++p);
	printf("%c", *(++p));
	printf("%c", ++*p);
	printf("%c", ++(*p));
	printf("\n");
	printf("%s",s);
}
```
```
Ans:
0113234
0123456
```
Note:先取直後取值的差異

*ask: the value ??
```
int main(){
int ref[]={8,4,0,2};
int *ptr;
int index;
for(index=0, ptr=ref; index<2; index++,ptr++)
     printf("%d %d\n", ref[index], *ptr);
(*ptr++);
printf("%d %d\n", ref[index], *ptr);
}
```
```
Ans:
8 8
4 4
0 2
```
Note: index = 2（來自前面的迴圈）因為index=2時才會跳出迴圈

*ask: the value ??
```
int a[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
int *p = &(a + 1)[3];
printf("%d\n", *p);
```
```
Ans:5
```
Note: (&a+1)跟&(a+1) 一個是指標位移整個array 一個是指標位移一格

*what is final value of cnt
```
int cnt = 10;
const char *pc = "welcome";
while(*pc++)
	cnt++;
```
```
Ans:17
```
Note: 字串的最後一個是'\0' 也算

*ask: output the value ??
```
ine main() {
	char *str[ ][2] =
		{ "professor", "Justin" ,
		  "teacher", "Momor" ,
		  "student", "Caterpillar"};

	char str2[3][10] = {"professor", "Justin", "etc"};
	printf("%s\n", str[1][1]);
	printf("%c\n",str2[1][1]);
}
```
```
Ans:
Momor
u
```
Note: str 是一個*str[3][2]字串陣列 str[1][0]= teacher; str[0][1] = Justin , str2 是一個字元陣列


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
```
Ans:00001313
```
Note:
a=0001 0001 0001 0001
b=0000 0010 0000 0010

b ~= 1111 1101 1111 1101
c = a & (~b) 0001 0001 0001 0001
c |= b 0001 0011 0001 0011 => 1313

*set bit
```
a |= (1 << n)
```
*clear bit
```
a &= ~(1 << n)
```
*rever bit
```
a ^= (1 << n)
```

* mask method and bitwise opreator
```
a = a | 7    // 最右側 3 位設為 1，其餘不變。
a = a & (~7) // 最右側 3 位設為 0，其餘不變。
a = a ^ 7    // 最右側 3 位執行 NOT operator，其餘不變。
```
 * Power of 2
```
return ((n & (n - 1)) == 0)
```
Note: 2的倍數只會有1個1 所以-1時會除了原本的位子會變0其他都為1

* 確認有幾個1
```
int count_bit(int x){

	int count = 0;
	while(x){
		count++;
		x= x & (x - 1);
	}
	return count;
}
```
* count_bit(456) + count_bit(123) + count_bit(789) = ?
```
Ans:4 + 6 + 5 = 16
```
 
----------------------------- 
**資料結構**

sizeof(byte)

sizeof(double)=8
sizeof(float)=4
sizeof(int)=4
sizeof(short)=2 
sizeof(char)=1
sizeof(*ptr)=8

* out put value
```
char *i[ ] = “Hello”;
char p = 1;
int n = 10;
printf(“%d %d %d”, sizeof(i), sizeof(p), 		sizeof(n));
```
```
Ans:8 1 4
```
Note: 指標大小為8

* out put value
```
union StateMachine {
    char character;
    int number;
    char *str;
};

int main(void) {
    union StateMachine machine;
    machine.number = 1;

    printf("sizeof: %lu\n", sizeof(machine));
    printf("number: %d\n", machine.number);
}
```
```
Ans:
8
1
```
Note: union 只會框出型別中最大的記憶體位置

* out put value
```
struct StateMachine {
    char character;
    int number;
    char *str;
};

int main(void) {
    struct StateMachine machine;
    machine.number = 1;

    printf("sizeof: %lu\n", sizeof(machine));
    printf("number: %d\n", machine.number);
}
```
```
Ans:
16
1
```
Note: struct 有記憶體對齊 所以char 跟int排列順序會直接組成8 + pointer size 8

* out put value
```
struct StateMachine {
    int number;
    char *str;
	char character;
};

int main(void) {
    struct StateMachine machine;
    machine.number = 1;

    printf("sizeof: %lu\n", sizeof(machine));
    printf("number: %d\n", machine.number);
}
```
```
Ans:
24
1
```
Note: struct 有記憶體對齊 排列順序會不足的地方直接組成8 + pointer size 8 + char 8

--------------------------------------------
**實做**

*strcpy
Set by array
```
void strcpy(char * dst, char * src){
	int i = 0;
	while( src[i] != '\0'){
		dst[i] = src[i];
		i++;
	}
}
```
Set by pointer
```
void strcpy(char * dst, char * src){
	int i = 0;
	while( *src != '\0'){
		*dst = *src;
		src++;
		dst++;
	}
}
```
*strlen
Set by array
```
int strlen(char * str){
	int i = 0;
	while( str[i] != '\0'){
		i++;
	}
	return i;
}
```
Set by pointer
```
int strlen(char * str){
	int i = 0;
	while( *str != '\0'){
		src++;
		i++;
	}
	return i;
}
```
*memcpy
```
void memcpy(void *dst, void *src, int memlen){

	char *d = (char *) dst;
	char *s = (char *) src;

	if ((dst == NULL) || (src == NULL)){
		printf("Pointer is NULL");
		exit(1);
	}
	for(int i = 0; i < memlen; i++){
		*d = *s;
	}

}
```
Note: 使用 char* 是因為 char 是最小記憶體單位（1 byte），用來逐 byte 複製

*String compare
```
int strcmp(char *str1, char *str2){

	if ((*str1 == '\0') || (*str2 == '\0')){
		printf("str is empty");
		return 0;
	}
	while( (*str1 != '\0') && (*str2 != '\0') ){
		if((*str1) != (*str2)){
			return 0;
		} else {
			str1++;
			str2++;
		}
	}
	// 須確保字串最後的結尾字也相符 否則str1=abc str2= abcd 會在檢查完c後因str1沒了就跳出
	if (*str1 == '\0' && *str2 == '\0') {
        return 1;
    } else {
        return 0;
    }
}
```

*String 反轉
```
void strRev(char* str){
	int len = strlen(str);
	char tmp = '\0';
	for(int i = 0; i < len/2; i++ ){
		tmp=str[i];
		str[i] = str[len - i - 1];
		str[len - i - 1] = tmp;
	}
}
```
Note: char 是字元 char*是字元指標有可能為字串

*Linklist
find value node in a range
```
int findNode(struct Node *head, int range, int value){

	if(head->data == value){
		return 0;
	}
	
	struct Node *tmpN = head;
	for(int i = 0; i < range; i++){
		if(tmpN->data == value){
			return (i);
		}else{
			tmpN=tmpN->next;
		}		
	}
	return -1;
}

```

find node
```
int findNode(struct Node *head, int range, int value){

	if(head->data == value){
		return 0;
	}
	
	struct Node *tmpN = head;
	int idx = 0;
	while(tmpN->next != NULL){
		if(tmpN->data == value)
			return idx;
		else{
			idx++;
			tmpN=tmpN->next;
		}
	}
	return -1;
}

```
insert node
```
void insertNode(struct Node** head, int insertidx, struct Node* insertNode){

	struct Node *current = *head;
	struct Node *perv = NULL;
	if(insertidx == 0){
		insertNode->next = head;
		*head = insertNode;
	}

	for(int i = 0; i < insertidx; i++ ){
		perv = current;
		current = current->next;
	}
	perv->next=insertNode;
	insertNode->next=current;

}
```
Note:因要改變頭部指標，head 是 struct Node** head，這樣才能影響外部變數

del node
```
void delNode(struct Node** head, struct Node* delNode){

	struct Node *current = *head;
	struct Node *perv = NULL;

	if(*(head)->data == delNode->data){
		*head=*head->next;
	}
	
	while(current->next != NULL){
		if(current->data == delNode->data){
			perv->next=current->next;
            return;
		}else{
			perv=current;
			current=current->next;
		}
	}
}
```


-------------------------------------------------------------
**參考**

https://paicc.github.io/2018/09/29/%E9%9D%A2%E8%A9%A6%E8%80%83%E9%A1%8C-C%E8%AA%9E%E8%A8%80/
https://medium.com/@earth875/c-c-%E5%B8%B8%E8%A6%8B%E8%A9%A6%E9%A1%8C-961619b14f88
https://hackmd.io/@DirtyDIrty/ry2v2i2Ui#Linked-list
https://hackmd.io/IMACcx1lRdSqjw_QdQJXiA#1-Function-Pointer
