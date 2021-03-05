# Writing lines to a file

I wanted to export a response from a request (a list of domains) to a txt file, so I would be able manlipuate it. 

I ran into two issues at first. Placing the file creation **INSIDE OF MY LOOP** no wonder i was only getting one domain in the file!! Doh!

Placing the file creation outside of my loop fixed that issue.

The I wanted to dump everything onto a single line. I completed this by using:

```python
.writelines("%s\n" % d["domain"])
```

### writelines(lines)
Write a list of lines to the stream. Line separators are not added, so it is usual for each of the lines provided to have a line separator at the end.

Alternatively after some testing got the same result

```python
.write("%s\n" % d["domain"])
```

So after some more testing, the only thing I needed to bring everything on one line was `"%s\n" %`

Here is a full example

```python
with open("./files/dump/sjson_domains.txt", "w") as sjson_domains_txt:
    for d in sellers:
        if d.get("domain", "domain_missing") != "domain_missing":
            sjson_domains_txt.write("%s\n" % d["domain"])
```
