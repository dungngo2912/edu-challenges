Fluent Python
===
# Part 1: Prologue
## Chapter 1: Data Model
### 1.1. Special methods:
* Note:
    - Specific methods:
    ![](https://i.imgur.com/eCxvumX.png)
    - What are diferences between `__repr__` and `__str__`?
    - Why `__len__` is not a method?
# Part 2: Data Structure
## Chapter 2: An array of sequences
### 2.1 List Comprehension & generator expression
* Notes:
    - The diferences of scope list comprehension between Python2 and Python3:
    ```python
    x = 'my precious'
    dummy = [x for x in 'ABC']
    x
    'C'
    # x will be protected with python3 and showed 'my precious'
    ```
    - Take over somethings without `map` and `filter`:
    ```python
    symbols = '$¢£¥€¤'
    beyond_ascii = [ord(s) for s in symbols if ord(s) > 127]
    beyond_ascii = list(filter(lambda c: c > 127, map(ord, symbols)))
    two results are same. :))
    ```
    - More than one iterator:
    ```python
    classes = [(grade, name) for grade in range(1,13) for name in 'ABCD']
    ```
### 2.2 Tuples are not just immutable lists
* Notes:
    - assign vairable:
    ```python
    # create tuple:
    v = (1, 2, 3) #or tuple([1, 2, 3]) or tuple(x for x in range(1, 4))
    a, b, c = v # tuple unpacking
    print('%s,%s,%s'%v)
    ```
    - tuple unpacking & difference between `*` and `**`
    ```
    divmod(*(2, 3)) # divmod((2, 3)) Error: expected 2 but recieved 1 agrument
    a, b, *c = range(5)
    a, b, c = range(2)
    a, *b, c = range(5)
    ```
    > *references:*
    >    * [python-kwargs-and-args](https://realpython.com/python-kwargs-and-args/)
    >   * [Asterisks_for_unpacking_into_function_call](https://treyhunner.com/2018/10/asterisks-in-python-what-they-are-and-how-to-use-them/#Asterisks_for_unpacking_into_function_call)
    > 
    - Nested unpacking and Named tuple
    ```python
    cities = [('Tokyo', 'JP', 36.933, (35.689722, 139.691667)),
    ('Delhi NCR', 'IN', 21.935, (28.613889, 77.208889))]
    for name, cc, pop, (latitude, longitude) in cities:
        print(name, latitude)
    from collections import namedtuple
    # Create City class with specific structure
    City = namedtuple('City', 'name country population coordinates')
    tokyo = City('Tokyo', 'JP', 36.933, (35.689722, 139.691667))
    # print tokyo => City(name='Tokyo', country='JP', population=36.933, coordinates=(35.689722, 139.691667))
    # We can make same with field coordinates
    type(tokyo) # result <class '__main__.City'>
    ```
    - Tuple as immutable lists
    ![](https://i.imgur.com/Fy4Wsld.png)
    - Slice operator
    ```python
    a = 'bicycle'
    a[::2] #1
    a[0:len(a):2] #2
    s = slice(0, len(a), 2) # slice is a object built-in python
    a[s] #3
    # 1, 2 and 3 will response same result.
    
    *n, = range(7) # n = [1, 2, 3, 4, 5, 6]
    n[1:5] = [b, c]
    print(n) # n = [1, b, c, 5, 6] :lost one element
    ```
    - Reassigned operator
    ```python
    #LIST
    l = [1, 2, 3]
    id(l) #1 
    l *= 2
    id(l) #2
    #1 and #2 are same
    
    #TUPLE
    t = (1, 2, 3)
    id(t) #1
    t += 2
    id(t) #2
    #1 and #2 are not same because tuple need to relocated as immutable object
    
    #wtf example
    t = (1, 2, [3, 4]) #1
    t[2] += [5, 6] #2
    # Error: tupple can not support reassigned operator
    print(t) # (1, 2, [3, 4, 5, 6]). WTF
    # this because value(t[2]) = id 
    # and at #2 state everthing still have nothing changed with #1 (see LIST example above). 
    # But Python always catch it as semantics concept with tuple. :))
    ```
    - generator expression for reading
    - Array for read/write binary file
## Chapter 3: Dictionaries and Sets
### 3.1 Mapping
* Notes:
    + Mapping types:
    ![](https://i.imgur.com/ZH4dEIr.png)
    + Mapping methods:
    ![](https://i.imgur.com/xtUS6ad.png)
    > \* a default_factory is not a method, but a callable instance attribute set by the end user when defaultdict is instantiated.
    > \* b OrderedDict.popitem() removes the first item inserted (FIFO); an optional last argument, if set to True , pops the last item (LIFO).
    + Using `setdefault`, `update` for prevent access fault:
        - different `'__getitem(k)__'`(s[k]) and get method `s.get(k, default)`
        - setdefault will be create default type when called to create new element when missing key and then return reference address for update the operator after.
    + What is hash table (which object types can be hashed?)
        - hash value, hash function (\_\_hash__), hash compare (\_\_eq__) 
        - str, byte, numerical, frozenset, tuple are hashable objects.
    + dict comprehensions
    ```python
    unicode_name = {ascii_code: chr(ascii_code) for ascii_code in range(0, 256)}
    ```
    
    + Other Mapping types:
        - `collections.OrderedDict`
        - `collections.ChainMap`
        - `collections.Counter`
        - `collections.UserDict`
    + Immutable mappings with `types.MappingProxyType`

### 3.2 Sets
* Notes:
    + Set elements have to hashable (for using hastable data structure) but set haven't.
    + Using `set()` or `{}` for construction?
    + Set comprehensions
    ```python
    {chr(i) for i in range(32, 256) if 'SIGN' in name(chr(i),'')}
    ```
    + Set operations:
    ![](https://i.imgur.com/sEAMseX.png)
    + Set operations and methods:
    ![](https://i.imgur.com/p7ZH7ew.png)
    ![](https://i.imgur.com/NbuIRoY.png)
### 3.3 Hash table
* Notes
    + Hash table always is a sparse array.
    + What is collision problem? and How to resolve it?
    ![](https://i.imgur.com/K2m4tW6.png)
        - Seperate Chaining
        ![](https://i.imgur.com/FV9qpmK.png)
        > + Advantages:
        > 1) Simple to implement.
        > 2) Hash table never fills up, we can always add more elements to the chain.
        > 3) Less sensitive to the hash function or load factors.
        > 4) It is mostly used when it is unknown how many and how frequently keys may be inserted or deleted.
        > + Disadvantages:
        > 1) Cache performance of chaining is not good as keys are stored using a linked list. Open addressing provides better cache performance as everything is stored in the same table.
        > 2) Wastage of Space (Some Parts of hash table are never used)
        > 3) If the chain becomes long, then search time can become O(n) in the worst case.
        > 4) Uses extra space for links.
        - Open Addressing
        \+ In Open Addressing, all elements are stored in the hash table itself. So at any point, size of the table must be greater than or equal to the total number of keys (Note that we can increase table size by copying old data if needed).
        *Example:*
        ![](https://i.imgur.com/GQhQi7z.png)
        \+ **Clustering** is term of Open Addressing where many consecutive elements form groups and it starts taking time to find a free slot or to search (probing) an element.
        \+ Have 3 probing methods: 
        > Linear probing: best cache performance but suffers from clustering and easy to compute
        > Quadratic Probing: lies between the two in terms of cache performance and clustering.
        > Double Hashing: poor cache performance but no clustering and requires more computation time as two hash functions need to be computed.
    + Compare **Seperate Chaining** and **Open Addressing**
    ![](https://i.imgur.com/t8rxf0T.png)
        - Perfromance:
            > m = Number of slots in hash table
            > n = Number of keys to be inserted in hash table
            > \+ **Seperate Chaining:**
            > Load factor α = n/m
            > Expected time to search = O(1 + α)
            > Expected time to insert/delete = O(1 + α)
            > Time complexity of search insert and delete is O(1) if  α is O(1)
            > \+ **Open Addressing:**
            > Load factor α = n/m  ( < 1 )
            > Expected time to search/insert/delete < 1/(1 - α) 
            > So Search, Insert and Delete take (1/(1 - α)) time
    + Summary:
        - Hash table is an efficient data structure for `set` and `dict` in Python
        - Key(dict) and item(set) must be hashable objects:
            > An object is hashable if all of these requirements are met:
            > 1. It supports the hash() function via a `__hash__()` method that always returns the same value over the lifetime of the object.
            > 2. It supports equality via an `__eq__()` method.
            > 3. If a == b is True then hash(a) == hash(b) must also be True.
        - Each items in hash table (sparse array) is called bucket and they contained referenced address of key and value object (dict) or item (set)
        - Both `dicts` and `sets` have significant memory overhead: build hash table and storing field names
        - Searching is very fast
        - Element ordering depends on insertion order
        - Adding items to a dict may change the order of existing keys
