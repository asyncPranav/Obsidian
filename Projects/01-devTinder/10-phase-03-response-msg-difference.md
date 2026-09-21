
---

## `data: connection` vs `data: { connection }`

Both are valid JavaScript, but they produce **different JSON response structures**.

---

### 1. `data: connection`

Here, the value of `data` is directly the `connection` object.

```
return res.json({
  data: connection,
});
```

Response:

```
{
  "data": {
    "_id": "123",
    "status": "interested"
  }
}
```

Access it as:

```
response.data._id
response.data.status
```

Think:

```
data
 └── connection fields
```

---

### 2. `data: { connection }`

Here, `data` contains a property named `connection`.

```
return res.json({
  data: { connection },
});
```

This is shorthand for:

```
data: {
  connection: connection,
}
```

Response:

```
{
  "data": {
    "connection": {
      "_id": "123",
      "status": "interested"
    }
  }
}
```

Access it as:

```
response.data.connection._id
response.data.connection.status
```

Think:

```
data
 └── connection
      └── connection fields
```

---

## Main Difference

|`data: connection`|`data: { connection }`|
|---|---|
|`data` directly contains the object|`data` contains a named `connection` property|
|Less nesting|One extra level of nesting|
|`data._id`|`data.connection._id`|
|Simple for a single resource|Useful when naming/grouping resources|
|Changes if more data is added directly|Easier to add related data inside `data`|

### Example with multiple values

With:

```
data: connection
```

you cannot naturally put another named value beside the connection object.

With:

```
data: {
  connection,
  message,
}
```

you can return:

```
{
  "data": {
    "connection": {
      "_id": "123"
    },
    "message": "Request sent"
  }
}
```

### Important

Curly brackets here are **not about whether the object exists**. They determine **how the object is structured inside `data`**.

```
data: connection
       ↓
data = connection object

data: { connection }
       ↓
data = {
  connection: connection object
}
```

For your DevTinder APIs, choose one response convention and use it consistently across controllers.