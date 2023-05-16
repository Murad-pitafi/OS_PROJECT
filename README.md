# OS_PROJECT
OS project on Parallel processing of algorithms 
## **Group Member**  
| Name  | Student Id | Email |
| ------------- | ------------- | ------------- |
| Muhammad Murad  | 21K-3064  | K213064@nu.edu.pk  |
| Ayesha Irshad | 21K-4734  | K214734@nu.edu.pk  |
## **Project Introduction** 

This project is about determining the most efficient programming technique out of the following three: Serial, OpenMp, and pthreads. This is done by experimenting each of these techniques on the following three sorting algorithms: Binary-Insertion Sort, Selection Sort, and Cocktail Sort. The serial, obviously, falls under the sequential programming, while the other two are used to achieve Parallel Programming.

## **Project Goal**
In this project, we aim to discover the most efficient technique which is done by executing programs of which each contain an array, with unsorted elements, and using a timer to record the sorting time of each technique. 
<details>
	<summary>Binary Insertion Sort:</summary><blockquote>  
	<details>  
		<summary>     Serial:</summary>  
		   
```C
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

**Selection Sort**		
// Serial
#include <stdio.h>
#include <time.h>
#include <wait.h>
#include<sys/wait.h>
#include <stdlib.h>
#include<unistd.h>
#include<sys/time.h>
clock_t ticks;
struct timeval stop, start;
void selectionSort(int* A, int n);
void swap(int* a, int* b);
void display(int* arr, int n);
int main(){

	time_t t;
    int number, iter =0, find;
	srand((unsigned) time(&t));	

	printf("\nEnter the Size of the Array: ");	
    scanf("%d", &number);
    int *Arr = (int *)malloc( number * sizeof(int));

    for(; iter<number; iter++){
	Arr[iter] = rand() % 100;    
	}

    
	double bstart = clock();
	gettimeofday(&start, NULL);
	selectionSort(Arr, number);
	gettimeofday(&stop, NULL);
	double bstop = clock();    
	display(Arr, number);
	FILE* fp;
	fp = fopen("Timings.txt", "a");
    	fprintf(fp, "Serial Burst Time: %lf\n",difftime(bstop,bstart));
	fprintf(fp, "Serial Execution Time: %lu\n\n\n", (stop.tv_sec - start.tv_sec) * 1000000 + stop.tv_usec - start.tv_usec);	
	fclose(fp);    
}

void display(int* arr, int n){
	printf(“\nSorted Array\n”)’
       for(int i = 0; i<n; i++)
    {
        if(i != n - 1)
        {
            printf("%d , ", *(arr + i));        
        }
        else
            printf("%d \n", *(arr + i));
    }
    
}

void selectionSort(int* A, int n)
{
    for(int startpos =0; startpos < n-1; startpos++){
        int maxpos = startpos;
        for(int i=startpos +1; i< n; ++i){
            if(A[i] < A[maxpos]){
                maxpos = i;
            }
        }
	if(maxpos != startpos)
        	swap(&A[startpos], &A[maxpos]);
    }

}

void swap(int* a, int* b){ int temp = *a;    *a = *b;    *b = temp; }

		
		
![image](https://github.com/Murad-pitafi/OS_PROJECT/assets/87450783/22e06393-001e-4d92-adc3-d2ec25a3e2bc)
				 
// in parallel	
#include <stdio.h>
#include <time.h>
#include <wait.h>
#include<sys/wait.h>
#include <stdlib.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/time.h>
//time_t start, stop;
struct timeval stop, start;
clock_t ticks;    
int linearSearch(int* A, int n, int tos);
void* selectionSort();
void swap(int* a, int* b);
void* merge(void* args);
void display(int* arr, int n);
#define MAX_THREAD 2

struct sortingArgs{
	
	int *ptr;
	int size;
	int start, end;
}FinalArgs;
int main(){
    
	time_t t;

	srand((unsigned) time(&t));
    pthread_t threads[MAX_THREAD];
    int number, iter =0;

	struct sortingArgs Args;
	
	printf("\nEnter the Size of the Array: ");	
 scanf("%d", &number);
	Args.size = number;
	Args.start = 0;
	Args.end = number/2;	
	int Arr[number];
    	Args.ptr = (int *)calloc( Args.size, Args.size * sizeof(int));
	
	for(; iter<number; iter++){		
		*(Args.ptr + iter) = rand() % 100;
	}
    	
	double bstart = clock();
	gettimeofday(&start, NULL);
			
       		 
		pthread_create(&threads[0], NULL, selectionSort, &Args);	
		pthread_join(threads[0], NULL);	

		
		Args.start = number/2 + 1;
		Args.end = number;

		
		pthread_create(&threads[1], NULL, selectionSort, &Args);
		pthread_join(threads[1], NULL);	
		
		pthread_create(&threads[0], NULL, merge, &Args);
		pthread_join(threads[0], NULL);
		
	gettimeofday(&stop, NULL);
	double bstop = clock(); 	

	Args.ptr = FinalArgs.ptr;
	
    	   
	display(FinalArgs.ptr, number);
	FILE* fp;
	fp = fopen("Timings.txt", "a");
    	fprintf(fp, "Multi-threading Burst Time: %lf\n",difftime(bstop,bstart));
	fprintf(fp, "Multi-threading Execution Time: %lu\n\n\n",(stop.tv_sec - start.tv_sec) * 1000000 + stop.tv_usec - start.tv_usec);	
	fclose(fp);        
}


void* selectionSort(void* input)
{
 	struct sortingArgs *Arg = (struct sortingArgs*) input;

	int n = Arg->end;
	int s = Arg->start;
	
    for(int startpos = s; startpos < n-1; startpos++){
        int maxpos = startpos;
        for(int i=startpos +1; i< n; ++i){
		if(Arg->ptr[i] < Arg->ptr[maxpos])              
		  	maxpos = i;
            
        }
	if(maxpos != startpos)
        	swap(&Arg->ptr[startpos], &Arg->ptr[maxpos]);
	}

	pthread_exit(0);
}


void* merge(void* args)
{
	struct sortingArgs *Arg = (struct sortingArgs*) args;
	FinalArgs.size = Arg->size;

	FinalArgs.ptr = (int *)calloc( FinalArgs.size, FinalArgs.size * sizeof(int));
	int i = 0, j = Arg->size/2 + 1, k=0;
	
	for(; i <(Arg->size/2 + 1); k++)
	{
				
		if(Arg->ptr[i] < Arg->ptr[j] || j>= Arg->size)
		{
			FinalArgs.ptr[k] = Arg->ptr[i];
			i++;
		}
		else
		{
			if(j<Arg->size)
			{
				FinalArgs.ptr[k] = Arg->ptr[j];
				j++;
			}
		}	
	} 
	while(j < Arg->size)
	{
		FinalArgs.ptr[k] = Arg->ptr[j];
		k++;
		j++;
	}
	
//pthread_exit(0);
}
void swap(int* a, int* b){
    int temp = *a;
    *a = *b;
    *b = temp;
}

void display(int* arr, int n){
    
    printf("\nSorted Array\n");
    for(int i = 0; i<n; i++)
    {
        if(i != n - 1)
        {
            printf("%d , ", *(arr + i));        
        }
        else
            printf("%d \n", *(arr + i));
    }
    
}

			
![Screenshot from 2023-05-16 22-39-03](https://github.com/Murad-pitafi/OS_PROJECT/assets/87450783/9bb2f068-3a8a-44c1-9e26-6b949c93c7df)





