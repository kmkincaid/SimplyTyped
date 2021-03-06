# SimplyTyped

Yet another typing library.
This differs by aiming to be less experimental than others, driven by industry use cases.

Many of the exposed types are a very thin layer above built in functionality.
The goal is to provide all of the building blocks necessary to make concise, yet complex types.

## Conditionals

Some (somewhat common) implementations of conditional type logic

### If
```ts
type x = If<True, string, number> // => string
type y = If<False, string, number> // => number
```

### And
```ts
type x = If<And<True, True>, string, number> // => string
type y = If<And<True, False>, string, number> // => number
...
```

### Or
```ts
type x = If<Or<True, False>, string, number> // => string
type y = If<Or<False, False>, string, number> // => number
...
```

### Not
```ts
type x = Not<True> // => False
type y = Not<False> // => True
```

### Xor
```ts
type x = Xor<True, False> // => True
type y = Xor<True, True> // => False
...
```

### Nand
```ts
type x = Nand<True, True> // => False
type y = Nand<False, True> // => True
```

## Objects

```ts
type obj1 = { w: string, x: string, y: number }
type obj2 = { y: string, z: number }
```

### Keys
No different than `keyof`, but can look a bit nicer when nesting many types deep
```ts
type x = keyof obj1 // => 'w' | 'x' | 'y'
type y = Keys<obj1> // => 'w' | 'x' | 'y'
```

### ObjectType
On its own, not that interesting.
Takes an object and makes it an object type.
Is useful when combined with `&` intersection types (as seen next).
```ts
type x = ObjectType<obj1> // => { w: string, x: string, y: number }
```

### CombineObjects
Takes the intersection between two objects, and flattens them.
This can make extremely complex types look much nicer.
```ts
type x = obj1 & obj2 // => { w: string, x: string, y: number } & { y: string, z: number }
type y = CombineObjects<obj1, obj2> // => { w: string, x: string, y: string & number, z: number }
```

### Intersect
Returns only the shared properties between two objects.
All shared properties must be the same type.
```ts
type x = Intersect<obj1, { x: string }> // => { x: string }
```

### SharedKeys
Gets all of the keys that are shared between two objects (as in keys in common).
```ts
type x = SharedKeys<obj1, obj2> // => 'y'
```

### DiffKeys
Gets all of the keys that are different from obj1 to obj2.
```ts
type x = DiffKeys<obj1, obj2> // => 'w' | 'x'
type y = DiffKeys<obj2, obj1> // => 'z'
```

### AllKeys
Gets all keys between two objects.
```ts
type x = AllKeys<obj1, obj2> // => 'w' | 'x' | 'y' | 'z'
```

### Omit
Gives back an object with listed keys removed.
```ts
type x = Omit<obj1, 'w' | 'x'> // => { y: number }
```

### Merge
Much like `_.merge` in javascript, this returns an object with all keys present between both objects, but conflicts resolved by rightmost object.
```ts
type x = Merge<obj1, obj2> // => { w: string, x: string, y: string, z: number }
```

### Overwrite
Can change the types of properties on an object.
This is similar to `Merge`, except that it will not add previously non-existent properties to the object.
```ts
type a = Overwrite<obj1, obj2> // => { w: string, x: string, y: string }
type b = Overwrite<obj2, obj1> // => { y: number, z: number }
```

### DeepPartial
Uses `Partial` to make every parameter of an object optional (`| undefined`).
```ts
type x = DeepPartial<obj1> // => { w?: string, x?: string, y?: number }
```

## Tuples
A tuple can be defined in two ways: `[number, string]` which as of Typescript 2.7 has an enforced length type parameter: `[number, string]['length'] === 2` or using this library's `Tuple<any>` which can be extended with any length of tuple: `function doStuff<T extends Tuple<any>>(x: T) {}`.

### Tuple
```ts
function doStuff<T extends Tuple<any>>(x: T) {}

doStuff(['hi', 'there']); // => doStuff(x: ['hi', 'there']): void
```

### UnionizeTuple
Returns elements within a tuple as a union.
```ts
type x = UnionizeTuple<[number, string]> // => number | string
```

### Length
Gets the length of either a built-in tuple, or a Vector.
This will only work after Typescript 2.7 is released.
```ts
type x = Length<['hey', 'there']> // => 2
```

## Strings

### Diff
Get the differences between two unions of strings.
```ts
type x = Diff<'hi' | 'there', 'hi' | 'friend'> // => 'there'
```

### IsNever
Returns true if type is `never`, otherwise returns false.
```ts
type x = IsNever<'hi'> // => True
type y = IsNever<never> // => False
```

### StringEqual
Returns true if all elements in two unions of strings are equal.
```ts
type x = StringEqual<'hi' | 'there', 'hi'> // => False
type y = StringEqual<'hi' | 'there', 'there' | 'hi'> // => True
```

### DropString
Can remove a string from a union of strings
```ts
type x = DropString<'hi' | 'there', 'hi'> // => 'there'
```

## Numbers

Supports numbers from [0, 63]. More slows down the compiler to a crawl right now.

### IsZero
Returns true if the number is equal to zero.
```ts
type x = IsZero<1> // => False
type y = IsZero<0> // => True
```

### IsOne
Returns true if the number is equal to one.
```ts
type x = IsOne<0> // => False
type y = IsOne<1> // => True
```

### NumberToString
Returns the string type for a given number
```ts
type x = NumberToString<0> // => '0'
type y = NumberToString<1> // => '1'
```

### Next
Returns the number + 1.
```ts
type x = Next<0> // => 1
type y = Next<22> // => 23
```

### Prev
Returns the number - 1.
```ts
type x = Prev<0> // => -1
type y = Prev<23> // => 22
```

### Add
Adds two numbers together.
```ts
type x = Add<22, 8> // => 30
```

### Sub
Subtracts the second from the first.
```ts
type x = Sub<22, 8> // => 14
```

### NumberEqual
Returns `True` if the numbers are equivalent
```ts
type x = NumberEqual<0, 0> // => True
type y = NumberEqual<22, 21> // => False
```
