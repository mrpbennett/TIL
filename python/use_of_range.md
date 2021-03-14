# The use of `range()`

`range()` allows you to loop over something depending on the arguments passed.


This will print i 5 times, as 5 was passed into range()
```python
for i in range(5):
  print(i)
  
>>>
0
1
2
3
4
```
`range()` also allows two arguments to be passed, the first argument is the start number and the 2nd arg is the end number. The result will be as follows:
```python
for i in range(12, 16)
  print(i)
 
>>>
12
13
14
15
```
`range()` will can also take 3 arguments, this time the first two are start & stop. The 3rd is the 'step' argument. In the example below the loop will count from 0 to 8 in increments of 2
```python
for i in range(0, 10, 2):
  prnit(i)
  
 >>>
0
2
4
6
8
```
You're even able to use negative numbers, to allow for a count down loop. By using `-1` as the step argument.
```python
for i in range(5, -1, -1):
  print(i)

>>>
5
4
3
2
1
0
```
