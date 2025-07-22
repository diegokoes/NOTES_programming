# Python

- [[python_variablescopes|Variable Scopes]]
- [[python_datastructures|Data Structures]]
- [[python_args_kwargs|Args & Kwargs]]
- [[python_exceptions|Exceptions]]
- [[python_files|File Handling]]
- [[python_modules|Standard Library Modules]]
- [[python_functions|Functions]]

\ to indicate new line 

\# single line comments
```python
''' or """
multi line
comment
''' or """

a=b=c=10
a,b,c = 1,2,3

del a

if x > 0: # ':' to start a code block
```
![[{D2195147-892A-4344-A8C0-1966D6FC7AAE}.png]]
```python
int to float GOOD
String to int ERROR

str() int() float() bool()

ord() hex() oct() tuple() set() list() dict() type()

email = input('owo email')
	
print
	object, sep (!) , end, file, flush
	('your total bill is $',bill_total)
	'...{}...{}'.format(..., ....) 
integer + float ? converts to float

and or not

elif if else

match http_status:
	case 200 | 201:
		print('Success'
	case 400:
		print('Bad request')
	case 500 | 501:
		print('Server error')
	case _:
		print('Unknown')

for item in range(10):
	... 
for index,item in enumerate(favorites, start = 0):
while 

break continue, pass

for ... : 

else:

print (round((time.time() - start_time),2))

`isinstance(object, class_or_tuple)`

function :
	def nameFunction(params)

variable scopes:
```



- - - 
#python