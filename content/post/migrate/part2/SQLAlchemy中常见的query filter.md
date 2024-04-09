---
title: "SQLAlchemy中常见的query filter"
date: 2019-12-08
draft: false
tags:
  - Python
  - sqlalchemy
---
## equals

```python
query.filter(User.name == 'leela')
```

## not equals:

```python
query.filter(User.name != 'leela')
```

## LIKE

```python
query.filter(User.name.like('%leela%'))
```

## IN

```python
query.filter(User.name.in_(['leela', 'akshay', 'santanu']))

# works with query objects too:

query.filter(User.name.in_(session.query(User.name).filter(User.name.like('%santanu%'))))
```

## NOT IN

```python
query.filter(~User.name.in_(['lee', 'sonal', 'akshay']))
```

## IS NULL

```python
filter(User.name == None)
```

## IS NOT NULL

```python
filter(User.name != None)
```

## AND

```python
from sqlalchemy import and_
filter(and_(User.name == 'leela', User.fullname == 'leela dharan'))

#or, default without and_ method comma separated list of conditions are AND

filter(User.name == 'leela', User.fullname == 'leela dharan')

# or call filter()/filter_by() multiple times

filter(User.name == 'leela').filter(User.fullname == 'leela dharan')
```

## OR

```python
from sqlalchemy import or_
filter(or_(User.name == 'leela', User.name == 'akshay'))
```

## match

```python
query.filter(User.name.match('leela'))
```

转载自[这篇博客](http://www.leeladharan.com/sqlalchemy-query-with-or-and-like-common-filters)
