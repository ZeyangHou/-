Given a sequence of K integers { N1, N2, ..., NK}. A continuous subsequence is defined to be { Ni, Ni+1, ..., Nj} where 1≤i≤j≤K. The Maximum Subsequence is the continuous subsequence which has the largest sum of its elements. For example, given sequence { -2, 11, -4, 13, -5, -2 }, its maximum subsequence is { 11, -4, 13 } with the largest sum being 20.

Now you are supposed to find the largest sum, together with the first and the last numbers of the maximum subsequence.

Input Specification:
Each input file contains one test case. Each case occupies two lines. The first line contains a positive integer K (≤10000). The second line contains K numbers, separated by a space.

Output Specification:
For each test case, output in one line the largest sum, together with the first and the last numbers of the maximum subsequence. The numbers must be separated by one space, but there must be no extra space at the end of a line. In case that the maximum subsequence is not unique, output the one with the smallest indices i and j (as shown by the sample case). If all the K numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence.

Sample Input:
```
10
-10 1 2 3 4 -5 -23 3 7 -21
```
Sample Output:
```
10 1 4
```
my program:
====
```
#include<stdio.h>
int main()
{
    int k;
    scanf("%d",&k);
    int firnum,lanum,inum,jnum,tinum,sum=0,flag=0,curnum,maxnum=-1;
    for (int i = 0; i < k; i++)
    {
        scanf("%d",&curnum);
        sum+=curnum;
        if (i==0)
        {
            firnum=curnum;
            if (curnum>=0)
            {
                inum=curnum;
            }
            
        }
        if (i==k-1)
        {
            lanum=curnum;
        }
        
        if (flag)
        {
            inum=curnum;
            
        }        
        if (sum<0)
        {
            sum=0;
            flag=1;
        }
        else
        {
            flag=0;
            if (sum>maxnum)
            {
                tinum=inum;
                jnum=curnum;
                maxnum=sum;
            }
        }
    }
    if (maxnum>=0)
    {
        printf("%d %d %d",maxnum,tinum,jnum);
    }
    else
    {
        printf("0 %d %d",firnum,lanum);
    }
    return 0;   
}
```
题目思路
====
要分情况讨论，就数字类型来说有：1全负数、2有负有零、3全零、4有零有正、5全正，对应的输出也有不同，
其中1应该输出
```
0 第一个数 最后一个数
```
2、3应该输出
```
0 0 0
```
4如果0出现在最大子列和的开头，为了满足output the one with the smallest indices i and j，应该输出
```
和 0 结尾数
```
4如果0包含在子列内部，以及5全正的情况时正常输出
因为1、2和3的最大子列和都是0，但是输出却不尽相同，所以为了区分这些情况可以将最大子列和变量maxsum设为-1，这样对应于情况1子算出的最大子列和为-1，对应于情况2、3则为0
对于数字j的确定是相对容易的，只要sum>maxsum就应该将当前的curnum用于更新jnum。但是inum的确定是相对困难的，inum对应于最大子列的开头，最大子列的开头要分为是否在数列的一开始两种情况，如果在数列的一开始，那么数列的第一个数就是inum，将其存储下来，如果在后续出现了sum<0的情况，则用下一个curnum来更新inum，但是inum的最终确定需要确保当前子列成为最大，因此需要在满足sum>maxnum后将inum赋值给tinum（trueinum）。以sum<0作为更新inum的条件也同时保证了上述情况4的正确输出。
