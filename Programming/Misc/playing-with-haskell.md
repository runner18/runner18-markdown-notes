Haskell is a popular functional programming language. Learning Haskell in order to become a better programmer, I guess.

### Installation and Setup
1. Install ghcup
	1. Sets up GHC, the Haskell compiler
	2. Sets up Cabal, the Haskell build tool
	3. Manages HLS whatever that is
2. Confirm install
	1. ghc --version
	2. cabal --version
3. Create and open folder in VS Code
4. Install Haskell VS Code extension
5. Put .hs file in there, that's where ur code goes
6. Add .cabal file, that's the project file
7. Restart VS Code

### Interactive Mode
Open terminal in VS Code, type:
```
ghci
```

This opens interactive mode where you can play with Haskall

Load the file (e.g. nameofhsfile.hs) with
```
:l nameofhsfile
```

Some things you can do

```
- ghci> 2 + 15  
- 17  
- ghci> 49 * 100  
- 4900  
- ghci> 1892 - 1472  
- 420  
- ghci> 5 / 2  
- 2.5  
- ghci>
```

```
- ghci> (50 * 100) - 4999  
- 1  
- ghci> 50 * 100 - 4999  
- 1  
- ghci> 50 * (100 - 4999)  
- -244950
```

```
- ghci> True && False  
- False  
- ghci> True && True  
- True  
- ghci> False || True  
- True  
- ghci> not False  
- True  
- ghci> not (True && True)  
- False
```

```
- ghci> 5 == 5  
- True  
- ghci> 1 == 0  
- False  
- ghci> 5 /= 5  
- False  
- ghci> 5 /= 4  
- True  
- ghci> "hello" == "hello"  
- True
```

```
- ghci> succ 8  
- 9
- ghci> min 9 10  
- 9  
- ghci> max 100 101  
- 101
- ghci> succ 9 + max 5 4 + 1  
- 16  
- ghci> (succ 9) + (max 5 4) + 1  
- 16
```

### Add a Function
In the .hs file
```hs
doubleMe x = x + x
```

Now you can run:
```
- ghci> :l nameofhsfile  
- [1 of 1] Compiling Main             ( nameofhsfile.hs, interpreted )  
- Ok, one module loaded.  
- ghci> doubleMe 9  
- 18  
- ghci> doubleMe 8.3  
- 16.6
```

Functions can't begin with Uppercase letters.

### Lists
```
- ghci> lostNumbers = [4,8,15,16,23,42]  
- ghci> lostNumbers  
- [4,8,15,16,23,42]
```
Every element in a list must be the same data type.

Add two lists together:
```
- ghci> [1,2,3,4] ++ [9,10,11,12]  
- [1,2,3,4,9,10,11,12]  
- ghci> "hello" ++ " " ++ "world"  
- "hello world"  
- ghci> ['w','o'] ++ ['o','t']  
- "woot"
```

Cons operator: Append a character to a string, or a digit to a numbered-list:
```
- ghci> 'A':" SMALL CAT"  
- "A SMALL CAT"  
- ghci> 5:[1,2,3,4,5]  
- [5,1,2,3,4,5]
```

Another way to write out a list
```
1:2:3:[]
```
Appends 3, then 2, then 1 to get \[1,2,3]

Retrieve element from list
```
- ghci> "Steve Buscemi" !! 6  
- 'B'  
- ghci> [9.4,33.2,96.2,11.2,23.25] !! 1  
- 33.2
```

Lists can contain lists lol
```
- ghci> b = [[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]  
- ghci> b  
- [[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]  
- ghci> b ++ [[1,1,1,1]]  
- [[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3],[1,1,1,1]]  
- ghci> [6,6,6]:b  
- [[6,6,6],[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]  
- ghci> b !! 2  
- [1,2,2,3,4]
```
Check if each element in a list is ><==
```
- ghci> [3,2,1] > [2,1,0]  
- True
```

Return certain parts of a list
```
- ghci> head [5,4,3,2,1]  
- 5
- ghci> tail [5,4,3,2,1]  
- [4,3,2,1]
- ghci> last [5,4,3,2,1]  
- 1
- ghci> init [5,4,3,2,1]  
- [5,4,3,2]
```

```
- ghci> head []  
- *** Exception: Prelude.head: empty list
```

Get list's length
```
- ghci> length [5,4,3,2,1]  
- 5
```

Reverse a list
```
- ghci> reverse [5,4,3,2,1]  
- [1,2,3,4,5]
```