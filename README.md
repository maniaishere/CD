## LEXICAL ANALYZER PY
```
file  = open("./add.c", 'r')
lines = file.readlines()

keywords    = ["void", "main", "int", "float", "bool", "if", "for", "else", "while", "char", "return"]
operators   = ["=", "==", "+", "-", "*", "/", "++", "--", "+=", "-=", "!=", "||", "&&"]
punctuations= [";", "(", ")", "{", "}", "[", "]"]

def is_int(x):
    try:
        int(x)
        return True
    except:
        return False

for line in lines:
    for i in line.strip().split(" "):
        if i in keywords:
            print (i, " is a keyword")
        elif i in operators:
            print (i, " is an operator")
        elif i in punctuations:
            print (i, " is a punctuation")
        elif is_int(i):
            print (i, " is a number")
        else:
            print (i, " is an identifier")
```

## RE-NFA C/CPP
```
#include<stdio.h>
#include<string.h>
int main()
{
	char reg[20]; int q[20][3],i=0,j=1,len,a,b;
	for(a=0;a<20;a++) for(b=0;b<3;b++) q[a][b]=0;
	scanf("%s",reg);
	printf("Given regular expression: %s\n",reg);
	len=strlen(reg);
	while(i<len)
	{
		if(reg[i]=='a'&&reg[i+1]!='|'&&reg[i+1]!='*') { 
		    q[j][0]=j+1; j++; 
		}
		
		if(reg[i]=='b'&&reg[i+1]!='|'&&reg[i+1]!='*') {	
		    q[j][1]=j+1; j++;	
		}
		
		if(reg[i]=='e'&&reg[i+1]!='|'&&reg[i+1]!='*') {	
		    q[j][2]=j+1; j++;	
		}
		
		
		if(reg[i]=='a'&&reg[i+1]=='|'&&reg[i+2]=='b') 
		{ 
		  q[j][2]=((j+1)*10)+(j+3); j++; 
		  q[j][0]=j+1; j++;
		  q[j][2]=j+3; j++;
		  q[j][1]=j+1; j++;
		  q[j][2]=j+1; j++;
		  i=i+2;
		}
		
		if(reg[i]=='b'&&reg[i+1]=='|'&&reg[i+2]=='a')
		{
			q[j][2]=((j+1)*10)+(j+3); j++;
			q[j][1]=j+1; j++;
			q[j][2]=j+3; j++;
			q[j][0]=j+1; j++;
			q[j][2]=j+1; j++;
			i=i+2;
		}
		
		if(reg[i]=='a'&&reg[i+1]=='*')
		{
			q[j][2]=((j+1)*10)+(j+3); j++;
			q[j][0]=j+1; j++;
			q[j][2]=((j+1)*10)+(j-1); j++;
		}
		
		if(reg[i]=='b'&&reg[i+1]=='*'){
			q[j][2]=((j+1)*10)+(j+3); j++;
			q[j][1]=j+1; j++;
			q[j][2]=((j+1)*10)+(j-1); j++;
		}
		
		if(reg[i]==')'&&reg[i+1]=='*')
		{
			q[0][2]=((j+1)*10)+1;
			q[j][2]=((j+1)*10)+1;
			j++;
		}
		i++;
	}
	
	
	printf("\n\tTransition Table \n");
	printf("_____________\n");
	printf("Current State |\tInput |\tNext State");
	printf("\n_____________\n");
	
	for(i=0;i<=j;i++)
	{
		if(q[i][0]!=0) 
		printf("\n  q[%d]\t      |   a   |  q[%d]",i,q[i][0]);
		
		if(q[i][1]!=0) 
		printf("\n  q[%d]\t      |   b   |  q[%d]",i,q[i][1]);
		
		
		if(q[i][2]!=0) 
		{
			if(q[i][2]<10) printf("\n  q[%d]\t      |   e   |  q[%d]",i,q[i][2]);
			else printf("\n  q[%d]\t      |   e   |  q[%d] , q[%d]",i,q[i][2]/10,q[i][2]%10);
		}
	}
	printf("\n_____________\n");
	return 0;
}
```
