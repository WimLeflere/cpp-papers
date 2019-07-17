Document number: P1679R1  
Date: 2019-05-26  
Project: WG21, Library Working Group  
Author: Wim Leflere <wim.leflere@gmail.com>

# String Contains function

## 1. Abstract
This paper proposes to add member function `contains` to class templates basic_string and basic_string_view.
This function checks, whether or not a string contains a given substring.

## 2. History
### 2.1. R0
Initial version
### 2.2. R1
Wording added

## 3. Motivation
Checking, whether or not a given string contains a given substring is a common task, that is missing from the standard library.

### 3.1. Other (standard) libraries
Standard libraries of many other programming languages include routines for performing such a check, for example:
* Java: class String contains method [<sup>1</sup>](#java_string)
* C#: class String Contains method [<sup>2</sup>](#csharp_string)
* Rust: class string contains method [<sup>3</sup>](#rust_string)

And so on.

Also, some C++ libraries (other than the standard library) that implement a string type include such methods.  
For example, Qt library has classes QString [<sup>4</sup>](#qstring) and QStringRef (analogous to std::string_view) which have contains member functions.

The teachability of C++ for people coming from other languages might improve, because they are already familiar with the `contains` function in a string type.

### 3.2. Why not find != npos?
The 'standard' [<sup>5</sup>](#contains_so) way of checking if a string contains a substring is to use the `find` member function.
```
if (str.find(substr) != std::string::npos)
	std::cout << "found!" << '\n';
```
But using `find` requires that one extra step of thinking when writing it.  
You're trying to do something positive (check if contains) but you have to do something negative (check inequality).

And one extra step when reading the code.  
Are we looking for the actual position? Or checking if the string contains a substring? Or checking if the string doesn't contain a substring?

A `contains` member function would make the intention of the programmer more clear and make the code more readable.
```
if (str.contains(substr))
	std::cout << "found!" << '\n';
```
The proposed change would improve teachability of C++ for beginners as the `contains` function better matches the intention of the programmer.
And because it is a simpler construct to write and remember than using `find`.

### 3.3. Three string checking Musketeers
The string `contains` function would complete the three string checking musketeers, together with the string prefix and suffix check, `starts_with` and `ends_with` [<sup>6</sup>](#string_checks).

## 4. Design considerations

### 4.1. Member function vs free function
This proposal adds member function `contains` to class templates basic_string and basic_string_view.  
Another considered option was to add a free function `contains` to namespace std, as in Boost [<sup>7</sup>](#boost_contains).   
The drawback of a free function is that the order of parameters of a free function is ambiguous, `contains(string, substring)` vs `contains(substring, string)`.

### 4.2. Overload set
Qt offers the following overload set (for QString and QStringRef):
```
bool contains(const QString &str, Qt::CaseSensitivity cs = ...) const
bool contains(QChar ch, Qt::CaseSensitivity cs = ...) const
bool contains(QLatin1String str, Qt::CaseSensitivity cs = ...) const
bool contains(const QStringRef &str, Qt::CaseSensitivity cs = ...) const
```
This proposal includes the following overloads:
```
// basic_string:
bool contains(charT ch) const noexcept;
bool contains(basic_string_view<charT, traits> str) const noexcept;
bool contains(const charT* str) const;

// basic_string_view:
constexpr bool contains(charT ch) const noexcept;
constexpr bool contains(basic_string_view<charT, traits> str) const noexcept;
constexpr bool contains(const charT* str) const;
```

## 5. Wording

## 6. References
1. <a name="java_string"></a>
Java™ Standard Edition 10 API. Class String, https://docs.oracle.com/javase/10/docs/api/java/lang/String.html#contains(java.lang.CharSequence)
2. <a name="csharp_string"></a>
.NET Core 2.2 API. String.Contains Method, https://docs.microsoft.com/en-us/dotnet/api/system.string.contains?view=netcore-2.2
3. <a name="rust_string"></a>
Rust 1.0 API. Struct std::string::String, https://doc.rust-lang.org/std/string/struct.String.html#method.contains
4. <a name="qstring"></a>
Qt 5.12 documentation. QString Class, https://doc.qt.io/qt-5/qstring.html#contains
5. <a name="contains_so"></a>
StackOverflow. C++ string contains, https://stackoverflow.com/questions/2340281/check-if-a-string-contains-a-string-in-c/2340309#2340309
6. <a name="string_checks"></a>
WG21 P0457R2. String Prefix and Suffix Checking, http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2017/p0457r2
7. <a name="boost_contains"></a>
Boost 1.70.0 documentation. boost\::algorithm\::contains function, https://www.boost.org/doc/libs/1_70_0/doc/html/boost/algorithm/contains.html
