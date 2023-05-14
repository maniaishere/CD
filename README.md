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

## NFA-DFA CPP
```
#include <iostream>
#include <vector>
#include <queue>
#include <set>
#include <algorithm>


using namespace std;
void print(vector<vector<vector<int> > > table)
{
    cout << "  STATE/INPUT  |";
    char a = 'a';
    for (int i = 0; i < table[0].size() - 1; i++)
    {
        cout << "   " << a++ << "   |";
    }
    cout << "   ^   " << endl
         << endl;
    for (int i = 0; i < table.size(); i++)
    {
        cout << "       " << i << "      ";
        for (int j = 0; j < table[i].size(); j++)
        {
            cout << " | ";
            for (int k = 0; k < table[i][j].size(); k++)
            {
                cout << table[i][j][k] << " ";
            }
        }
        cout << endl;
    }
}

void printdfa(vector<vector<int> > states, vector<vector<vector<int> > > dfa)
{
    cout << "  STATE/INPUT  ";
    char a = 'a';
    for (int i = 0; i < dfa[0].size(); i++)
    {
        cout << "|   " << a++ << "   ";
    }
    cout << endl;
    for (int i = 0; i < states.size(); i++)
    {
        cout << "{ ";
        for (int h = 0; h < states[i].size(); h++)
            cout << states[i][h] << " ";
        if (states[i].empty())
        {
            cout << "^ ";
        }
        cout << "} ";
        for (int j = 0; j < dfa[i].size(); j++)
        {
            cout << " | ";
            for (int k = 0; k < dfa[i][j].size(); k++)
            {
                cout << dfa[i][j][k] << " ";
            }
            if (dfa[i][j].empty())
            {
                cout << "^ ";
            }
        }
        cout << endl;
    }
}
vector<int> closure(int s, vector<vector<vector<int> > > v)
{
    vector<int> t;
    queue<int> q;
    t.push_back(s);
    int a = v[s][v[s].size() - 1].size();
    for (int i = 0; i < a; i++)
    {
        t.push_back(v[s][v[s].size() - 1][i]);
        // cout<<"t[i]"<<t[i]<<endl;
        q.push(t[i]);
    }
    while (!q.empty())
    {
        int f = q.front();
        q.pop();
        if (!v[f][v[f].size() - 1].empty())
        {
            int u = v[f][v[f].size() - 1].size();
            for (int i = 0; i < u; i++)
            {
                int y = v[f][v[f].size() - 1][i];
                if (find(t.begin(), t.end(), y) == t.end())
                {
                    // cout<<"y"<<y<<endl;
                    t.push_back(y);
                    q.push(y);
                }
            }
        }
    }
    return t;
}
int main()
{
    int n, alpha;
    cout << "************************* NFA to DFA *************************" << endl
         << endl;
    cout << "Enter total number of states in NFA : ";
    cin >> n;
    cout << "Enter number of elements in alphabet : ";
    cin >> alpha;
    vector<vector<vector<int> > > table;
    for (int i = 0; i < n; i++)
    {
        cout << "For state " << i << endl;
        vector<vector<int> > v;
        char a = 'a';
        int y, yn;
        for (int j = 0; j < alpha; j++)
        {
            vector<int> t;
            cout << "Enter no. of output states for input " << a++ << " : ";
            cin >> yn;
            cout << "Enter output states :" << endl;
            for (int k = 0; k < yn; k++)
            {
                cin >> y;
                t.push_back(y);
            }
            v.push_back(t);
        }
        vector<int> t;
        cout << "Enter no. of output states for input ^ : ";
        cin >> yn;
        cout << "Enter output states :" << endl;
        for (int k = 0; k < yn; k++)
        {
            cin >> y;
            t.push_back(y);
        }
        v.push_back(t);
        table.push_back(v);
    }
    cout << "***** TRANSITION TABLE OF NFA *****" << endl;
    print(table);
    cout << endl
         << "***** TRANSITION TABLE OF DFA *****" << endl;
    vector<vector<vector<int> > > dfa;
    vector<vector<int> > states;
    states.push_back(closure(0, table));
    queue<vector<int> > q;
    q.push(states[0]);
    while (!q.empty())
    {
        vector<int> f = q.front();
        q.pop();
        vector<vector<int> > v;
        for (int i = 0; i < alpha; i++)
        {
            vector<int> t;
            set<int> s;
            for (int j = 0; j < f.size(); j++)
            {

                for (int k = 0; k < table[f[j]][i].size(); k++)
                {
                    vector<int> cl = closure(table[f[j]][i][k], table);
                    for (int h = 0; h < cl.size(); h++)
                    {
                        if (s.find(cl[h]) == s.end())
                            s.insert(cl[h]);
                    }
                }
            }
            for (set<int>::iterator u = s.begin(); u != s.end(); u++)
                t.push_back(*u);
            v.push_back(t);
            if (find(states.begin(), states.end(), t) == states.end())
            {
                states.push_back(t);
                q.push(t);
            }
        }
        dfa.push_back(v);
    }
    printdfa(states, dfa);
}

// Sample Input

// Enter total number of states in NFA : 3
// Enter number of elements in alphabet : 2
// For state 0
// Enter no. of output states for input a : 2
// Enter output states :
// 1 2
// Enter no. of output states for input b : 1
// Enter output states :
// 0
// Enter no. of output states for input ^ : 1
// Enter output states :
// 1
// For state 1
// Enter no. of output states for input a : 1
// Enter output states :
// 2
// Enter no. of output states for input b : 1
// Enter output states :

// 2
// Enter no. of output states for input ^ : 2
// Enter output states :
// 0 1
// For state 2
// Enter no. of output states for input a : 1
// Enter output states :
// 1
// Enter no. of output states for input b : 2
// Enter output states :
// 1 2
// Enter no. of output states for input ^ : 1
// Enter output states :
// 2
```

