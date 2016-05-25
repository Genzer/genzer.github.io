# How to iterate over a Java collection

## Traditional way

```
List<Integer> numbers = ...

for (int i = 0; i < numbers.length(); i++) {
    if (i % 2 == 0) {
        Integer value = numbers.get(i);
        if (value % 2 == 0) {
            // do something to value
        }
    }
}
```

**Pros**:
- an iconic way
- can access to both index and value at the same time

**Cons**:
- too much code

Use:
- when you need to know the `index` of the element in a loop.


## The `Iterator` way

In Java, `Array` and `Collection` all implement `Iterable` which returns an `Iterator`.

```
List<Integer> numbers = ...

for (Iterator<Integer> it = numbers.iterator(); it.hasNext();) {
    Integer value = it.next();
    if (value % 2 == 0) {
        it.remove();
    }
}
```

If you need to know the `index`, use `ListIterator` (only work with `List`):

```
List<Integer> numbers = ...

for (ListIterator<Integer> it = numbers.listIterator(); it.hasNext();) {
    Integer index = it.nextIndex();
    Integer value = it.next();
    if (index % 2 == 0 && value % 2 == 0) {
        it.remove();
    }
}
```

**Pros**:
- supports iterator over all kinds of `Collection` types (`Set`, `Queue`, etc)
- can `remove` elements during iteration.
- faster than *Tranditional way* in case of `LinkedList` or other `Collection` implementation where random access is time-consuming.

**Cons**:
- too much code

## The Enhanced `for` way

This is the most **recommended** way since Java 5 if you just need to iterate over a `Collection` without the need to access to the `index`.

```
List<Integer> numbers = ...

// read as "for each number in numbers"
for (Integer number : numbers) {
    if (number % 2 == 0) {
        // do something with number
    }
}
```

Tips:

To iterate over an _entire_ `java.util.Map`, please use this:

```
Map<String, Object> cache = ...

for (Map.Entry<String, Object> entry : cache.entrySet()) {
    doSomeThingTo(entry.getKey(), entry.getValue());
}

```

**Pros**:
- simple, easy to read
- keep variables' scopes clean (especially in nested loops)

**Cons**:
- cannot access to the `index` of the element (well, not every time we need it actually)

## The Java 8 way (or the function way or the lambda way)

In Java 8, `Stream` API has been introduce which makes it very easy to iterate and do somthing no elements of a `Collection`.

```
List<Integer> numbers = ...

numbers.stream()
    .filter(number -> numbers % 2 == 0)
        .forEach(number -> doSomething(number));

// or simply

numbers.forEach(number -> doSomething(number));
```

**Pros**:
- easy to write and read
- method chain
- lazy evaluate

**Cons**
- the API has quite a lot of methods so it takes time to master all of them.
