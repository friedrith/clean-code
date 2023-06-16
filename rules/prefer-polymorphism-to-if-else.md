# Prefer polymorphism to if/else or switch/case

## Theory

Very often, you have a first feature `A` using a function `f` and you have to introduce another feature `B` that would require.
a code similar to the function `f`.
Sometimes generalization works and you are able to just modify `f` to handle features `A` and `B` indistinctly.
Sometimes the two features are too different and developers just add conditions to make the difference between the two features.
The result code after few years and more features added is a lot of if/else everywhere.

```ts
function f() {
  if (featureA) {
  } else {
  }

  // 200 lines later

  if (featureB) {
  } else {
  }
}
```

This situation really degradates the code readibility. If you are a developer debugging some part of the feature `A` and who arrives
in the function `f`, you will need to understand the code of the feature `A` and `B` to understand the function and be able to find the bug.
The approach based on conditions will make you slower to debug and fix bug. It is a wrong practice. With examples, you will understand how to do better.

## Practical examples

Given the following function return prices in euro like `100 €`:

```ts
function renderTotalPrice(prices: number[]) {
  const sum = prices.reduce((sum, price) => sum + price, 0)

  return `total price: ${sum} €`
}
```

When you are asked to manage also US dollars with a result like `$100 USD`, then the common behavior is to add another parameter to detect the difference between
euros and dollars:

```ts
// with boolean
function renderTotalPrice(prices: number[], inDollar: boolean) {
  const sum = prices.reduce((sum, price) => sum + price, 0)

  const priceWithCurrency = inDollar ? `$${sum} USD` : `${sum} €`

  return `total price: ${priceWithCurrency}`
}

// with option/enum
function renderTotalPrice(prices: number[], currency: Currency) {
  const sum = prices.reduce((sum, price) => sum + price, 0)

  const priceWithCurrency =
    currency === Currency.dollar ? `$${sum} USD` : `${sum} €`

  return `total price: ${priceWithCurrency}`
}
```

But these approaches have two same major drawbacks:

- they are not scalable: everytime you need to add a currency you need to modify the function
- when developers read the code they need to pass through the complexity of euros and dollars. For more complex features, it may be really long and hard.

We suggest the alternative solution:

```ts
function renderTotalPrices(
  prices: number[],
  addCurrency: (n: number) => string
) {
  const sum = prices.reduce((sum, price) => sum + price, 0)

  const priceWithCurrency = addCurrency(sum)

  return `total price: ${priceWithCurrency}`
}

function addEuro(price: number) {
  return `${sum} €`
}
function addUsDollar(price: number) {
  return `$${sum} USD`
}

renderTotalPrices([1, 2, 3], addEuro)
renderTotalPrices([1, 2, 3], addUsDollar)
```

This approach based on polymorphism is more scalable. It also hides some complexity and helps to keep the code clean.

Benefits:

- make the function shorter (shorter is better)
- it is scalable to other modification (adding the currency symbol on the left or right depending on the locale for example, rounding the price)

> Instead of polymorphism, another approach is often to break the function `f` in smaller functions.

```ts
function sumTotalPrice(prices: number[]) {
  return prices.reduce((sum, price) => sum + price, 0)
}

function addEuro(price: number) {
  return `${sum} €`
}
function addUsDollar(price: number) {
  return `$${sum} USD`
}

function renderTotalPrices(priceWithCurrency: string) {
  return `total price: ${priceWithCurrency}`
}

renderTotalPrices(sumTotalPrice([1, 2, 3]), addEuro)
renderTotalPrices(sumTotalPrice([1, 2, 3]), addUsDollar)
```

> But if this second approach works for small problems and should be promoted in these cases, it often doesn't work at larger scale because it
> pushes back the complexity to the parent function calling the function `f`
