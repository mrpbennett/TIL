# Using filters

Sometimes you need to use a filter, to format something like a data or a number that's outputted from an API or any other source which doesnt format the output.

Using filters in vue.js reminds me a little of Django.

But with packages such as [moment](https://momentjs.com/) and [numeral](http://numeraljs.com/) you can create a vue filter.

```javascript
// main.js
import moment from 'moment';
import numeral from 'numeral';

// Format Dates
Vue.filter('formatDate', function (value) {
    return moment(String(value)).format('MM/DD/YYYY');
});

// Format Numbers
Vue.filter('formatNumber', function (value) {
    return numeral(value).format('0,0');
});
```

With the above we can use the filter `formatDate` or `formatNumber` to bend the out putted data to our will. All that is left is to add the filter as simple as that.

```
{{ ci.someUnformattedNum | formatNumber }}
```

```
{{ Date | formatDate }}
```
