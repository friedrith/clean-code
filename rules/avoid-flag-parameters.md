# Avoid flag parameters

## Theory

When one of your function or component has a lot of parameters, it is sometimes the clue of a mistake in the architecture.

Either several parameters may be grouped together because they are properties of a same object like `filename` and `fileSize` may be grouped as `file: { name, size }`, either they are flag parameters used to trigger one behavior or another in the function.

Boolean are often good examples of flag parameters. They should be avoided as much as possible because they reduce the readibility of both the function calls and function definitions.

## Practical examples

> The issue of the function definition readibility is detailed in the rule [Prefer polymorphism to conditions]('./prefer-polymorphism-to-conditions.md). This rule also explains the solution to these flags parameters.

Given a function to render the price with 2 different behavior for euros and dollar using a boolean.

```ts
function renderTotalPrice(prices: number[], inDollar: boolean) {
  const sum = prices.reduce((sum, price) => sum + price, 0)

  const priceWithCurrency = inDollar ? `$${sum} USD` : `${sum} €`

  return `total price: ${priceWithCurrency}`
}
```

The way to call this function would be `renderTotalPrice([10, 20], true)` or `renderTotalPrice([10, 20], true)`. Without reading the function definition, it is hard to understand what this parameter is doing.

You could use a intermediate variable:

```ts
const inDollar = true

renderTotalPrice([10, 20], inDollar) // quite understandable

const inDollar = false

renderTotalPrice([10, 20], inDollar) // not understandable. If it is not dollar, what is it.
```
