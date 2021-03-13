# Flow control with while loops

![flow-control](https://automatetheboringstuff.com/2e/images/000039.jpg)

In a flowchart, there is usually more than one way to go from the start to the end. The same is true for lines of code in a computer program

Using a `while` loop allows you keep looping through conditions until a condition is met or there is a `break`. Using the above image you can see how this would be implemented in python code

```python
import time

while True:
    r = input("is it raining? y/n: ")

    if r == "y":
        u = input("do you have an umbrella? y/n: ")

        if u == "y":
            r = False
            print("Go Outside")
            break

        if u == "n":
            print("wait a while")
            time.sleep(3)
            r = True
    else:
        r = False
        print("Go Outside")
        break

```
