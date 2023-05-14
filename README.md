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

## SHIFT REDUCE - PY
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
## RECURSION PY
```
gram = {
    "E":["E+T","T"],
    "T":["T*F","F"],
    "F":["(E)","i"]
}

def removeDirectLR(gramA, A):
	temp = gramA[A]
	tempCr = []
	tempInCr = []
	for i in temp:
		if i[0] == A:
			tempInCr.append(i[1:]+[A+"'"])
		else:
			tempCr.append(i+[A+"'"])
	tempInCr.append(["e"])
	gramA[A] = tempCr
	gramA[A+"'"] = tempInCr
	return gramA


def checkForIndirect(gramA, a, ai):
	if ai not in gramA:
		return False
	if a == ai:
		return True
	for i in gramA[ai]:
		if i[0] == ai:
			return False
		if i[0] in gramA:
			return checkForIndirect(gramA, a, i[0])
	return False

def rep(gramA, A):
	temp = gramA[A]
	newTemp = []
	for i in temp:
		if checkForIndirect(gramA, A, i[0]):
			t = []
			for k in gramA[i[0]]:
				t=[]
				t+=k
				t+=i[1:]
				newTemp.append(t)

		else:
			newTemp.append(i)
	gramA[A] = newTemp
	return gramA

def rem(gram):
	c = 1
	conv = {}
	gramA = {}
	revconv = {}
	for j in gram:
		conv[j] = "A"+str(c)
		gramA["A"+str(c)] = []
		c+=1

	for i in gram:
		for j in gram[i]:
			temp = []
			for k in j:
				if k in conv:
					temp.append(conv[k])
				else:
					temp.append(k)
			gramA[conv[i]].append(temp)

	for i in range(c-1,0,-1):
		ai = "A"+str(i)
		for j in range(0,i):
			aj = gramA[ai][0][0]
			if ai!=aj :
				if aj in gramA and checkForIndirect(gramA,ai,aj):
					gramA = rep(gramA, ai)

	for i in range(1,c):
		ai = "A"+str(i)
		for j in gramA[ai]:
			if ai==j[0]:
				gramA = removeDirectLR(gramA, ai)
				break

	op = {}
	for i in gramA:
		a = str(i)
		for j in conv:
			a = a.replace(conv[j],j)
		revconv[i] = a

	for i in gramA:
		l = []
		for j in gramA[i]:
			k = []
			for m in j:
				if m in revconv:
					k.append(m.replace(m,revconv[m]))
				else:
					k.append(m)
			l.append(k)
		op[revconv[i]] = l

	return op

result = rem(gram)

for i in result:
    print(f'{i}->{result[i]}')
```
## FACTORING - PY
```
from itertools import takewhile
def groupby(ls):
    d = {}
    ls = [ y[0] for y in rules ]
    initial = list(set(ls))
    for y in initial:
        for i in rules:
            if i.startswith(y):
                if y not in d:
                    d[y] = []
                d[y].append(i)
    return d

def prefix(x):
    return len(set(x)) == 1


starting=""
rules=[]
common=[]
alphabetset=["A'","B'","C'","D'","E'","F'","G'","H'","I'","J'","K'","L'","M'","N'","O'","P'","Q'","R'","S'","T'","U'","V'","W'","X'","Y'","Z'"]


s= "S->aSa|aSv"
while(True):
    rules=[]
    common=[]
    split=s.split("->")
    starting=split[0]
    for i in split[1].split("|"):
        rules.append(i)

    for k, l in groupby(rules).items():
        r = [l[0] for l in takewhile(prefix, zip(*l))]
        common.append(''.join(r))
    for i in common:
        newalphabet=alphabetset.pop()
        print(starting+"->"+i+newalphabet)
        index=[]
        for k in rules:
            if(k.startswith(i)):
                index.append(k)
        print(newalphabet+"->",end="")
        for j in index[:-1]:
            stringtoprint=j.replace(i,"", 1)+"|"
            if stringtoprint=="|":
                print("\u03B5","|",end="")
            else:
                print(j.replace(i,"", 1)+"|",end="")
        stringtoprint=index[-1].replace(i,"", 1)+"|"
        if stringtoprint=="|":
            print("\u03B5","",end="")
        else:
            print(index[-1].replace(i,"", 1)+"",end="")
        print("")
    break

```
## FIRST AND FOLLOW PY
```
import sys
sys.setrecursionlimit(60)

def first(string):
    #print("first({})".format(string))
    first_ = set()
    if string in non_terminals:
        alternatives = productions_dict[string]

        for alternative in alternatives:
            first_2 = first(alternative)
            first_ = first_ |first_2

    elif string in terminals:
        first_ = {string}

    elif string=='' or string=='@':
        first_ = {'@'}

    else:
        first_2 = first(string[0])
        if '@' in first_2:
            i = 1
            while '@' in first_2:
                #print("inside while")

                first_ = first_ | (first_2 - {'@'})
                #print('string[i:]=', string[i:])
                if string[i:] in terminals:
                    first_ = first_ | {string[i:]}
                    break
                elif string[i:] == '':
                    first_ = first_ | {'@'}
                    break
                first_2 = first(string[i:])
                first_ = first_ | first_2 - {'@'}
                i += 1
        else:
            first_ = first_ | first_2


    #print("returning for first({})".format(string),first_)
    return  first_


def follow(nT):
    #print("inside follow({})".format(nT))
    follow_ = set()
    #print("FOLLOW", FOLLOW)
    prods = productions_dict.items()
    if nT==starting_symbol:
        follow_ = follow_ | {'$'}
    for nt,rhs in prods:
        #print("nt to rhs", nt,rhs)
        for alt in rhs:
            for char in alt:
                if char==nT:
                    following_str = alt[alt.index(char) + 1:]
                    if following_str=='':
                        if nt==nT:
                            continue
                        else:
                            follow_ = follow_ | follow(nt)
                    else:
                        follow_2 = first(following_str)
                        if '@' in follow_2:
                            follow_ = follow_ | follow_2-{'@'}
                            follow_ = follow_ | follow(nt)
                        else:
                            follow_ = follow_ | follow_2
    #print("returning for follow({})".format(nT),follow_)
    return follow_





no_of_terminals=int(input("Enter no. of terminals: "))

terminals = []

print("Enter the terminals :")
for _ in range(no_of_terminals):
    terminals.append(input())

no_of_non_terminals=int(input("Enter no. of non terminals: "))

non_terminals = []

print("Enter the non terminals :")
for _ in range(no_of_non_terminals):
    non_terminals.append(input())

starting_symbol = input("Enter the starting symbol: ")

no_of_productions = int(input("Enter no of productions: "))

productions = []

print("Enter the productions:")
for _ in range(no_of_productions):
    productions.append(input())


#print("terminals", terminals)

#print("non terminals", non_terminals)

#print("productions",productions)


productions_dict = {}

for nT in non_terminals:
    productions_dict[nT] = []


#print("productions_dict",productions_dict)

for production in productions:
    nonterm_to_prod = production.split("->")
    alternatives = nonterm_to_prod[1].split("/")
    for alternative in alternatives:
        productions_dict[nonterm_to_prod[0]].append(alternative)

#print("productions_dict",productions_dict)

#print("nonterm_to_prod",nonterm_to_prod)
#print("alternatives",alternatives)


FIRST = {}
FOLLOW = {}

for non_terminal in non_terminals:
    FIRST[non_terminal] = set()

for non_terminal in non_terminals:
    FOLLOW[non_terminal] = set()

#print("FIRST",FIRST)

for non_terminal in non_terminals:
    FIRST[non_terminal] = FIRST[non_terminal] | first(non_terminal)

#print("FIRST",FIRST)


FOLLOW[starting_symbol] = FOLLOW[starting_symbol] | {'$'}
for non_terminal in non_terminals:
    FOLLOW[non_terminal] = FOLLOW[non_terminal] | follow(non_terminal)

#print("FOLLOW", FOLLOW)

print("{: ^20}{: ^20}{: ^20}".format('Non Terminals','First','Follow'))
for non_terminal in non_terminals:
    print("{: ^20}{: ^20}{: ^20}".format(non_terminal,str(FIRST[non_terminal]),str(FOLLOW[non_terminal])))
    
# Enter no. of terminals: 4
# Enter the terminals :
# a
# b
# c
# d
# Enter no. of non terminals: 2
# Enter the non terminals :
# S
# A
# Enter the starting symbol: S
# Enter no of productions: 3
# Enter the productions:
# S->Aa
# S->Ab
# A->c
```


