#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <math.h>
 
#define Sample_Size 5  //샘플들의 사이즈
#define TrainingA_Size 30  //그룹 A의 사이즈
#define TrainingB_Size 20  //그룹 B의 사이즈
#define Total TrainingA_Size + TrainingB_Size // Training 전체 사이즈
#define K 3  // K Value
#define Range 50 //랜덤 값 범위
#define Gap 10  // 그룹 간 차이
 
void Initial_Matrix(int S[][2],int Ga[][2], int Gb[][2]);
void Compare_Matrix(int S[2],int Ga[][2], int Gb[][2], int D[][2]);
void Sort_Matrix(int D[][2]);
int K_Classfication(int D[][2]);
 
void main()
{
 char stop;/* 정지를 위한 값 */
 
 int Sample[Sample_Size][2], Group_A[TrainingA_Size][2], Group_B[TrainingB_Size][2], Dist[Total][2];
 int i;
 
 Initial_Matrix(Sample, Group_A, Group_B); 
 
 for(i = 0; i < Sample_Size; i++)
 {
  Compare_Matrix(Sample[i], Group_A, Group_B, Dist); 
 
  Sort_Matrix(Dist); 
  
  if(K_Classfication(Dist) == 0)
   printf("Sample :(%2d,%2d) ===> Grourp A \n", Sample[i][0], Sample[i][1]);
  else
   printf("Sample :(%2d,%2d) ===> \t\tGrourp B \n", Sample[i][0], Sample[i][1]);
 }
 
 scanf("%c",&stop);
}
 
/*  배열 초기화  */
void Initial_Matrix(int S[][2],int Ga[][2], int Gb[][2])
{
 int i, j;
 long seed;
 
 seed = time(NULL);
 srand(seed); 
 
 for(i = 0; i < Sample_Size; i++)
 {// 샘플 초기화
  for( j = 0; j < 2; j++)
  {
   S[i][j] = rand()%Range;
   
  }
 }
 
 
 for(i = 0; i < TrainingA_Size; i++)
 {// 그룹 A 초기화
  for( j = 0; j < 2; j++)
  {
   Ga[i][j] = rand()%Range - Gap;
   
  }
 }
 
 for(i = 0; i < TrainingB_Size; i++)
 {// 그룹 B 초기화
  for( j = 0; j < 2; j++)
  {
   Gb[i][j] = rand()%Range + Gap;
  }
 }
  
}
 
/*   샘플 값과 그룹 A & B와 비교  */
void Compare_Matrix(int S[2],int Ga[][2], int Gb[][2], int D[][2])
{
 int i, j, flag;
 int distance = 0, num = 0;
 
 flag = 0;
 for(i = 0; i < TrainingA_Size; i++)
 {// 그룹 A와의 거리
  for(j = 0; j < 2; j++)
  {
   distance += pow((S[j] - Ga[i][j]), 2);
  }
  D[num][0] = distance;
  D[num++][1] = flag;
  distance = 0;
 }
 
 flag = 1;
 for(i = 0; i < TrainingB_Size; i++)
 {// 그룹 B와의 거리
  
  for(j = 0; j < 2; j++)
  {
   distance += pow((S[j] - Gb[i][j]), 2);
  }
  D[num][0] = distance;
  D[num++][1] = flag;
  distance = 0;
 }
}
 
 
/*  각의 거리들을 오름 차순으로 정렬  */
void Sort_Matrix(int D[][2])
{
 int i, j, k, temp = 0;
 
 for(i = 0; i < Total-1; i++)
 {
  for(j = 0; j < Total-1; j++)
  {
   if(D[j][0] > D[j+1][0])
   {
    for(k = 0; k < 2; k++)
    {
     temp = D[j][k];
     D[j][k] = D[j+1][k];
     D[j+1][k] = temp;
    }
   }
  }
 }
 
}
 
/*  인접한 K개와 비교 후 다수결로 그룹을 분류  */
int K_Classfication(int D[][2])
{
 int i, A_count = 0, B_count = 0;
 
 for(i = 0; i < K; i++)
 {
  if(D[i][1] == 0)
   A_count++;
  else
   B_count++;
 }
 
 if(A_count > B_count)
  return 0;
 else
  return 1;
}
