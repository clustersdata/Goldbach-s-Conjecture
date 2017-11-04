# Goldbach-s-Conjecture

Goldbach's Conjecture

Description

In 1742, Christian Goldbach, a German amateur mathematician, sent a letter to Leonhard Euler in which he made the following conjecture:
Every even number greater than 4 can be
written as the sum of two odd prime numbers.


For example:

8 = 3 + 5. Both 3 and 5 are odd prime numbers.

20 = 3 + 17 = 7 + 13.

42 = 5 + 37 = 11 + 31 = 13 + 29 = 19 + 23.


Today it is still unproven whether the conjecture is right. (Oh wait, I have the proof of course, but it is too long to write it on the margin of this page.)

Anyway, your task is now to verify Goldbach's conjecture for all even numbers less than a million.

Input

The input will contain one or more test cases.

Each test case consists of one even integer n with 6 <= n < 1000000.

Input will be terminated by a value of 0 for n.

Output

For each test case, print one line of the form n = a + b, where a and b are odd primes. Numbers and operators should be separated by exactly one blank like in the sample output below. If there is more than one pair of odd primes adding up to n, choose the pair where the difference b - a is maximized. If there is no such pair, print a line saying "Goldbach's conjecture is wrong."

Sample Input

8

20

42

0

Sample Output

8 = 3 + 5

20 = 3 + 17

42 = 5 + 37

对于任何一个偶数 n，从 x = 1 和 y = n - 1 开始，看 x、y 是否是质数，不是则 x += 2, y += 2  
这题需要开很大的内存。

#include <iostream>  
  
#include <stdio.h>    

#include <math.h> 

#include <stack>  
  
#include <memory.h>   

#include <string.h>    
  
using namespace std; 

  
// 判断是否为质数的函数  

int IsPrime ( int x )  

{  

    int i; 
    
    if( x < 2 )  
    
        return 0;  
        
     
    for( i = 2; i <= (int) ( sqrt( (double)x + 0.5 ) ); i++ )      
    
        if( x % i == 0)         
        
            return   0;        
            
    return   1;    
    
}   

  
int main() 

{   

    // 输入数，输入数 / 2 向上延伸，输入数 / 2 向下延伸，输入数 / 2    
    
    int m_Input, m_Num_Max, m_Num_Min, m_InputToTwo;  
    
    // 总体输出   
    
    char m_Output[ 1000000 ];   
    
    memset( m_Output, 0, 1000000 );  
    
    // 标识 m_Output 的 Pos    
    
    int m_Output_Pos = 0;     
    
    // 是否找到标识    
    
    bool b_Find; 
    
    // 栈    
    
    stack<int> m_Stack;    
    
    // 临时数    
    
    int m_Value_Top;   
    
    // 循环输入    
    
    while ( scanf( "%d", &m_Input ) && ( m_Input != 0 ) )   
    
    {   
    
        b_Find = true;   
        
        // m_Input 肯定是一个偶数   
        
        m_InputToTwo = m_Input / 2;  
        
        // 置值    
        
        m_Num_Max = m_Input - 1;  
        
        m_Num_Min = 1;   
        
        // 寻找，直至都为素数 或者 找不到 为止  
        
        while ( ( !IsPrime( m_Num_Max ) ) || ( !IsPrime( m_Num_Min ) ) )  
        
        {          
        
             // 否则，前进 & 后退 2 格    
             
             m_Num_Max -= 2;   
             
             m_Num_Min += 2;   
             
             // 如果发生如下情况，则输出 "Goldbach's conjecture is wrong."     
             
             if ( ( m_Num_Max < m_InputToTwo ) || ( m_Num_Min > m_InputToTwo ) )   
             
             {   
             
                  char* m_TempChar = "Goldbach's conjecture is wrong.\n";   
                  
                  strcat( m_Output, m_TempChar );   
                  
                  b_Find = false;   
                  
                  m_Output_Pos += strlen( m_TempChar );   
                  
                  break;   
                  
             }   
             
        }   
        
        // 如果找到了  
        
        if ( b_Find ) 
        
        {   
        
            // 将 m_Input 转换为字符串存入 m_Output   
            
            while ( m_Input != 0 )  
            
            {   
            
            
                m_Value_Top = m_Input % 10;  
                
                m_Stack.push( m_Value_Top ); 
                
                m_Input /= 10;   
                
            }   
            
            while ( !m_Stack.empty() )  
            
            {   
            
                m_Value_Top = m_Stack.top();   
                
                m_Output[ m_Output_Pos++ ] = m_Value_Top + 48;   
                
                m_Stack.pop();   
                
            }   
            
            // 加入 =    
            
            m_Output[ m_Output_Pos++ ] = ' ';   
            
            m_Output[ m_Output_Pos++ ] = '=';  
            
            m_Output[ m_Output_Pos++ ] = ' ';  
            
            // 将 m_Num_Min 转换为字符串存入 m_Output    
            
            while ( m_Num_Min != 0 )   
            
            {   
            
            
                m_Value_Top = m_Num_Min % 10;  
                
                m_Stack.push( m_Value_Top );   
                
                m_Num_Min /= 10;   
                
            }   
            
            while ( !m_Stack.empty() )   
            
            {  
            
                m_Value_Top = m_Stack.top();  
                
                m_Output[ m_Output_Pos++ ] = m_Value_Top + 48;
                
                m_Stack.pop();   
                
            }   
            
            // 加入 + 
            
            m_Output[ m_Output_Pos++ ] = ' ';   
            
            m_Output[ m_Output_Pos++ ] = '+';
            
            m_Output[ m_Output_Pos++ ] = ' '; 
            
            // 将 m_Num_Max 转换为字符串存入 m_Output  
            
            while ( m_Num_Max != 0 )   
            
            {   
            
                m_Value_Top = m_Num_Max % 10;   
                
                m_Stack.push( m_Value_Top );
                
                m_Num_Max /= 10;   
                
            }     
            
            while ( !m_Stack.empty() )   
            
            {   
            
                m_Value_Top = m_Stack.top();  
                
                m_Output[ m_Output_Pos++ ] = m_Value_Top + 48;   
                
                m_Stack.pop();   
                
            }     
            
            // 加入 \n  
            
            m_Output[ m_Output_Pos++ ] = '\n';   
            
        }   
        
    }    
    
    // 输出   
    
    printf( "%s", m_Output );  
    
    system( "pause" );   
    
    return 0;   
    
}  
