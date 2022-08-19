# Using the FV formula

The FV function calculates the future value of an investment based on an interest rate.

- [Excel](https://support.microsoft.com/en-us/office/fv-function-2eef9f44-a084-4c61-bdd8-4fe4bb1b71b3)
- [Google Sheets](https://support.google.com/docs/answer/3093224?hl=en-GB)

So for example, say you wanted to see what a £1000 starting balance looked like after 10 years at 5% interest per year you would write.

```
=(FV(5%,10,0,-1000))
```

Which gives a total of £1,628.89

Or say you have a £1000 starting balance, you're depositing £100 every month, and expecting interest to compound every month for 120 months you could write
```
=(FV(interest_rate / 12, months, monthly_depoists, starting_balance))
---
=(FV(5%/12,120,-100, -1000))
```
Which gives a total of £17,175.24

Plugging in your numbers, to see your balance in 27 years time we could write
```
=(FV(12.01%,27,-18000,0))
```
Which gives you a total of £3,053,907.38