## PREDICTIVE PARSING PY
```
gram = {
	"E":["E+T","T"],
	"T":["T*F","F"],
	"F":["(E)","i"],
    # "S":["CC"],
    # "C":["eC","d"],
}

def removeDirectLR(gramA, A):
	"""gramA is dictonary"""
	temp = gramA[A]
	tempCr = []
	tempInCr = []
	for i in temp:
		if i[0] == A:
			#tempInCr.append(i[1:])
			tempInCr.append(i[1:]+[A+"'"])
		else:
			#tempCr.append(i)
			tempCr.append(i+[A+"'"])
	tempInCr.append(["e"])
	gramA[A] = tempCr
	gramA[A+"'"] = tempInCr
	return gramA


def checkForIndirect(gramA, a, ai):
	if ai not in gramA:
		return False 
	if a == ai:
		return True
	for i in gramA[ai]:
		if i[0] == ai:
			return False
		if i[0] in gramA:
			return checkForIndirect(gramA, a, i[0])
	return False

def rep(gramA, A):
	temp = gramA[A]
	newTemp = []
	for i in temp:
		if checkForIndirect(gramA, A, i[0]):
			t = []
			for k in gramA[i[0]]:
				t=[]
				t+=k
				t+=i[1:]
				newTemp.append(t)

		else:
			newTemp.append(i)
	gramA[A] = newTemp
	return gramA

def rem(gram):
	c = 1
	conv = {}
	gramA = {}
	revconv = {}
	for j in gram:
		conv[j] = "A"+str(c)
		gramA["A"+str(c)] = []
		c+=1

	for i in gram:
		for j in gram[i]:
			temp = []	
			for k in j:
				if k in conv:
					temp.append(conv[k])
				else:
					temp.append(k)
			gramA[conv[i]].append(temp)


	#print(gramA)
	for i in range(c-1,0,-1):
		ai = "A"+str(i)
		for j in range(0,i):
			aj = gramA[ai][0][0]
			if ai!=aj :
				if aj in gramA and checkForIndirect(gramA,ai,aj):
					gramA = rep(gramA, ai)

	for i in range(1,c):
		ai = "A"+str(i)
		for j in gramA[ai]:
			if ai==j[0]:
				gramA = removeDirectLR(gramA, ai)
				break

	op = {}
	for i in gramA:
		a = str(i)
		for j in conv:
			a = a.replace(conv[j],j)
		revconv[i] = a

	for i in gramA:
		l = []
		for j in gramA[i]:
			k = []
			for m in j:
				if m in revconv:
					k.append(m.replace(m,revconv[m]))
				else:
					k.append(m)
			l.append(k)
		op[revconv[i]] = l

	return op

result = rem(gram)
terminals = []
for i in result:
	for j in result[i]:
		for k in j:
			if k not in result:
				terminals+=[k]
terminals = list(set(terminals))
#print(terminals)

def first(gram, term):
	a = []
	if term not in gram:
		return [term]
	for i in gram[term]:
		if i[0] not in gram:
			a.append(i[0])
		elif i[0] in gram:
			a += first(gram, i[0])
	return a

firsts = {}
for i in result:
	firsts[i] = first(result,i)
#	print(f'First({i}):',firsts[i])

def follow(gram, term):
	a = []
	for rule in gram:
		for i in gram[rule]:
			if term in i:
				temp = i
				indx = i.index(term)
				if indx+1!=len(i):
					if i[-1] in firsts:
						a+=firsts[i[-1]]
					else:
						a+=[i[-1]]
				else:
					a+=["e"]
				if rule != term and "e" in a:
					a+= follow(gram,rule)
	return a

follows = {}
for i in result:
	follows[i] = list(set(follow(result,i)))
	if "e" in follows[i]:
		follows[i].pop(follows[i].index("e"))
	follows[i]+=["$"]
#	print(f'Follow({i}):',follows[i])

resMod = {}
for i in result:
	l = []
	for j in result[i]:
		temp = ""
		for k in j:
			temp+=k
		l.append(temp)
	resMod[i] = l

# create predictive parsing table
tterm = list(terminals)
tterm.pop(tterm.index("e"))
tterm+=["d"]
pptable = {}
for i in result:
	for j in tterm:
		if j in firsts[i]:
			pptable[(i,j)]=resMod[i[0]][0]
		else:
			pptable[(i,j)]=""
	if "e" in firsts[i]:
		for j in tterm:
			if j in follows[i]:
				pptable[(i,j)]="e" 	
pptable[("F","i")] = "i"
toprint = f'{"": <10}'
for i in tterm:
	toprint+= f'|{i: <10}'
print(toprint)
for i in result:
	toprint = f'{i: <10}'
	for j in tterm:
		if pptable[(i,j)]!="":
			toprint+=f'|{i+"->"+pptable[(i,j)]: <10}'
		else:
			toprint+=f'|{pptable[(i,j)]: <10}'
	print(f'{"-":-<76}')
	print(toprint)
```

