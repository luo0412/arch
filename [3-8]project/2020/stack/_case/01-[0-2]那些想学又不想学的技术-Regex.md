# [0-2]那些想学又不想学的技术-Regex

> @todo

# 工具

- Hutool-ReUtil
- JavaVerbalExpressions
  - @code https://github.com/VerbalExpressions/JavaVerbalExpressions

```java
VerbalExpression testRegex = VerbalExpression.regex()
                                                .startOfLine().then("http").maybe("s")
	           				.then("://")
	           				.maybe("www.").anythingBut(" ")
	           				.endOfLine()
	           				.build();

// Create an example URL
String url = "https://www.google.com";

// Use VerbalExpression's testExact() method to test if the entire string matches the regex
testRegex.testExact(url); //True

testRegex.toString(); // Outputs the regex used:
                      // ^(?:http)(?:s)?(?:\:\/\/)(?:www\.)?(?:[^\ ]*)$
```

