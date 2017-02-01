#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <math.h>
 
#define TRUE 1
#define FALSE 0
#define K 7  // K Value
#define TrainingData_Size 100
#define N_Label 3 // Label의 갯수
#define S 1 // Smoothing Parameter(0이 되지 않게 더해주는 값)
 
void Initial_TrainingData(int T[][N_Label]);
void Computing_Prior_Probabilities(int T[][N_Label], float P[][2]);
void Initial_Label_Sum(float LS[][2]);
void Comparison_Each(int T[][N_Label], int num, float CS[][2]);
void Sort_Check_Score(float CS[][2]);
void Find_Neighbors(float CS[][2], int N[]);
void Count_Label(float LS[][2], int N[], int T[][N_Label]);
void Computing_Posterior_Probabilities(float LS[][2], int L, float P[][2]);
void Comparison_Sample(int T[][N_Label], float R[], float CS[][2]);
 
void main()
{
 char stop;/* 정지를 위한 값 */
 int Training[TrainingData_Size][N_Label], Neighbor[K];
 int i, l, j, seed;
 float Prior_P[N_Label][2], Label_Sum[N_Label][2], Check_Score[TrainingData_Size][2], Posterior_P[N_Label][2], Real_Value[N_Label], Real_P[N_Label];
 
 Initial_TrainingData(Training);
 
 // Computing the prior probabilities
 Computing_Prior_Probabilities(Training, Prior_P);
 
 // Computing the posterior probabilities
 for(l = 0; l < N_Label; l++)
 {
  Initial_Label_Sum(Label_Sum);
 
  for(i = 0; i < TrainingData_Size; i++)
  {
   Comparison_Each(Training, i, Check_Score);
   Sort_Check_Score(Check_Score);
   Find_Neighbors(Check_Score, Neighbor);
   Count_Label(Label_Sum, Neighbor, Training);
  }
  Computing_Posterior_Probabilities(Label_Sum, l, Posterior_P);
 }
 printf("\n");
 
 //Computing probabilities of real-valued vector
 for(l = 0; l < N_Label; l++)
 {
  seed = time(NULL);
  srand(seed+l);
  Real_Value[l] = rand()%2;
  printf("Real_Value[%d] = %.0f \t", l+1, Real_Value[l]);
 }
 printf("\n");
 
 Comparison_Sample(Training, Real_Value, Check_Score);
 Sort_Check_Score(Check_Score);
 Find_Neighbors(Check_Score, Neighbor);
 Count_Label(Label_Sum, Neighbor, Training);
 
 for(l = 0; l < N_Label; l++)
 {
  Computing_Posterior_Probabilities(Label_Sum, l, Posterior_P);
 }
 printf("\n");
 
 for(i = 0; i < N_Label; i++)
 {
  Real_P[i] = ( Posterior_P[i][TRUE] ) / ( Posterior_P[i][TRUE] + Posterior_P[i][FALSE]);
  printf("Real_P[%d] = %f \n", i+1, Real_P[i]);
 }
 
 scanf("%c",&stop);
}
 
/* Training Data 무작위로 생성 */
void Initial_TrainingData(int T[][N_Label])
{
 int i, j, seed;
 
 for(i = 0; i < TrainingData_Size; i++)
 {
  for(j = 0; j < N_Label; j++)
  {
   seed = time(NULL);
   srand(seed+i+j);
   T[i][j] = rand()%2;
  }
 }
 
}
 
/* 사전 확률을 계산 */
void Computing_Prior_Probabilities(int T[][N_Label], float P[][2])
{
 int i , j;
 float Label_Sum_Temp = 0;
 
 for(i = 0; i < N_Label; i++)
 {
  for (j = 0; j < TrainingData_Size; j++)
  {
   if(T[i][j] == 1)
    Label_Sum_Temp++;
  }
  P[i][TRUE] =  (S + Label_Sum_Temp) / (S + TrainingData_Size);
  P[i][FALSE] = 1 - P[i][TRUE];
  Label_Sum_Temp = 0;
  printf("Prior_P[%d][TRUE] = %f \t Prior_P[%d][FALSE] = %f \n",i+1, P[i][TRUE], i+1, P[i][FALSE]);
 }
 printf("\n");
}
 
/* 총 Label 카운트 초기화 */
void Initial_Label_Sum(float LS[][2])
{
 int i;
 
 for(i = 0; i < N_Label; i++)
 {
  LS[i][TRUE] = 0;
  LS[i][FALSE] = 0;
 }
}
 
 
/* 해당 값과 차이를 비교 */
void Comparison_Each(int T[][N_Label], int num, float CS[][2])
{
 int j, k, conform, non_conform;
 float ;
 
 for(j = 0; j < TrainingData_Size; j++)
  {
   conform = 0;
   non_conform = 0;
   for(k = 0; k < N_Label; k++)
   {
    if(num == j)
     conform = -100;
    else
    {
     if(T[num][k] == T[j][k])
      conform++;
     else
      non_conform++;
    }
   }
   CS[j][0] = conform * 1.0 + non_conform * -1;
   CS[j][1] = j+1;
  }
}
 
/* Real Value와 Training 값을 비교 */
void Comparison_Sample(int T[][N_Label], float R[], float CS[][2])
{
 int j, k, conform, non_conform;
 float ;
 
 for(j = 0; j < TrainingData_Size; j++)
 {
  conform = 0;
  non_conform = 0;
  for(k = 0; k < N_Label; k++)
  {
   if(R[k] == T[j][k])
    conform++;
   else
    non_conform++;
  }
  CS[j][0] = conform * 1.0 + non_conform * -1.0;
  CS[j][1] = j+1;
 }
  
}
 
/*  각의 거리들을 내림 차순으로 정렬  */
void Sort_Check_Score(float CS[][2])
{
 int i, j, k, temp = 0;
 
 for(i = 0; i < TrainingData_Size-1; i++)
 {
  for(j = 0; j < TrainingData_Size-1; j++)
  {
   if(CS[j][0] < CS[j+1][0])
   {
    for(k = 0; k < 2; k++)
    {
     temp = CS[j][k];
     CS[j][k] = CS[j+1][k];
     CS[j+1][k] = temp;
    }
   }
  }
 }
 
}
 
 
/* 해당 값의 Neighbors를 찾는다 */
void Find_Neighbors(float CS[][2], int N[])
{
 int i;
 
 for(i = 0; i < K; i++)
 {
  N[i] = CS[i][1];
 }
}
 
/* L번째 Label의 횟수를 계산 */
void Count_Label(float LS[][2], int N[], int T[][N_Label])
{
 int i, j;
 
 for(i = 0; i < K; i++)
 {
  for(j = 0; j < N_Label; j++)
  {
   if(T[N[i]][j] == TRUE)
    LS[j][TRUE]++;
   else
    LS[j][FALSE]++;
  }
   
 }
}
 
/* L번째 Label의 사후 확률 계산 */
void Computing_Posterior_Probabilities(float LS[][2], int L, float P[][2])
{
 int Total_Label_Count[2], i;
 
 Total_Label_Count[TRUE] = 0;
 Total_Label_Count[FALSE] = 0;
 
 for(i = 0; i < N_Label; i++)
 {
  Total_Label_Count[TRUE] += LS[i][TRUE];
  Total_Label_Count[FALSE] += LS[i][FALSE];
 }
 
 P[L][TRUE] = (S + LS[L][TRUE]) / (S * (N_Label+1) + Total_Label_Count[TRUE]);
 P[L][FALSE] = (S + LS[L][FALSE]) / (S * (N_Label+1) + Total_Label_Count[FALSE]);
 
 printf("Poste_P[%d][TRUE] = %f \t Poste_P[%d][FALSE] = %f \n", L+1, P[L][TRUE], L+1, P[L][FALSE]);
}
