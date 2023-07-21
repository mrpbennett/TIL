# How Redis stores data using python lib

In Redis, you have several data structures available to store data. Each data
structure has its own specific use cases and provides different operations for
manipulating the stored data. Here are some common data structures you can use
in Redis, along with examples in Python using the redis-py library:

1. Key-Value Pairs (String)

This is the simplest and most basic data structure in Redis. You can store
individual values associated with unique keys.

```python
import redis

# Connect to Redis server
redis_client = redis.StrictRedis(host='localhost', port=6379, db=0)

# Storing key-value pairs
redis_client.set('name', 'Alice')
redis_client.set('age', 30)

# Retrieving values
name = redis_client.get('name')
age = redis_client.get('age')
print(name.decode('utf-8'))  # Output: "Alice"
print(int(age))             # Output: 30
```

2. Lists

Lists allow you to store a collection of ordered elements. They are useful for
implementing queues or stacks.

```python
# Storing and retrieving lists
redis_client.rpush('numbers', 1)
redis_client.rpush('numbers', 2)
redis_client.rpush('numbers', 3)

numbers_list = redis_client.lrange('numbers', 0, -1)
print(numbers_list)  # Output: [b'1', b'2', b'3']
```

3. Sets

Sets store a collection of unordered, unique elements. They are useful for
handling unique items or performing set operations like unions, intersections,
and differences.

```python
# Storing and retrieving sets
redis_client.sadd('fruits', 'apple')
redis_client.sadd('fruits', 'banana')
redis_client.sadd('fruits', 'orange')

fruits_set = redis_client.smembers('fruits')
print(fruits_set)  # Output: {b'apple', b'banana', b'orange'}
```

4. Hashes

Hashes store field-value pairs and are often used to represent objects or
dictionaries.

```python
# Storing and retrieving hashes
person = {
    'name': 'Alice',
    'age': 30,
    'email': 'alice@example.com'
}

redis_client.hmset('person:123', person)
stored_person = redis_client.hgetall('person:123')
print(stored_person)  # Output: {b'name': b'Alice', b'age': b'30', b'email': b'alice@example.com'}
```

5. Sorted Sets

Sorted sets store a collection of unique elements with associated scores,
allowing you to perform range-based operations.

```python
# Storing and retrieving sorted sets
redis_client.zadd('scores', {'Alice': 100, 'Bob': 85, 'Charlie': 95})

# Retrieve the rank of 'Bob'
rank_bob = redis_client.zrevrank('scores', 'Bob')
print(rank_bob)  # Output: 2 (assuming zero-based indexing)

# Retrieve the top two scores
top_scores = redis_client.zrevrange('scores', 0, 1, withscores=True)
print(top_scores)  # Output: [(b'Alice', 100.0), (b'Charlie', 95.0)]

```

These examples showcase some of the key data structures available in Redis and
how to use them with Python using the redis-py library. Remember that Redis
provides many more commands and features.
