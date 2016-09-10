# JavaScript和C的数学解题

这段时间，怿飞的博客上有一篇[用 JavaScript 解数学题](http://www.planabc.net/2010/05/26/solving_the_mathematical_problem_using_javascript/)，代码很简练优雅。于是突然想用C写写看，因为很久没写，就权当是练习也挺不错啊:)

**题目**是这样的：一个六位数，分别用2，3，4，5，6乘它，得到的五个新数仍是由原数中的六个数字组成，只是位置不同，则此六位数是多少？
```
#include <stdio.h>
#include <string.h>

#define DIGIT 6
#define MULTI 5

void swap( char *a, char *b ){
    int tmp;
    tmp = *a;
    *a = *b;
    *b = tmp;
}

void bubbleSort( char arr[] )
{
    int i,j;
    for(i = 0; i < DIGIT; i++){
        for(j = 0; j < DIGIT-1; j++){
            if ( arr[j] > arr[j+1] ){
                swap( arr+j, arr+j+1 );
            }
        }
    }
}

int main(){
    int num, numCopy, i, j, ind;
    int mul[MULTI] = {2,3,4,5,6};
    char indStr[DIGIT+1];
    char StrToCmp[DIGIT+1];
    for( num = 1000000/6; num >= 100000; num-- ){
        numCopy = num;
        for( i = 0; i < DIGIT; i++ ){
            StrToCmp[i] = numCopy%10+48;
            numCopy /= 10;
        }
        StrToCmp[DIGIT] = '\0';
        bubbleSort( StrToCmp );
        for( i = 0; i < MULTI; i++ ){
            ind = num * mul[i];
            for( j = 0; j < DIGIT; j++){
                indStr[j] = ind%10 + 48;
                ind /= 10;
            }
            indStr[DIGIT] = '\0';
            bubbleSort( indStr );
            if( strcmp( StrToCmp, indStr )){
                j = 0;
                break;
            }
        }
        if(j){
            printf( "This number is %d\n", num );
        }
    }
    getchar();
    return 0;
}
```
无奈水平不高，用C写费了不少事。主要是排序(总觉得这里用个qsort会很别扭)以及转换。中间的转换步骤因为又有段时间不做题的关系，有点生疏。不过最后还是捣鼓出来了，尽管写的比较简陋~。

**答案是：142857。**