## SHIFT REDUCE
```
gram = {
	"E":["E*E","E+E","i"]
}
starting_terminal = "E"
inp = "i+i*i$"

stack = "$"
print(f'{"Stack": <15}'+"|"+f'{"Input Buffer": <15}'+"|"+f'Parsing Action')
print(f'{"-":-<50}')

while True:
	action = True
	i = 0
	while i<len(gram[starting_terminal]):
		if gram[starting_terminal][i] in stack:
			stack = stack.replace(gram[starting_terminal][i],starting_terminal)
			print(f'{stack: <15}'+"|"+f'{inp: <15}'+"|"+f'Reduce S->{gram[starting_terminal][i]}')
			i=-1
			action = False
		i+=1
	if len(inp)>1:
		stack+=inp[0]
		inp=inp[1:]
		print(f'{stack: <15}'+"|"+f'{inp: <15}'+"|"+f'Shift')
		action = False

	if inp == "$" and stack == ("$"+starting_terminal):
		print(f'{stack: <15}'+"|"+f'{inp: <15}'+"|"+f'Accepted')
		break

	if action:
		print(f'{stack: <15}'+"|"+f'{inp: <15}'+"|"+f'Rejected')
		break
```
## RECURSION
```
#include <iostream>
#include <vector>
#include <string>
using namespace std;

int main()
{
    int n;
    cout<<"\nEnter number of non terminals: ";
    cin>>n;
    cout<<"\nEnter non terminals one by one: ";
    int i;
    vector<string> nonter(n);
    vector<int> leftrecr(n,0);
    for(i=0;i<n;++i) {
            cout<<"\nNon terminal "<<i+1<<" : ";
        cin>>nonter[i];
    }
    vector<vector<string> > prod;
    cout<<"\nEnter 'esp' for null";
    for(i=0;i<n;++i) {
        cout<<"\nNumber of "<<nonter[i]<<" productions: ";
        int k;
        cin>>k;
        int j;
        cout<<"\nOne by one enter all "<<nonter[i]<<" productions";
        vector<string> temp(k);
        for(j=0;j<k;++j) {
            cout<<"\nRHS of production "<<j+1<<": ";
            string abc;
            cin>>abc;
            temp[j]=abc;
            if(nonter[i].length()<=abc.length()&&nonter[i].compare(abc.substr(0,nonter[i].length()))==0)
                leftrecr[i]=1;
        }
        prod.push_back(temp);
    }
    for(i=0;i<n;++i) {
        cout<<leftrecr[i];
    }
    for(i=0;i<n;++i) {
        if(leftrecr[i]==0)
            continue;
        int j;
        nonter.push_back(nonter[i]+"'");
        vector<string> temp;
        for(j=0;j<prod[i].size();++j) {
            if(nonter[i].length()<=prod[i][j].length()&&nonter[i].compare(prod[i][j].substr(0,nonter[i].length()))==0) {
                string abc=prod[i][j].substr(nonter[i].length(),prod[i][j].length()-nonter[i].length())+nonter[i]+"'";
                temp.push_back(abc);
                prod[i].erase(prod[i].begin()+j);
                --j;
            }
            else {
                prod[i][j]+=nonter[i]+"'";
            }
        }
        temp.push_back("esp");
        prod.push_back(temp);
    }
    cout<<"\n\n";
    cout<<"\nNew set of non-terminals: ";
    for(i=0;i<nonter.size();++i)
        cout<<nonter[i]<<" ";
    cout<<"\n\nNew set of productions: ";
    for(i=0;i<nonter.size();++i) {
        int j;
        for(j=0;j<prod[i].size();++j) {
            cout<<"\n"<<nonter[i]<<" -> "<<prod[i][j];
        }
    }
    return 0;
}
```
