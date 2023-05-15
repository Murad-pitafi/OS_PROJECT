# OS_PROJECT
OS project on Parallel processing of algorithms 

## **Project Introduction** 

This project is about determining the most efficient programming technique out of the following three: Serial, OpenMp, and pthreads. This is done by experimenting each of these techniques on the following three sorting algorithms: Binary-Insertion Sort, Selection Sort, and Cocktail Sort. The serial, obviously, falls under the sequential programming, while the other two are used to achieve Parallel Programming.

## **Project Goal**
In this project, we aim to discover the most efficient technique which is done by executing programs of which each contain an array, with unsorted elements, and using a timer to record the sorting time of each technique. 
## **Binary Insertion Sort:**
### **Serial:**
```
#include <stdio.h>
#include <stdlib.h>
#include <sys/time.h>
#include <time.h>
#define SIZE 10
int num1[SIZE];
struct timeval Stop2,start2;   // micro second resolution
int binary_search(int Num1,int left,int right)
{
	if(right<=left)
	{
		if(Num1>num1[left])
		{
			return (left+1);
		}
		else
		{
			return left;
		}
	}
	int mid_int=(left+right)/2;
	if(Num1==num1[mid_int])
	{
		return (mid_int+1);
	}
	if(Num1>num1[mid_int])
	{
		return binary_search(Num1,mid_int+1,right);
	}
	else
	{
		return binary_search(Num1,left,mid_int-1);
	}		
	}
	

void insertion_sort()
{
	int num2,i,j,num3,k;
	for(i=1;i<SIZE;i++)
	{
		j=i-1;
		num3=num1[i];
		num2=binary_search(num3,0,j);
		while(j>=num2)
		{
			k=num1[j];
			num1[j+1]=k;
			j--;
		}
		j++;
		num1[j]=num3;
	}		
	}void fill_array(int size) {
    int i;
    srand(time(NULL));
    for (i=0; i<size; i++)
        num1[i] = rand() % 100;
}
void file_create(double start,double stop)
{
	FILE *ptr;
	ptr=fopen("Sequencial_Insertion.txt","a+");
	if(ptr==NULL)
	{
		printf("Unable to Open File");
		exit(1);
	}
	else
	{
		double total= (double)(stop-start);
		fprintf(ptr,"Time Taken(Burst Time): %lf\n",total);
		fprintf(ptr,".comTime Taken(Clock Time): %lu\n",(Stop2.tv_sec-start2.tv_sec)*1000000+Stop2.tv_usec-start2.tv_usec);	
	}
	fclose(ptr);
}
	int main()
	{
		
	fill_array(SIZE);
	double Start1=clock();
	
	gettimeofday(&start2,NULL);
	insertion_sort();
	gettimeofday(&Stop2,NULL);
	double stop1=clock();
	for(int i=0;i<SIZE;i++)
	{
		printf("%d ",num1[i]);
	}
	
	file_create(Start1,stop1);	
	return 0;
	}
```

### **Output Screen Shot**








