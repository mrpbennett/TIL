# Disabling Web Security for Chrome

Sometimes when you're calling an API from the client it will be rejected becaused of CORS to work around this, you can disable the web security features. 

Like so

```
# Chrome Shortcuts
alias chrome="open -na Google\ Chrome --args --user-data-dir=/tmp/temporary-chrome-profile-dir --disable-web-security"
```