## LEADING TRAILING PY
```
  
a = ["E=E+T",
     "E=T",
     "T=T*F",
     "T=F",
     "F=(E)",
     "F=i"]

rules = {}
terms = []
for i in a:
    temp = i.split("=")
    terms.append(temp[0])
    try:
        rules[temp[0]] += [temp[1]]
    except:
        rules[temp[0]] = [temp[1]]

terms = list(set(terms))
print(rules,terms)

def leading(gram, rules, term, start):
    s = []
    if gram[0] not in terms:
        return gram[0]
    elif len(gram) == 1:
        return [0]
    elif gram[1] not in terms and gram[-1] is not start:
        for i in rules[gram[-1]]:
            s+= leading(i, rules, gram[-1], start)
            s+= [gram[1]]
        return s
    
def trailing(gram, rules, term, start):
    s = []
    if gram[-1] not in terms:
        return gram[-1]
    elif len(gram) == 1:
        return [0]
    elif gram[-2] not in terms and gram[-1] is not start:

        for i in rules[gram[-1]]:
            s+= trailing(i, rules, gram[-1], start)
            s+= [gram[-2]]
        return s

leads = {}
trails = {}
for i in terms:
    s = [0]
    for j in rules[i]:
        s+=leading(j,rules,i,i)
    s = set(s)
    s.remove(0)
    leads[i] = s
    s = [0]
    for j in rules[i]:
        s+=trailing(j,rules,i,i)
    s = set(s)
    s.remove(0)
    trails[i] = s

for i in terms:
    print("LEADING("+i+"):",leads[i])
for i in terms:
    print("TRAILING("+i+"):",trails[i])
```

