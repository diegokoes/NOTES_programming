# PYTHON > EXCEPTIONS

```python

try: 
	ans = divide_by(40,0)
except Exception (o ZeroDivisionError) as e:
	print('error message...',e)
	print(e.__class__)
except Exception as e:
	print(e, ' something went wrong')
```


- - -
#python