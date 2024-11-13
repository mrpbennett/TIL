# How to keep a script running if it fails

I had issues with my script that needs to run for long periods of time, where it kept crashing meaning I would have to start the script again. This lead me to finding a solution to automatically restart the script if it fails.

To automatically restart a Python script if it stops running, there are have several options here I went with a simple bash script.

```bash
# script to keep script running if it stops
while true; do
    poetry run python <path-to-file>/main.py
    echo "Script crashed. Restarting in 5 seconds..."
    sleep 5
done

```

- Make it executable with `chmod +x restart_script.sh`
- Run it with `./restart_script.sh`

This will restart the script automatically if it stops, with a 5-second delay between attempts.