## LR0 PY 
```
gram = {
	"S":["CC"],
	"C":["aC","d"]
}
start = "S"
terms = ["a","d","$"]

non_terms = []
for i in gram:
	non_terms.append(i)
gram["S'"]= [start]


new_row = {}
for i in terms+non_terms:
	new_row[i]=""


non_terms += ["S'"]
# each row in state table will be dictionary {nonterms ,term,$}
stateTable = []
# I = [(terminal, closure)]
# I = [("S","A.A")]

def Closure(term, I):
	if term in non_terms:
		for i in gram[term]:
			I+=[(term,"."+i)]
	I = list(set(I))
	for i in I:
		# print("." != i[1][-1],i[1][i[1].index(".")+1])
		if "." != i[1][-1] and i[1][i[1].index(".")+1] in non_terms and i[1][i[1].index(".")+1] != term:
			I += Closure(i[1][i[1].index(".")+1], [])
	return I

Is = []
Is+=set(Closure("S'", []))


countI = 0
omegaList = [set(Is)]
while countI<len(omegaList):
	newrow = dict(new_row)
	vars_in_I = []
	Is = omegaList[countI]
	countI+=1
	for i in Is:
		if i[1][-1]!=".":
			indx = i[1].index(".")
			vars_in_I+=[i[1][indx+1]]
	vars_in_I = list(set(vars_in_I))
	# print(vars_in_I)
	for i in vars_in_I:
		In = []
		for j in Is:
			if "."+i in j[1]:
				rep = j[1].replace("."+i,i+".")
				In+=[(j[0],rep)]
		if (In[0][1][-1]!="."):
			temp = set(Closure(i,In))
			if temp not in omegaList:
				omegaList.append(temp)
			if i in non_terms:
				newrow[i] = str(omegaList.index(temp))
			else:
				newrow[i] = "s"+str(omegaList.index(temp))
			print(f'Goto(I{countI-1},{i}):{temp} That is I{omegaList.index(temp)}')
		else:
			temp = set(In)
			if temp not in omegaList:
				omegaList.append(temp)
			if i in non_terms:
				newrow[i] = str(omegaList.index(temp))
			else:
				newrow[i] = "s"+str(omegaList.index(temp))
			print(f'Goto(I{countI-1},{i}):{temp} That is I{omegaList.index(temp)}')

	stateTable.append(newrow)
print("\n\nList of I's\n")
for i in omegaList:
	print(f'I{omegaList.index(i)}: {i}')


#populate replace elements in state Table
I0 = []
for i in list(omegaList[0]):
	I0 += [i[1].replace(".","")]
print(I0)

for i in omegaList:
	for j in i:
		if "." in j[1][-1]:
			if j[1][-2]=="S":
				stateTable[omegaList.index(i)]["$"] = "Accept"
				break
			for k in terms:
				stateTable[omegaList.index(i)][k] = "r"+str(I0.index(j[1].replace(".","")))
print("\nStateTable")

print(f'{" ": <9}',end="")
for i in new_row:
	print(f'|{i: <11}',end="")

print(f'\n{"-":-<66}')
for i in stateTable:
	print(f'{"I("+str(stateTable.index(i))+")": <9}',end="")
	for j in i:
		print(f'|{i[j]: <10}',end=" ")
	print()
```
## QUAD TRIPLE
```
OPERATORS = set(['+', '-', '*', '/', '(', ')'])
PRI = {'+':1, '-':1, '*':2, '/':2}

### INFIX ===> POSTFIX ###
def infix_to_postfix(formula):
    stack = [] # only pop when the coming op has priority 
    output = ''
    for ch in formula:
        if ch not in OPERATORS:
            output += ch
        elif ch == '(':
            stack.append('(')
        elif ch == ')':
            while stack and stack[-1] != '(':
                output += stack.pop()
            stack.pop() # pop '('
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
                exp_stack.append( op+b+a )
            op_stack.pop() # pop '('
        else:
            while op_stack and op_stack[-1] != '(' and PRI[ch] <= PRI[op_stack[-1]]:
                op = op_stack.pop()
                a = exp_stack.pop()
                b = exp_stack.pop()
                exp_stack.append( op+b+a )
            op_stack.append(ch)
    
    # leftover
    while op_stack:
        op = op_stack.pop()
        a = exp_stack.pop()
        b = exp_stack.pop()
        exp_stack.append( op+b+a )
    print(f'PREFIX: {exp_stack[-1]}')
    return exp_stack[-1]

### THREE ADDRESS CODE GENERATION ###
def generate3AC(pos):
	print("### THREE ADDRESS CODE GENERATION ###")
	exp_stack = []
	t = 1
	
	for i in pos:
		if i not in OPERATORS:
			exp_stack.append(i)
		else:
			print(f't{t} := {exp_stack[-2]} {i} {exp_stack[-1]}')
			exp_stack=exp_stack[:-2]
			exp_stack.append(f't{t}')
			t+=1

expres = input("INPUT THE EXPRESSION: ")
pre = infix_to_prefix(expres)
pos = infix_to_postfix(expres)
generate3AC(pos)
def Quadruple(pos):
  stack = []
  op = []
  x = 1
  for i in pos:
    if i not in OPERATORS:
       stack.append(i)
    elif i == '-':
        op1 = stack.pop()
        stack.append("t(%s)" %x)
        print("{0:^4s} | {1:^4s} | {2:^4s}|{3:4s}".format(i,op1,"(-)"," t(%s)" %x))
        x = x+1
        if stack != []:
          op2 = stack.pop()
          op1 = stack.pop()
          print("{0:^4s} | {1:^4s} | {2:^4s}|{3:4s}".format("+",op1,op2," t(%s)" %x))
          stack.append("t(%s)" %x)
          x = x+1
    elif i == '=':
      op2 = stack.pop()
      op1 = stack.pop()
      print("{0:^4s} | {1:^4s} | {2:^4s}|{3:4s}".format(i,op2,"(-)",op1))
    else:
      op1 = stack.pop()
      op2 = stack.pop()
      print("{0:^4s} | {1:^4s} | {2:^4s}|{3:4s}".format(i,op2,op1," t(%s)" %x))
      stack.append("t(%s)" %x)
      x = x+1
print("The quadruple for the expression ")
print(" OP | ARG 1 |ARG 2 |RESULT  ")
Quadruple(pos)

def Triple(pos):
        stack = []
        op = []
        x = 0
        for i in pos:
          if i not in OPERATORS:
            stack.append(i)
          elif i == '-':
            op1 = stack.pop()
            stack.append("(%s)" %x)
            print("{0:^4s} | {1:^4s} | {2:^4s}".format(i,op1,"(-)"))
            x = x+1
            if stack != []:
              op2 = stack.pop()
              op1 = stack.pop()
              print("{0:^4s} | {1:^4s} | {2:^4s}".format("+",op1,op2))
              stack.append("(%s)" %x)
              x = x+1
          elif i == '=':
            op2 = stack.pop()
            op1 = stack.pop()
            print("{0:^4s} | {1:^4s} | {2:^4s}".format(i,op1,op2))
          else:
            op1 = stack.pop()
            if stack != []:           
              op2 = stack.pop()
              print("{0:^4s} | {1:^4s} | {2:^4s}".format(i,op2,op1))
              stack.append("(%s)" %x)
              x = x+1
print("The triple for given expression")
print("  OP | ARG 1 |ARG 2  ")
Triple(pos)

# GIVE SPACE BETWEEN TWO EXPRESSION
```
