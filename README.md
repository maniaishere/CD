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

## POSTFIX / PREFIX - PY - M
```
output = []
operator = []
priority = { '(':0, '+':1, '-':1, '/':2, '*':2}

exp = input('Enter the expression')

for ch in exp:
    if ch == '(':
        operator.append(ch)
    
    elif ch == ')':
        while operator[-1] != '(':
            ele = operator.pop()
            output.append(ele)
        operator.pop()
        
    elif ch == '+' or ch == '-' or ch == '/' or ch == '*' :
        if len(operator) > 0:
            while priority[operator[-1]] > priority[ch]:
                ele = operator.pop()
                output.append(ele)
                if len(operator) == 0:
                    break
        operator.append(ch)
    else :
        output.append(ch)
        
if len(operator) > 0:
    while len(operator) != 0:
        ele = operator.pop()
        output.append(ele)
        
print('Postfix Exp', end = '\n')
for ch in output:
    print(ch, end='')

//-----------------------------
output = []
operator = []
priority = { ')':0, '+':1, '-':1, '/':2, '*':2}

exp = input('Enter the expression')

for ch in exp[::-1]:
    if ch == ')':
        operator.append(ch)
    
    elif ch == '(':
        while operator[-1] != ')':
            ele = operator.pop()
            output.append(ele)
        operator.pop()
        
    elif ch == '+' or ch == '-' or ch == '/' or ch == '*' :
        if len(operator) > 0:
            while priority[operator[-1]] > priority[ch]:
                ele = operator.pop()
                output.append(ele)
                if len(operator) == 0:
                    break
        operator.append(ch)
    else :
        output.append(ch)
        
if len(operator) > 0:
    while len(operator) != 0:
        ele = operator.pop()
        output.append(ele)
        
print('Prefix Exp', end = '\n')
for ch in output[::-1]:
    print(ch, end='')

```

## POSTFIX / PREFIX - PY - G
```
OPERATORS = set(['+', '-', '*', '/', '(', ')'])

PRI = {'+': 1, '-': 1, '*': 2, '/': 2}
def infix_to_postfix(formula):
    stack = []  # only pop when the coming op has priority

    output = ''

    for ch in formula:

        if ch not in OPERATORS:

            output += ch

        elif ch == '(':

            stack.append('(')

        elif ch == ')':

            while stack and stack[-1] != '(':
                output += stack.pop()

            stack.pop()  # pop '('

        else:

            while stack and stack[-1] != '(' and PRI[ch] <= PRI[stack[-1]]:
                output += stack.pop()

            stack.append(ch)

            # leftover

    while stack:
        output += stack.pop()

    print(f'POSTFIX: {output}')

    return output


### INFIX ===> PREFIX ###

def infix_to_prefix(formula):
    op_stack = []

    exp_stack = []

    for ch in formula:

        if not ch in OPERATORS:

            exp_stack.append(ch)

        elif ch == '(':

            op_stack.append(ch)

        elif ch == ')':

            while op_stack[-1] != '(':
                op = op_stack.pop()

                a = exp_stack.pop()

                b = exp_stack.pop()

                exp_stack.append(op + b + a)

            op_stack.pop()  # pop '('

        else:

            while op_stack and op_stack[-1] != '(' and PRI[ch] <= PRI[op_stack[-1]]:
                op = op_stack.pop()

                a = exp_stack.pop()

                b = exp_stack.pop()

                exp_stack.append(op + b + a)

            op_stack.append(ch)

            # leftover

    while op_stack:
        op = op_stack.pop()

        a = exp_stack.pop()

        b = exp_stack.pop()

        exp_stack.append(op + b + a)

    print(f'PREFIX: {exp_stack[-1]}')

    return exp_stack[-1]

expres = input("INPUT THE EXPRESSION: ")

pre = infix_to_prefix(expres)

pos = infix_to_postfix(expres)
```

