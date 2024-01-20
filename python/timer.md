## Timer Display in Minutes and Seconds

To display the elapsed time in minutes and seconds, you can modify the `print` statement in your timer display function. You'll need to convert the total seconds into minutes and seconds. Here's how you can do it:

```python
import threading
import time

def function_to_time():
    # Replace this with the actual work of the function
    time.sleep(10)  # Example function that takes some time

def timer_display(stop_event):
    start_time = time.time()
    while not stop_event.is_set():
        elapsed_time = time.time() - start_time
        minutes = int(elapsed_time // 60)
        seconds = int(elapsed_time % 60)
        print(f"\rElapsed Time: {minutes} minutes, {seconds} seconds", end="")
        time.sleep(0.1)
    print("\nFunction completed!")

# Event to signal when the function is done
stop_event = threading.Event()

# Timer thread
timer_thread = threading.Thread(target=timer_display, args=(stop_event,))

# Start the timer
timer_thread.start()

# Run the function
function_to_time()

# Signal the timer to stop and wait for the timer thread to finish
stop_event.set()
timer_thread.join()
```

In this updated `timer_display` function:

- `minutes` is calculated by dividing the `elapsed_time` by 60 and taking the integer part (using floor division `//`).
- `seconds` is calculated by taking the remainder of the `elapsed_time` divided by 60 (using the modulo operator `%`).
- The `print` statement now displays the elapsed time in minutes and seconds.

This format provides a more readable output of the elapsed time.
