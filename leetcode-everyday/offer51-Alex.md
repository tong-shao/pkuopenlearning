# 剑指 Offer 51. 数组中的逆序对

**归并排序**，同时可利用归并排序的特性计算序列的逆序数。

归并排序是用分治思想，分治模式在每一层递归上有三个步骤：

​               分解：将n个元素分成个含n/2个元素的子序列。

​               解决：用合并排序法对两个子序列递归的排序。

​               合并：合并两个已排序的子序列已得到排序结果。

算法一直把序列分解，最后算法将自底向上两个长度为一的序列合并成已排好序的，接着将长度为2的序列合并成长度为4的有序序列，等等。一者合并成长度为n的有序序列。

在归并排序算法中修改，就可以在nlog2(n)的时间内求逆序对。

```
#include<iostream>
#include<cstdio>
#include<algorithm>
using namespace std;
const int maxn=1010;
int arr[maxn];
int temp[maxn];
int number;//记录逆序数对的数目
void combine(int left,int middle,int right){//归并排序的合并方法
    int i=left;
    int j=middle+1;
    int k=left;
    while(i<=middle&&j<=right){
        if(arr[i]<=arr[j]){
            temp[k++]=arr[i++];
        }
        else{
            temp[k++]=arr[j++];//以middle划分的左右两部分中，
            number+=middle-i+1;//当出现左值大于右值时，
        }                      //逆序数对为middle—i+1
    }
    while(i<=middle){
        temp[k++]=arr[i++];
    }
    while(j<=right){
        temp[k++]=arr[j++];
    }
    for(k=left;k<=right;k++){
        arr[k]=temp[k];
    }
}
void mergesort(int left,int right){//递归实现归并排序
    if(left<right){
        int middle=left+(right-left)/2;
        mergesort(left,middle);
        mergesort(middle+1,right);
        combine(left,middle,right);
    }
}
int main(){
    int casenumber;//多组数据输入
    scanf("%d",&casenumber);
    int current;
    for(current=1;current<=casenumber;current++){
    number=0;
    int n;
    scanf("%d",&n);
    int i;
    for(i=0;i<n;i++){
        scanf("%d",&arr[i]);
    }
    mergesort(0,n-1);
    printf("Scenario #%d:\n",current);
    printf("%d\n\n",number);
    }
    return 0;
}
```

