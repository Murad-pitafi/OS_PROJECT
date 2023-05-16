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
		<summary>Serial:</summary>  
		   
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
### Output:  
		  
![image](https://github.com/Murad-pitafi/OS_PROJECT/assets/87450783/b35dc444-0b78-4228-8118-605f40eefa16)
		</details>
<details>
		<summary> parallel</summary>

#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <sys/time.h>
#define SIZE 10
int n1[SIZE];
int n2[SIZE];
struct timeval Stop2,start2;
typedef struct dim{
	int start;
	int end;
}limit;

typedef struct joined{
int Start;
int mid;
int End;
}join;


int binary_search(int move,int start,int end)
{
	if (end<=start)
	{
        	if(move > n1[end])
		{
			return (end + 1);
		} 
		else
		{
			return end;
		}
	}
 
    	int mid = (start + end) / 2;
 
    	if(move == n1[mid])
	{
        	return mid + 1;
	}
 
    	if(move > n1[mid])
	{
        	return binary_search(move,mid + 1, end);
	}
    	return binary_search(move, start,mid - 1);
}


void* merge(void *args) {
   
    struct joined *params = (struct joined*) args;
    int begin = params->Start,
        mid = params->mid,
        end = params->End;


    int i = begin, j = mid, tpos = begin;

    while (i < mid && j <= end)
    {
        if (n1[i] < n1[j])
            n2[tpos++] = n1[i++];
        else
            n2[tpos++] = n1[j++];
    }

    
    while (i <mid)
        n2[tpos++] = n1[i++];

    
    while (j <= end)
        n2[tpos++] = n1[j++];
    return NULL;
}

void* insertion(void *l)
{
	limit *l1= (limit*)l;
	int i=l1->start;
	i++;
	int j,k,move;
	while(i<=l1->end)
	{
		j=i-1;
		move=n1[i];
		k=binary_search(move,l1->start,j);
		while(j>=k)					
		{
			n1[j+1]=n1[j];
			j--;
		}
		n1[j+1]=move;
		i++; 
	}
	for(int i=l1->start;i<l1->end;i++)
	{
		printf("%d ",n1[i]);
	}
	printf("\n\n\n");
	return NULL;
}


void fillarray(int size) {
    int i;
    srand(time(NULL));
    for (i=0; i<size; i++)
        n1[i] = rand() % 100;
}

void print_array(int list[], int size) {
    int i;
    for (i=0; i<size-1; i++)
        printf("%d, ", list[i]);
    printf("%d\n", list[i]);
}


void file_create(double start,double stop)
{
	FILE *ptr;
	ptr=fopen("PTHREAD_Insertion.txt","a+");
	if(ptr==NULL)
	{
		printf("Unable to Open File");
		exit(1);
	}
	else
	{
		double total= (double)(stop-start);
		fprintf(ptr,"Time Taken(Burst Time): %lf\n",total);
		fprintf(ptr,"Time Taken(Clock Time): %lu\n",(Stop2.tv_sec-start2.tv_sec)*1000000+Stop2.tv_usec-start2.tv_usec);	
	}
	fclose(ptr);
}
		

int main()
{
	fillarray(SIZE);
	pthread_t t1,t2;
	limit l1,l2;
	l1.start=0;
	l1.end=SIZE/2;
	l2.start=l1.end+1;
	l2.end=SIZE-1;
	join j1;
	j1.Start=l1.start;
	j1.mid=l2.start;
	j1.End=l2.end;
	
	pthread_t m1;
	
	double Start1=clock();
	gettimeofday(&start2,NULL);
	
	pthread_create(&t1,NULL,insertion,&l1);
		
	pthread_create(&t2,NULL,insertion,&l2);
	pthread_join(t1,NULL);
	pthread_join(t2,NULL);
	pthread_create(&m1,NULL,merge,&j1);
		
	pthread_join(m1,NULL);
	gettimeofday(&Stop2,NULL);	
	double stop1=clock();
	
	print_array(n2,SIZE);
	file_create(Start1,stop1);
	return 0;

![Screenshot from 2023-05-16 22-51-07](https://github.com/Murad-pitafi/OS_PROJECT/assets/87450783/4638094e-65b8-4914-bdbb-afb0a73dcd55)

		
</details>
<details>
		<summary> OpenMP</summary>

// openmp
#include <stdio.h>
#include <stdlib.h>
#include <omp.h>
#include <time.h>
#include <sys/time.h>

#define Size 50
struct timeval Stop2,start2;
int n1[Size];
int n2[Size];
typedef struct dim{
	int start;
	int end;
}limit;

typedef struct joined{
int Start;
int mid;
int End;
}join;


int binary_search(int move,int start,int end)
{
	if (end <= start)
	{
        	if(move > n1[end])
		{
			return (end + 1);
		} 
		else
		{
			return end;
		}
	}
 
    	int mid = (start + end) / 2;
	if(move == n1[mid])
	{
        	return mid + 1;
	}					
	if(move > n1[mid])
	{
       		return binary_search(move,mid + 1, end);
	}
	return binary_search(move, start,mid - 1);			
    	
}


void* merge(void *args) {
    //unpack parameters
    struct joined *params = (struct joined*) args;
    int begin = params->Start,
        mid = params->mid,
        end = params->End;


    int i = begin, j = mid, tpos = begin;

    while (i < mid && j <= end)
    {
        if (n1[i] < n1[j])
            n2[tpos++] = n1[i++];
        else
            n2[tpos++] = n1[j++];
    }

    //still elements left over in first list. copy over
    while (i <=mid)
        n2[tpos++] = n1[i++];

    //still elements left over in first list. copy over
    while (j <= end)
        n2[tpos++] = n1[j++];
    return NULL;
}

void* insertion(void *l)
{
	limit *l1= (limit*)l;
	int i=l1->start;
	i++;
	int j,k,move;
	while(i<=l1->end)
	{
		j=i-1;
		move=n1[i];
		k=binary_search(move,l1->start,j);
		while(j>=K)
		{
			n1[j+1]=n1[j];
			j--;
		}
		n1[j+1]=move;
		i++;
	}
	return NULL;
}



void fill_array(int size) {
    int i;
    srand(time(NULL));
    for (i=0; i<size; i++)
        n1[i] = rand() % 100;
}

void print_array(int *list, int size) {
    int i;
    for (i=0; i<size-1; i++)
        printf("%d, ", list[i]);
    printf("%d\n", list[i]);
}




void file_create(double start,double stop)
{
	FILE *ptr;
	ptr=fopen("OPENMP_Insertion.txt","a+");
	if(ptr==NULL)
	{
		printf("Unable to Open File");
		exit(1);
	}
	else
	{
		double total= (double)(stop-start);
		fprintf(ptr,"Time Taken(Burst Time): %lf\n",total);
		fprintf(ptr,"Time Taken(Clock Time): %lu\n",(Stop2.tv_sec-start2.tv_sec)*1000000+Stop2.tv_usec-start2.tv_usec);	
	}
	fclose(ptr);
}



int main()
{
	fill_array(Size);
	limit l1,l2;
	l1.start=0;
	l1.end=Size/2;
	l2.start=l1.end+1;	
	l2.end=Size-1;
	join j1;
	j1.Start=l1.start;
	j1.mid=l2.start;
	j1.End=l2.end;
	double Start1=clock();
	
	gettimeofday(&start2,NULL);
	#pragma omp parallel sections num_threads(2)
	{
		#pragma omp section
		{
			insertion(&l1);		
		}
		#pragma omp section
		{
			insertion(&l2);		
		}
	}
	
		

	merge(&j1);
	gettimeofday(&Stop2,NULL);
	double stop1=clock();
	print_array(n2,Size);
	file_create(Start1,stop1);
	return 0;
}
		![image](https://github.com/Murad-pitafi/OS_PROJECT/assets/87450783/a325c560-21fb-4971-aef2-577bc9090aec)

</details>
</blockquote>		
		
		
</details>
<details> 	
	<summary>Selection Sort</summary>
<blockquote>
<details>
	<summary>Serial</summary>
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
</details>				 
<details>

<summary>parallel</summary>	
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
</details>
<details>
	<summary>openmp</summary>
#include <stdio.h>
#include <time.h>
#include <wait.h>
#include<sys/wait.h>
#include <stdlib.h>
#include<unistd.h>
#include<sys/time.h>

struct timeval stop, start;
clock_t ticks;    
int linearSearch(int* A, int n, int tos);
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

    
    //time(&start);
    
	double bstart = clock();
	gettimeofday(&start, NULL);
	selectionSort(Arr, number);
    //time(&stop);
	gettimeofday(&stop, NULL);
	double bstop = clock();    
	display(Arr, number);
	FILE* fp;
	fp = fopen("Timings.txt", "a");
    	fprintf(fp, "OpenMP Burst Time: %lf\n",difftime(bstop,bstart));
	fprintf(fp, "OpenMP Execution Time: %lu\n\n\n",(stop.tv_sec - start.tv_sec) * 1000000 + stop.tv_usec - start.tv_usec);	
	fclose(fp);       
	//printf("\nTIME: %lf ",difftime(stop,start));
}

void display(int* arr, int n){

    printf("\nARRAY: {");
    for(int i = 0; i<n; i++)
    {
        if(i != n - 1)
        {
            printf("%d, ", *(arr + i));        
        }
        else
            printf("%d}\n\n", *(arr + i));
    }
    
}

void selectionSort(int* A, int n)
{
    #pragma omp parallel for num_threads(2)
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
    #pragma barrier
}

void swap(int* a, int* b){
    int temp = *a;
    *a = *b;
    *b = temp;
}

![image](https://github.com/Murad-pitafi/OS_PROJECT/assets/87450783/e94b2532-1bed-4320-933e-aa9a342537d7)

</detaiks>
</blockquote>
</details>
