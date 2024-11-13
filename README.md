# unit-4-4-assignment-b

## Git Config
```
git config user.name "user"
git config user.email "email"
```

## Compiling and Running Java Programs
Note that since the shape classes are separate classes, you will need to compile ALL the files (at least one time).  You can do this by running
```
javac *.java
```
The star means to compile every file that is a Java file type.

Run your code by running
```
java Main.java
```

After you compile the shape classes, you only need to compile and run `Main.java` as usual.

# Problem 0
To help you with Problem 3, it might be useful to refamiliraize yourself with the "find and remove" algorithm.  Write a program which takes in two strings, a `sentence` and a `target`, and removes the `target` from the `sentence` (if the target exists).

The general steps are:
1. Determine if the `target` is in the `sentence` (use `indexOf()`)
2. If it is in the `sentence`, then get a substring of everything before the `target`
3. Get a substring of everything after the `target`
4. Combine the "before" and "after" substrings

# Problem 1 - CountSub
Write a program which takes in two Strings `word` and `target` and returns the number of occurrences of `target` in the `word`.  Assume all inputs are given in lowercase.

**Hint:** Check `N` letters at a time, where `N` is the length of the target.  Get a substring that is `target.length()` long, and see if it is equal to the `target`.  If it is, then count it.

**Hint:** See the slides for an example of checking two letters at at time.  How would you check three letters at a time?  Generalize this to check `N` letters at a time.

### Sample run
```md
Input word:
gagagigo the risen

Input target:
ga

The substring ga appears 2 times
```

# Problem 2 - CountProperContains
Write a program that takes in two Strings, `word` and `target`, and returns the number of times the `target` is *properly contained* within the `word`.

A substring is said to be *properly contained* if it meets both of the following conditions:
* The substring is the beginning of the String or is immediately preceded by a space (i.e., has a space before the substring)
* The substring is the end of the String or is immediately followed by a space (i.e., there's a space after the substring)

## Examples of substrings being *properly contained*:
* `"bada"` is properly contained in `"bada boom baby"`
* `"boom"` is properly contained in `"bada boom baby"`
* `"oom"` is properly contained in `"badab oom baby"`

## Non-examples of substrings being *properly contained*:
* `"bada"` is NOT properly contained in `"badaboom baby"` (there needs to be a space after the `"bada"`)
* `"dab"` is NOT properly contained in `"badaboom baby"`
* `"ada"` is NOT properly contained in `"badaboom baby"`
* `"schlay"` is NOT properly contained in `"badaboom baby"`

Sample Run
```md
Input word:
bada the badaboom the bobadabo baby

Input Target:
bada

The substring bada is properly contained 1 times
```

# Problem 3 - DeleteSub
Write a program which takes in two Strings `word` and `target` and removes all occurrences of `target` from `word`.  Return this new `String` with all instances of the `target` removed.  Assume all inputs are in lowercase.

One possible solution goes like this:
1. While the `target` can still be found in the `word` (use `indexOf()`)
2. Remove the `target` from the word (by combining a substring of everything before the `target` with everything after the `target`)

Sample run
```
Input word:
gagagigo the giga Risen

Input target:
gig

New String:
gagao the a risen
```

## Sample Solutions
```java
public static int countSub(String word, String target)
{
  int count = 0;

  // This stops at the Nth to last letter, where N is the
  // length of the target.  As in, I am checking N letters
  // at a time.
  for (int i = 0; i < word.length() - (target.length() - 1); i++)
  {
    String sub = word.substring(i, i + target.length());
    if (sub.equals(target))
    {
      count++;
    }
  }
  return count;
}

public static int countProperContains(String word, String target)
{
  int count = 0;
  for (int i = 0; i < word.length() - (target.length() - 1); i++)
  {
    String sub = word.substring(i, i+target.length());
    if (sub.equals(target))
    {
      int beginOfTarget = i;
      int endOfTarget = i+target.length();

      // Note: short circuiting prevents me from checking out of bounds when I attempt
      // to check for the letter at (begin-1, begin) and at (end, end+1)
      boolean beginOrSpaceBefore = (beginOfTarget == 0 || word.substring(beginOfTarget-1, beginOfTarget).equals(" "));
      boolean endOrSpaceAfter = (endOfTarget == word.length() || word.substring(endOfTarget, endOfTarget+1).equals(" "));
      if (beginOrSpaceBefore && endOrSpaceAfter)
      {
        count++;
      }
    }
  }
  return count;
}

public static String deleteSub(String word, String target)
{
  // As long as the target is found in the word, we remove it
  while (word.indexOf(target) != -1)
  {
    // Combine everything before the target with everything after target
    word = word.substring(0, indexOfTarget) + word.substring(indexOfTarget + target.length());
  }
  return word;
}
```
