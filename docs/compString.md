# Compiletime String

The type compsString is the type thats acts as a string,<br>
located in the namespace 'CompStringNS'.<br>

A compString can be declared using the type of the user-defined literal _compStr.<br>
Example:<br>
```cpp
using str = decltype("hello world"_compStr); // 'str' is  a compString type containing the string "hello world"
```

# member types / functions
The member types of compString act as member functions 'returning' a type or value.
CompString has most functions that [std::string](https://en.cppreference.com/w/cpp/string/basic_string) contains.<br>
As aliases are not mutable any 'function', that would modify std::string, instead return the modified compString as a new type.<br>

## Size
compString::size -> size_t<br>
Returns the length of the string compString contains. Type is size_t.<br>
Example:<br>
```cpp
constexpr size_t len = decltype("hello world"_compStr)::size; // len = 11
```

## Element access

### at 
compString::at\<size_t pos> -> char<br>
Returns the character at the postion pos of the string.<br>
Example:<br>
```cpp
constexpr char c = decltype("hello world"_compStr)::at<6>; // c = 'w'
```

### front
compString::front -> char<br>
Returns the first character of the string.<br>
Equivalent to compString::at\<0>.<br>
Example:<br>
```cpp
constexpr char c = decltype("hello world"_compStr)::front; // c = 'h'
```

### back
compString::back -> char<br>
Returns the last character in the string.<br>
Equivalent to compString::at\<compString::size-1>.<br>
Example:<br>
```cpp
constexpr char c = decltype("hello world"_compStr)::back; // c = 'd'
```

### c_str
compString::c_str -> const char*<br>
Return a const char* to a null terminated array of characters of length compString::size+1, that represents the string.<br>
Example:<br>
```cpp
const char* strP =  decltype("hello world"_compStr)::c_str;
std::string str(strP); // str will be constructed and contain "hello world"
```

### data
compString::data -> const char* <br>
Same as c_str.<br>

### to_basic_string_view
compString::to_basic_string_view -> constexpr std::basic_string_view<br>
Returns a basic string view of the string.<br>
Example:<br>
```cpp
constexpr std::basic_string_view strView = decltype("hello world"_compStr)::to_basic_string_view; // strView will contain "hello world"
```

### sv
compString::sv -> constexpr std::basic_string_view<br>
Same as to_basic_string_view.<br>

## Modifiers
__NOTE:__ these functions do __NOT__ change the compString that calls them, but return a new compString that __IS__ modified.<br>

### append
compString::append\<compString str> -> compString<br>
Returns a compString with str appended to the end of the compString.<br>
Example:<br>
```cpp
using retStr = decltype("AA"_compStr)::append<decltype("BB"_compStr)>; // retStr contains "AABB"
```

### prepend
compString::prepend\<compString str> -> compString<br>
Returns a compString with str prepended to the begginning of the compString.<br>
Example:<br>
```cpp
using retStr = decltype("AA"_compStr)::prepend<decltype("BB"_compStr)>; // retStr contains "BBAA"
```

### clear
compString::clear -> compString<br>
Returns an empty compString.<br>
Equivalent to decltype(""_compStr).<br>
Example:<br>
```cpp
using retStr = decltype("hello world"_compStr)::clear; // retStr is an empty compString
```

#### insert
compString::insert\<size_t pos, compString str> -> compString<br>
Returns a compString with the compString str inserted after the character at pos.<br>
Example:<br>
```cpp
using retStr =  decltype("hello world"_compStr)::insert<5,decltype("my "_compStr)>; // retStr contains "hello my wolrd"
```

### erase
compString::erase\<size_t pos, size_t count> -> compString<br>
Returns a compString that contains all characters, except the characters in the range \[pos,min(size,pos+count)].<br>
Example:<br>
```cpp
using retStr = decltype("hello my world"_compStr)::erase<5,3>; // retStr contains "hello world"
```

### push_back
compString::push_back\<char chr> -> compString<br>
Returns a compString with char appended to the end.<br>
Example:<br>
```cpp
using retStr = decltype("hello world"_compStr)::push_back<'s'>; // restStr contains "hello worlds"
```

### pop_back
compString::pop_back<> -> compString<br>
Return a compString with all but the last character.<br>
Example:<br>
```cpp
using retStr = decltype("hello world"_compStr)::pop_back<>; // retStr contains "hello worl"
```

### reverse
compString::reverse\<> -> compString
Returns the reverse of compString.<br>
Example:<br>
```cpp
Using str = decltype("hello world"_compStr)::reverse<>; // str contains "dlrow olleh"
```

## Search

### find
compString::find\<compString str, size_t pos = 0> -> size_t<br>
Returns the index of the first occurrence of str in the compString, after pos.<br>
Pos defaults to 0.<br>
If no occurrence is found, -1ull will be returned.<br>
Example:<br>
```cpp
constexpr size_t i =  decltype("hello my world"_compStr)::find<decltype("my"_compStr)>; // i = 6
```

### rfind 
ompString::rfind\<compString str, size_t pos = compString::size> -> size_t<br>
Returns the index of the last occurrence of str in the compString, before pos.<br>
Pos defaults to compString::size.<br>
If no occurrence is found, -1ull will be returned.<br>
Example:<br>
```cpp
constexpr size_t i =  decltype("my, oh my"_compStr)::rfind<decltype("my"_compStr)>; // i = 7
```

### find_first_of
compString::find_first_of\<compString str, size_t pos = 0> -> size_t<br>
Returns the index of the first occurrence, of any character of str, in compString, after pos.<br>
Pos defaults to 0.<br>
If no occurrence is found, -1ull will be returned.<br>
Example:<br>
```cpp
constexpr size_t i = decltype("hello my world"_compStr)::find_first_of<decltype("mylw"_compStr)>; // i = 2
```

### find_first_not_of
compString::find_first_not_of\<compString str, size_t pos = 0> -> size_t
Returns the index of the first occurrence, of any character not in str, in compString, after pos.<br>
Pos defaults to 0.<br>
If no occurrence is found, -1ull will be returned.<br>
Example:<br>
```cpp
constexpr size_t i = decltype("hello my world"_compStr)::find_first_not_of<decltype("ole h"_compStr)>; // i = 6
```

### find_last_of
compString::find_last_of\<compString str, size_t pos = compString::size> -> size_t<br>
Returns the index of the last occurrence, of any character of str, in compString, before pos.<br>
Pos defaults to compString::size.<br>
If no occurrence is found, -1ull will be returned.<br>
Example:<br>
```cpp
constexpr size_t i = decltype("hello my world"_compStr)::find_last_of<decltype("mylw"_compStr)>; // i = 12
```

### find_last_not_of
compString::find_last_not_of\<compString str, size_t pos = compString::size> -> size_t<br>
Returns the index of the last occurrence, of any character not in str, in compString, before pos.<br>
Pos defaults to compString::size.<br>
If no occurrence is found, -1ull will be returned.<br>
Example:<br>
```cpp
constexpr size_t i = decltype("hello my world"_compStr)::find_last_not_of<decltype("mylwo rd"_compStr)>; // i = 1
```

## Operations

### equal
compString::equal\<compString str> -> bool<br>
Checks wether compString is equal to str<br>
Example:<br>
```cpp
constexpr bool b = decltype("hello"_compStr)::equal<decltype("hello"_compStr)>; // b = true
constexpr bool b = decltype("hello"_compStr)::equal<decltype("hello "_compStr)>; // b = false
```

### lesser
compString::lesser\<compString str> -> bool<br>
Checks wether compString is lesser than str.<br>
Can use lesserEq for <=.<br>
Example:<br>
```cpp
constexpr bool b = decltype("hello"_compStr)::lesser<decltype("zzz"_compStr)>; // b = false
constexpr bool b = decltype("hello"_compStr)::lesser<decltype("aaaaaa"_compStr)>; // b = true
```

### greater
compString::greater\<compString str> -> bool<br>
Checks wether compString is greater than str.<br>
Can use greaterEq for >=.<br>
Example:<br>
```cpp
constexpr bool b = decltype("hello"_compStr)::greater<decltype("zzzzzz"_compStr)>; // b = false
constexpr bool b = decltype("hello"_compStr)::greater<decltype("aaa"_compStr)>; // b = true
```

### compare
compString::compare\<compString str> -> int<br>
Compares compString and str in lexicographical order from left to right.<br>
If compString is greater than str, then it returns 1.<br>
If compString is lesser than str, then it returns -1.<br>
If compString is equal to str, then it returns 0.<br>
Example:<br>
```cpp
constexpr int i = decltype("hello my world"_compStr)::compare<decltype("hello our world"_compStr)>; // i = -1
```

compString::compare\<compString str, size_t pos1, size_t count1, size_t pos2, size_t count2> -> int<br>
Compares substring of compString in range \[pos1,pos1+count1] and substring of str in range \[pos2,pos2+count2].<br>

### starts_with
compString::starts_with\<compString str> -> bool <br>
Checks wether compString starts with the string str.<br>
Example:<br>
```cp
constexpr bool b = decltype("hello my world"_compStr)::starts_with<decltype("hello"_compStr)>; // b = true
```

### ends_with
compString::ends_with\<compString str> -> bool <br>
Checks wether compString end with the string str.<br>
Example:<br>
```cpp
constexpr bool b = decltype("hello my world"_compStr)::starts_ends<decltype("word"_compStr)>; // b = false
```

### substr
compString::substr\<size_t pos, size_t count> -> compString<br>
Returns a compString that is the substr of compString in range \[pos,min(pos+count,compString::size)].<br>
Example:<br>
```cpp
using retStr = decltype("hello my world"_compStr)::substr<3,4>; // retStr contains "lo m"
```

### substrLR
compString::substr\<size_t pos1, size_t pos2> -> compString<br>
Returns a compString that is the substr of compString in range \[pos1,min(pos2,compString::size)].<br>
Pos1 must be lesser than pos2.<br>
Example:<br>
```cpp
using retStr = decltype("hello my world"_compStr)::substr<3,8>; // retStr contains "lo my"
```

 

### erase_if
compString::erase_If\<typename pred> -> compString<br>
pred is the predicate that determines wether, a character is erased.<br>
pred must be default constructable and must have an operator().<br>
A lambda can be used.<br>
The operator() must have a function signature of one of the following:
- bool(char)
- bool(size_t)
- bool(char,size_t)
- bool(size_t,char)
  
Where the paramater char is the current character being considered.<br>
Where the paramater size_t is the index of the current character being considered.<br>
Where bool determines wether the character is deleted.<br>
Example:<br>
cpp++20 required for lambda-expressions in unevaluated contexts
```cpp
using pred = decltype( [](char c)->bool{
	return c =='l';
});
//pred will delete all 'l' in the string
using retStr = decltype("hello world"_compStr)::erase_if<pred>; // retStr contains "heo word"
```

### replace_if
compString::replace_if\<typename funcT> -> compString<br>
funcT is a functor type that will return the value that the current character will be replaced with.<br>
funcT must be default constructable and must have an operator().<br>
A lambda can be used.<br>
The operator() must have a function signature of one of the following:
- char(char)
- char(size_t)
- char(char,size_t)
- char(size_t,char)
  
Where the paramater char is the current character being considered.<br>
Where the paramater size_t is the index of the current character being considered.<br>
Where return char determines with what character the current character is replaced with.<br>
Example:<br>
cpp++20 required for lambda-expressions in unevaluated contexts
```cpp
using replaceFunc = decltype( [](char c)->char{
	if(c == 'l')
		return 'z';
	else
		return c;
});
// replaceFunc will replace all 'l' with 'z'<br>
using retStr = decltype("hello world"_compStr)::replace_with<repalceFunc>; // retStr will contain "hezzo worzd"
```
