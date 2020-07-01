# Creating a Star Rating in React

When trying to create say a card, that allows for a rating in stars or any icon of that matter. It wasn't that easy for me...especially when watching a TailWind tutorial using Vue.js

This is how I created similar in React

```javascript
<div>
{
    [...Array(5)].map((star, i) => {
        const ratingValue = i + 1;
        return (
            <AiFillStar color={ratingValue > card.rating ? 'grey' : 'teal'} />
        );
    });
}
</div>
```

Using the above I was able to create these delightful looking cards.

![cards](https://github.com/mrpbennett/TIL/blob/master/images/cards.png)

### 1.

First, we need to create an empty array of 5 items by adding the following: `[...Array(5)]` we then map over it. Giving us 5 stars which we will return.

### 2.

We need to create an iterator so we can give our stars a value of 1 - 5. `const ratingValue = i + 1;` using 1 allows us to start our array at 1 instead of 0.

### 3.

In our return statement, we have our icon which is imported from react-icons with a `color` prop. We need to conditionally check if our `ratingValue` is greater than our `card.rating` to be able to produce the number of coloured stars we need. We do this by using an inline if-else with an conditional operator `condition ? true : false.`.

Here we are comparing if `ratingValue` is greater than `card.rating` if true then produce grey stars if false produce teal stars.

```javascript
<AiFillStar color={ratingValue > card.rating ? 'grey' : 'teal'} />
```

If you're wondering where my `card.` is coming from the star rating sits in another map method which is how we're using `card.rating` in our conditional:

```javascript
<div className='cardContainer'>
    {cardData.map((card) => (
        <div className='card'>
            <img src={card.imgUrl} />
            ...

```

Below is the data set I used to get the card rating from.

```javascript
const cardData = [
    {
        imgUrl:
            'https://images.unsplash.com/photo-1479502806991-251c94be6b15?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80',
        beds: 3,
        baths: 2,
        title: 'Modern apartment in city center',
        price: 390000,
        formattedPrice: '£3,900.00',
        reviewCount: 69,
        rating: 4,
    },
    {
        imgUrl:
            'https://images.unsplash.com/photo-1445019980597-93fa8acb246c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1353&q=80',
        beds: 1,
        baths: 2,
        title: 'Desert get away in the sun',
        price: 230000,
        formattedPrice: '£2,300.00',
        reviewCount: 108,
        rating: 3,
    },
    {
        imgUrl:
            'https://images.unsplash.com/photo-1553653924-39b70295f8da?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80',
        beds: 2,
        baths: 1,
        title: 'Coastal apartment with communal pool',
        price: 900,
        formattedPrice: '£900.00',
        reviewCount: 2567,
        rating: 5,
    },
];
```

That is it really...I got stuck with this a couple of time but now it's all working I am happy with the results.
