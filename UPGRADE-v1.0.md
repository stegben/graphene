# v1.0 Upgrade Guide

Big changes from v0.10.x to 1.0. While on the surface a lot of this just looks like shuffling around API, the entire codebase has been rewritten to handle some really great use cases and improved performance.


## Backwards Compatibility and Deprecation Warnings

This has been a community project from the start, we need your help making the upgrade as smooth as possible for everybody!
We have done our best to provide backwards compatibility with deprecated APIs.


## Interfaces

For implementing an Interface in a ObjectType, you have to add also `ObjectType`.

Like:

```python
from graphene import Interface, ObjectType, String

class Character(Interface):
    name = String()

class Human(Character): # Old way, not working anymore
    pass

class Droid(Character, ObjectType): # New way, you have to specify the ObjectType
    pass
```

## Mutations

Mutation fields have changed the way of usage, before if you have the mutation `MyMutation` you
only have to reference with `graphene.Field(MyMutation)` now it's simply `MyMutation.Field()`

Example:

```python
from graphene import ObjectType, Mutation, String

class ReverseString(Mutation):
    class Input:
        input = String(required=True)

    reversed = String()

    def mutate(self, args, context, info):
        reversed = args.get('input')[::-1]
        return ReverseString(reversed=reversed)

class Query(ObjectType):
    reverse_string = graphene.Field(ReverseString) # Old way, not working anymore
    reverse_string = ReverseString.Field()
```

## Nodes

Apart of implementing as showed in the previous section, for use the node field you have to
specify the node Type.

Example:

```python
from graphene import ObjectType, relay

class Query(ObjectType):
    node = relay.NodeField() # Old way, NodeField no longer exists
    node = relay.Node.Field() # New way
```

Also, if wanted to create an `ObjectType` that implements `Node`, you have to do it
explicity.


## Django

The Django integration with Graphene now have an independent package: `graphene-django`.
For installing, you have to replace the old `graphene[django]` with `graphene-django`.

### Importing

As the package is now independent, you have to import now from `graphene_django`.

```python
from graphene.contrib.django import DjangoObjectType  # Old way of importing
from graphene_django import DjangoObjectType  # New way
```

### Attributes

Also, the `Meta` option attrs have changed:
* `only_fields` is now `fields`.
* `exclude_fields` is now `exclude`.


## SQLAlchemy

The SQLAlchemy integration with Graphene now have an independent package: `graphene-sqlalchemy`.
For installing, you have to replace the old `graphene[sqlalchemy]` with `graphene-sqlalchemy`.

### Importing

As the package is now independent, you have to import now from `graphene_sqlalchemy`.

```python
from graphene.contrib.sqlalchemy import SQLAlchemyObjectType  # Old way of importing
from graphene_sqlalchemy import SQLAlchemyObjectType  # New way
```