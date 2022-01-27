# Creating a progress bar

For this progress bar I generally use the package called [progess](https://github.com/verigak/progress)

This is how I have generally set it up

```python
def progress_bar():
    with Bar(f"Uploading!", fill="🟪", max=60, suffix='%(percent)d%%') as bar:
        for _ in range(60):
            time.sleep(1)
            bar.next()
```

A few things I learnt here was the use of `max`, `range()` and `sleep()`. My thought process behind this was that I need the progress bar to last 1 minute, as I have used this to prevent an overload of an API. 

So I used `max` and gave it an int of `60` with a `range(60)` then gave the for loop a `time.sleep(1)` of one 1 second. The loop will last for 60 seconds, due to the `range(60)`, the `max` value allows the `suffix` to display the correct percentage.

I have used the above progress bar in the following manner:

```python
try:
    for i, v in enumerate(x):
        r = requests.post('https://httpbin.org/post', json=json.dumps(v))
        r.raise_for_status()

        if r.status_code == requests.codes.ok:
            progress_bar()
            print(f'List #{i + 1} uploaded 👍')

except requests.exceptions.HTTPError as err:
    raise SystemExit(err)
```

Using the for loop here, we POST to an API and if successful it triggers `progress_bar()` for 60s. Once complete the `enumerate()` for loop POSTs to the API again, then triggers the progress bar once more.

So would kind of look like this. Depending what the lenght of `x` was. BTW I was passing a list into the value of `x`

<img width="676" alt="Screenshot 2022-01-27 at 13 52 47" src="https://user-images.githubusercontent.com/1844080/151372287-07833d8f-4395-4b77-88e6-5952c721e3ea.png">
