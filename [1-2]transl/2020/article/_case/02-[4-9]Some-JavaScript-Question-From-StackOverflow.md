# [4-9]Some-JavaScript-Question-From-StackOverflow

- @doc 
  - https://stackoverflow.com/questions/tagged/javascript
  - https://stackoverflow.com/search?q=react+hooks

# [4-1]Grammar

- How do JavaScript closures work?
  - https://stackoverflow.com/questions/111102/how-do-javascript-closures-work

- How do I return the response from an asynchronous call?
  - https://stackoverflow.com/questions/14220321/how-do-i-return-the-response-from-an-asynchronous-call
- What is the difference between call and apply?
  - https://stackoverflow.com/questions/1986896/what-is-the-difference-between-call-and-apply

```
A for array and C for comma
```

# [4-2]React Hooks

- How to fix missing dependency warning when using useEffect React Hook?
  - https://stackoverflow.com/questions/55840294/how-to-fix-missing-dependency-warning-when-using-useeffect-react-hook

```
// 1)
useEffect(fetchBusinesses, [])

// 2)
useEffect(() => {
  function fetchBusinesses() {
    ...
  }
  fetchBusinesses()
}, [])

// 3)
const fetchBusinesses = useCallback(() => {
  ...
}, [])
useEffect(() => {
  fetchBusinesses()
}, [fetchBusinesses])

// 4)
useEffect(() => {
  fetchBusinesses()
}, []) // eslint-disable-line react-hooks/exhaustive-deps
```

- Why eslint-plugin-react-hooks doesn't warn when using react hooks conditionally?
  - https://stackoverflow.com/questions/55892009/why-eslint-plugin-react-hooks-doesnt-warn-when-using-react-hooks-conditionally

```js
"eslintConfig": {
  "extends": "react-app"
}
```

# [4-3]DOM

- How can I change an element's class with JavaScript?
  - https://stackoverflow.com/questions/195951/how-can-i-change-an-elements-class-with-javascript

```js
document.getElementById('id').classList.add('class');
document.getElementById('id').classList.remove('class');
document.getElementById('id').classList.toggle('class');
```