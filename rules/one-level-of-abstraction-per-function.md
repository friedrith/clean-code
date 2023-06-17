# One level of abstraction per function

## Theory

The principle of coding is grouping complexity in function recursively until all your feature is implemented.

You can define a level of abstraction as the level of knowledge to understand a part of the code.

For example a product manager with some tech skills might understand some part of your code.
A junior might understand the code a bit deeper but will still be lost while diving deeper.

So your code is like an onion. The top layers contains high level of abstraction where everything is easily understandable while the deepest levels are
understanble by only senior devs.

This rule suggests to keep one level of abstraction per function so nobody gets lost in the middle of a function.

The levels of abstraction should change only by changing the function.

It requires a good empathy to be aware of the changes of level of abstraction of the code you are writing. But as a group effort,
it is really awarding since everybody will be able to understand the scope of their own capabilities in the project.

## Practical examples

We often see code like below:

```ts
function renderDuration(departure: Date, arrival: Date) {
  const duration = arrival.getTime() - departure.getTime()

  const hours = durationInMinutes / 1000 / 60 / 60 // <-- this line has another level of abstraction

  const minutes = (durationInMinutes / 1000 / 60) % 60 // <-- this line has another level of abstraction

  return `${hours}h ${minutes}min`
}
```

In this previous example, the first line and the end of the function is clearly understandable by a junior or even a product owner while 2 lines require more
mathematical skills.

Following the rule "One level of abstraction per function", you should have encapsulate these 2 lines so the complexity is not visible in the function `renderDuration`:

```ts
function extractHours(durationInMilliseconds: number) {
  return durationInMinutes / 1000 / 60 / 60
}

function extractMinutes(durationInMilliseconds: number) {
  return (durationInMinutes / 1000 / 60) % 60
}

function renderDuration(departure: Date, arrival: Date) {
  const duration = arrival.getTime() - departure.getTime()

  const hours = extractHours(durationInMinutes)

  const minutes = extractMinutes(durationInMinutes)

  return `${hours}h ${minutes}min`
}
```
