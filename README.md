# 📝 String Library Project — `clsString`

A lightweight, beginner-friendly C++ string utility library that wraps and extends standard string operations through a clean, object-oriented interface. The library can be used both **statically** (without creating an object) and **as an instance** (for working with a stored string value).

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Implemented String Operations](#-implemented-string-operations)
  - [Case Conversion](#-case-conversion)
  - [Counting Operations](#-counting-operations)
  - [Splitting & Joining](#-splitting--joining)
  - [Trimming](#-trimming)
  - [Searching & Replacing](#-searching--replacing)
  - [Vowel Utilities](#-vowel-utilities)
  - [Word Utilities](#-word-utilities)
  - [Reversal](#-reversal)
  - [Cleaning](#-cleaning)
- [OOP Concepts Applied](#-oop-concepts-applied)
- [Example Usage](#-example-usage)
- [Project Structure](#-project-structure)
- [Design Notes](#-design-notes)
- [Future Improvements](#-future-improvements)
- [Author](#-author)

---

## 🔍 Overview

`clsString` is a self-contained C++ header library (`clsString.h`) that provides a rich set of string manipulation utilities. It was built to go beyond what the standard `<string>` library offers out of the box, adding operations like word-level manipulation, vowel detection, trimming, splitting, joining, and more — all in a simple and readable API.

The library is designed with **dual-mode usage** in mind: every major operation is available as a **static method** (called directly on the class) and as a **non-static instance method** (called on a stored string value). This gives developers flexibility to use it however they prefer.

---

## ✨ Features

- ✅ Upper / lower case conversion (entire string or first letters only)
- ✅ Invert letter casing character by character
- ✅ Count letters, capital letters, small letters, vowels, words, and specific characters
- ✅ Split a string by any custom delimiter into a `vector<string>`
- ✅ Join a vector of strings or an array of strings back with a delimiter
- ✅ Trim whitespace from the left, right, or both sides of a string
- ✅ Find and replace any substring or word within a string
- ✅ Remove all punctuation characters from a string
- ✅ Reverse the order of words in a string
- ✅ Detect vowel characters
- ✅ Print first letters, vowels, or individual words
- ✅ Get the length of a string
- ✅ Dual-mode API: static methods + instance methods on a stored value

---

## 🔧 Implemented String Operations

### 🔠 Case Conversion

These methods change the casing of letters in a string.

| Method | Description |
|---|---|
| `UpperAllString(string)` | Converts every character to uppercase |
| `LowerAllString(string)` | Converts every character to lowercase |
| `UpperFirstLetterOfEachWord(string)` | Capitalizes the first letter of each word |
| `LowerFirstLetterOfEachWord(string)` | Lowercases the first letter of each word |
| `InvertLetterCase(char)` | Flips a single character: uppercase → lowercase and vice versa |
| `InvertAllLettersCase(string)` | Inverts the case of every character in the string |

Each method exists in both a **static form** (pass the string as a parameter) and a **non-static form** (operates on the stored `_Value`).

```cpp
string result = clsString::UpperAllString("hello world");
// result → "HELLO WORLD"

clsString s("hello world");
s.UpperFirstLetterOfEachWord();
// s.Value → "Hello World"
```

---

### 🔢 Counting Operations

Methods to count various elements within a string.

| Method | Description |
|---|---|
| `Length(string)` | Returns the total number of characters |
| `CountLetters(string, enWhatToCount)` | Counts all letters, or only capitals / small letters |
| `CountCapitalLetters(string)` | Counts uppercase letters only |
| `CountSmallLetters(string)` | Counts lowercase letters only |
| `CountSpecificLetter(string, char, bool)` | Counts occurrences of a specific character; optional case-sensitive flag |
| `CountVowels(string)` | Counts vowel characters (a, e, i, o, u) |
| `CountWords(string)` | Counts the number of words (space-delimited) |

The `CountLetters` method uses the `enWhatToCount` enum:

```cpp
enum enWhatToCount { SmallLetters = 1, CapitalLetters = 2, All = 3 };
```

```cpp
short caps   = clsString::CountCapitalLetters("Hello World");  // → 2
short vowels = clsString::CountVowels("Hello World");          // → 3
short words  = clsString::CountWords("one two three");         // → 3

// Count occurrences of 'l' (case-insensitive)
short count  = clsString::CountSpecificLetter("Hello World", 'l', false); // → 3
```

---

### ✂️ Splitting & Joining

Parse a string into parts, or reassemble parts back into a string.

| Method | Description |
|---|---|
| `Split(string, string delim)` | Splits a string by a delimiter → returns `vector<string>` |
| `JoinString(vector<string>, string delim)` | Joins a vector of strings with a delimiter |
| `JoinString(string[], short length, string delim)` | Joins a C-style string array with a delimiter |

```cpp
vector<string> parts = clsString::Split("one#two#three", "#");
// parts → {"one", "two", "three"}

string joined = clsString::JoinString(parts, " - ");
// joined → "one - two - three"
```

> 💡 `Split` keeps empty tokens — it does not skip empty strings between consecutive delimiters. This is a deliberate design choice for consistent field parsing (e.g., reading `#//#`-separated data files).

---

### 🧹 Trimming

Remove unwanted leading or trailing spaces.

| Method | Description |
|---|---|
| `TrimLeft(string)` | Removes leading whitespace from the left |
| `TrimRight(string)` | Removes trailing whitespace from the right |
| `Trim(string)` | Removes whitespace from both sides (calls `TrimLeft` + `TrimRight`) |

```cpp
string s = "   hello world   ";

clsString::TrimLeft(s);   // → "hello world   "
clsString::TrimRight(s);  // → "   hello world"
clsString::Trim(s);       // → "hello world"
```

---

### 🔍 Searching & Replacing

Find and replace substrings anywhere in a string.

| Method | Description |
|---|---|
| `ReplaceWord(string, string toFind, string toReplace)` | Replaces **all** occurrences of a substring with another |

```cpp
string result = clsString::ReplaceWord("I love C, C is great", "C", "C++");
// result → "I love C++, C++ is great"
```

> The method replaces **every** occurrence of the target substring, not just the first one. It uses `std::string::find` and `std::string::replace` internally in a loop.

---

### 🅰️ Vowel Utilities

Detect and display vowel characters in a string.

| Method | Description |
|---|---|
| `IsVowel(char)` | Returns `true` if the character is a vowel (a, e, i, o, u) — case-insensitive |
| `CountVowels(string)` | Returns the total count of vowels |
| `PrintVowels(string)` | Prints all vowel characters to the console |

```cpp
bool check = clsString::IsVowel('E');  // → true

clsString::PrintVowels("Hello World");
// Output: e o o
```

---

### 📖 Word Utilities

Work with individual words inside a string.

| Method | Description |
|---|---|
| `CountWords(string)` | Counts the number of space-separated words |
| `PrintEachWordInString(string)` | Prints each word on a separate line |
| `PrintFirstLetterOfEachWord(string)` | Prints the first letter of each word on a separate line |

```cpp
clsString::PrintEachWordInString("one two three");
// Output:
// one
// two
// three

clsString::PrintFirstLetterOfEachWord("Hello World");
// Output:
// H
// W
```

---

### 🔄 Reversal

| Method | Description |
|---|---|
| `ReverseWordsInString(string)` | Reverses the **order of words** in a string (not the characters) |

```cpp
string result = clsString::ReverseWordsInString("one two three");
// result → "three two one"
```

> Note: this reverses word order, not character order. `"Hello World"` becomes `"World Hello"`, not `"dlroW olleH"`.

---

### 🧽 Cleaning

| Method | Description |
|---|---|
| `RemovePunctuations(string)` | Removes all punctuation characters (using `ispunct`) |

```cpp
string result = clsString::RemovePunctuations("Hello, World! How are you?");
// result → "Hello World How are you"
```

---

## 🧱 OOP Concepts Applied

| Concept | How It's Used |
|---|---|
| **Encapsulation** | The internal string value `_Value` is `private`; accessed only through `GetValue()` / `SetValue()` or the `Value` property |
| **Properties** | `__declspec(property(get=..., put=...))` provides a clean `Value` accessor that looks like a public field |
| **Static Methods** | All major operations are `static` — they can be called without instantiating `clsString` at all |
| **Method Overloading** | Every static method has a matching non-static instance version with the same name but no parameters |
| **Enumerations** | `enWhatToCount` (`SmallLetters`, `CapitalLetters`, `All`) makes the `CountLetters` API readable and type-safe |
| **Default Parameters** | Methods like `CountLetters` and `CountSpecificLetter` have sensible defaults so callers only pass what they need |
| **Dual Constructor** | A default constructor (empty string) and a parameterized constructor (initial value) are both provided |

---

## 💡 Example Usage

### Static usage (no object needed)

```cpp
#include "clsString.h"
#include <iostream>
using namespace std;

int main() {
    // Case conversion
    cout << clsString::UpperAllString("hello");          // HELLO
    cout << clsString::LowerAllString("WORLD");          // world

    // Counting
    cout << clsString::CountWords("one two three");      // 3
    cout << clsString::CountVowels("Hello World");       // 3

    // Split and join
    vector<string> v = clsString::Split("a,b,c", ",");  // {"a", "b", "c"}
    cout << clsString::JoinString(v, " | ");             // a | b | c

    // Trim
    cout << clsString::Trim("  hello  ");                // hello

    // Replace
    cout << clsString::ReplaceWord("cat and cat", "cat", "dog"); // dog and dog

    // Remove punctuation
    cout << clsString::RemovePunctuations("Hi! How are you?");   // Hi How are you

    // Reverse words
    cout << clsString::ReverseWordsInString("one two three");    // three two one

    return 0;
}
```

### Instance usage (with stored value)

```cpp
#include "clsString.h"
#include <iostream>
using namespace std;

int main() {
    clsString s("  hello world  ");

    s.Trim();
    cout << s.Value;   // "hello world"

    s.UpperFirstLetterOfEachWord();
    cout << s.Value;   // "Hello World"

    s.InvertAllLettersCase();
    cout << s.Value;   // "hELLO wORLD"

    cout << s.CountVowels();   // 3
    cout << s.CountWords();    // 2

    return 0;
}
```

---

## 📁 Project Structure

```
StringLibrary/
│
└── clsString.h      # Single-header library — everything is here
```

This is a **single-header library**. No `.cpp` file, no compilation step, no linking required. Just include the header and start using it.

---

## 🎨 Design Notes

- **Single-header design:** The entire library lives in one `.h` file. This makes it trivial to add to any project — just copy and include.
- **Static-first approach:** All logic is implemented in static methods. The non-static instance methods are thin wrappers that call their static counterparts using `_Value`. This avoids code duplication.
- **Standard library wrappers:** Several methods are thin wrappers around standard C++ functions (`toupper`, `tolower`, `ispunct`, `find`, `substr`, `replace`, `erase`). The value is in composing these into readable, named operations.
- **Space as the word delimiter:** Word-level operations (`CountWords`, `Split` by words, `ReverseWordsInString`) assume a single space as the delimiter. Multiple consecutive spaces may affect results.
- **No exception handling:** The library trusts the caller to pass valid inputs. There is no bounds-checking or empty-string handling beyond what the standard library provides.
- **`__declspec(property)`:** This is a Microsoft-specific extension (MSVC). It allows `s.Value = "..."` and `s.Value` to work like a real public field while internally calling `SetValue` / `GetValue`. This will not compile on GCC or Clang without modification.
