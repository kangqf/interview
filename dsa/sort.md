## 排序
归并与快排
``` cpp
#include <iostream>
#include <vector>

using namespace std;


int partition(vector<int> &data,int start, int end)
{
    int i = start, j = end;
    int tmp = data[start];
    while(i < j)
    {
        while(data[j]>tmp && i<j)
            j--;
        if(i<j)
            data[i] = data[j];
        while(data[i]<tmp && i<j)
            i++;
        if(i<j)
            data[j] = data[i];
    }
    data[i] = tmp;
    return i;
}

void quick_sort(vector<int> &data, int start, int end)
{
    if(end <= start)//只有一个元素的不用进行排序的
        return ;
    int parPos = partition(data,start,end);
    quick_sort(data,start,parPos-1);
    quick_sort(data,parPos+1,end);

}

void merge(vector<int> &data, int start, int middle, int end)
{
    vector<int> tmp(end-start+1,0);
    int i = start, j = middle+1;
    int k = 0;
    while(i <= middle && j <= end)
    {
        if(data[i] < data[j])
            tmp[k++] = data[i++];
        else
            tmp[k++] = data[j++];
    }
    while(i <= middle)//取数组里面没有取完的
        tmp[k++] = data[i++];
    while(j <= end)
        tmp[k++] = data[j++];

    for(int m = 0; m < tmp.size(); ++m)
    {
        data[start+m] = tmp[m];
    }
}

void mergeSort(vector<int> &data, int start, int end)
{
    if(start >= end)
        return;
    int middle = (start+end)/2;
    mergeSort(data,start,middle);
    mergeSort(data,middle+1,end);
    merge(data,start,middle,end);
}


int main(int argc, char *argv[])
{
    vector<int> data = {1,3,2,5,4,6,8,7,9};

    //quick_sort(data,0,data.size()-1);

    mergeSort(data,0,data.size()-1);

    for(auto it:data)
    {
        cout<<it<<" ";
    }

    return 0;
}
```

### 拓扑排序

### 归并排序

### 冒泡排序

### 选择排序

### 插入排序

### 桶排序

### 希尔排序

### 外部排序