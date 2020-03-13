Fluent Python
===
# Part 1: Prologue
## Chapter 1: Data Model
### 1.1. Special methods:
* Descriptions:
![](https://i.imgur.com/eCxvumX.png)
* Note:
    - what are diferences between __repr__ and __str__?
    - why __len__ is not a method?
    - 
* References:
## Chapter 2: Data structures
### 2.1 List Comprehension & generator expression
* Descriptions:
* Notes:
    - The diferences of scope list comprehension between Python2 and Python3:
    ```python
    >>> x = 'my precious'
    >>> dummy = [x for x in 'ABC']
    >>> x
    'C'
    # x will be protected with python3
    ```
    - Take over somethings without **map** and **filter**:
    ```python
    >>> symbols = '$¢£¥€¤'
    >>> beyond_ascii = [ord(s) for s in symbols if ord(s) > 127]
    >>> beyond_ascii = list(filter(lambda c: c > 127, map(ord, symbols)))
    two results are same. :))
    ```
* References: