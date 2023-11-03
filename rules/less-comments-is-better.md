# Less comments a better

## Theory

A common approach in projects who want to follow good practices is adding comments to describe the code.

By adding comments, the code is supposed to be easier and faster to understand. This approach is a natural but that struggles when facing reality of development.

These are 3 reasons why you should use less comments:

- developer will still need to read the code since no comment will be as accurate to describe your feature as your code (the only way would be to write as many lines as your code in your documentation and you definitely don't want that)
- comments get out dated very fast
- it takes time to write good comments

Behind this flashy title, the goal is not to remove all documentation of your code. The goal is to transform your code into its own documentation so you don't need to use comments.

_There are certainly times when code makes a poor vehicle for explanation. Unfortunately, many programmers have taken this to mean that code is seldom, if ever, a good means for explanation. This is patently false._ from _Clean code_ by Robert C. Martin p55.

> This rule applies to comments and generally all documentation related to the code explanation. But it doesn't apply to API documentation that is often designed to be used without even diving in the code. [Mui documentation](https://mui.com/material-ui/getting-started/overview/) is for example very useful and shouldn't be remove. It also doesn't apply to CONTRIBUTING.md or README.md that are often documentation about the team practices or how to start the project.

## Practical examples

We can see code like this sometimes:

```ts
function renderDuration(durationInMinutes: number) {
  // extract hours
  const hours = durationInMinutes / 60

  // extract minutes
  const minutes = durationInMinutes % 60

  // if less than one hour we only display minutes
  if (hours === 0) {
    return `${minutes}min`
  }

  return `${hours}h ${minutes}min`
}
```

But it is possible to make the code more explicit to make the comments useless.

```ts

// there is no shame to write a function used only once
function extractHours = (durationInMinutes: number) {
  return durationInMinutes / 60
}

function extractMinutes = (durationInMinutes: number) {
  return durationInMinutes % 60
}

// using a function to describe a condition is a nice trick
function lessThanOneHour = (hours: number) {
  return hours === 0
}

function renderDuration(durationInMinutes: number) {

  const hours = extractHours(durationInMinutes)
  const minutes = extractMinutes(durationInMinutes)

  if (lessThanOneHour(hours)) {
    return `${minutes}min`
  }

  return `${hours}h ${minutes}min`
}
```

Yes your code is a bit longer at the end but you are almost sure it will be always up to date. Besides, this second approach brings other benefits that directly improves the
code quality.

By using intermediate functions, the complexity is hidden in lower functions and:

- it is easier to test and debug
- first level (function `renderDuration` in the example) is more understandable without technical skills and can be used as a specification.

This rule could be renamed "write more explicit code". But forcing yourself to write less comments naturally pushes to find new ways to document your code and not
only write cleaner code. So now when you write your code, you should think how I can make my code understandable by a first years old kid.
