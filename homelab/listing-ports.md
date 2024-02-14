# Listening ports and applications using lsof command

Let us run the following to check open TCP and UDP ports using the lsof command:

```bash
sudo lsof -i -P -n | grep LISTEN
```

Where,

- `i` : Look for listing ports
- `P` : Inhibits the conversion of port numbers to port names for network files. Inhibiting the conversion may make `lsof` run a little faster. It is also useful when port name lookup is not working properly.
- `n` : Do not use DNS name
- `| grep LISTEN` : Again only show ports in LISTEN state using the grep command as filter.


0327a144-6ae2-4b5e-899e-3cb6174e9674