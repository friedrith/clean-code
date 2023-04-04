# Prefer polymorphism to if/else or switch/case

Given the following function:

```ts
function renderTotalPrice(prices: number[]) {
  const sum = prices.reduce((sum, price) => sum + price, 0)

  return `${sum} €`
}
```

When you are asked to manage also dollars, then the common behavior is to add a boolean or a option

```ts
// with boolean
function renderTotalPrice(prices: number[], inDollar: boolean) {
  const sum = prices.reduce((sum, price) => sum + price, 0)

  return inDollar ? `$${sum}` : `${sum} €`
}

// with option
function renderTotalPrice(prices: number[], currency: string) {
  const sum = prices.reduce((sum, price) => sum + price, 0)

  if (currency === 'dollar') {
    return `$${sum}`
  } else {
    return `${sum} €`
  }
}
```

But these approaches have the same major drawbacks:

- they are not scalable: everytime you need to add a currency you need to modify the function
- when developers read the code they need to pass through the complexity of each condition. Sometimes it may be really long and hard.

We suggest the alternative solution:

```ts
function renderTotalPrices(
  prices: number[],
  formatPrice: (n: number) => string
) {
  const sum = prices.reduce((sum, price) => sum + price, 0)

  return formatPrice(sum)
}

function formatEuro(price: number) {
  return `${sum} €`
}
function formatDollar(price: number) {
  return `$${sum}`
}

renderTotalPrices([1, 2, 3], formatEuro)
renderTotalPrices([1, 2, 3], formatDollar)
```

This approach based on polymorphism is more scalable. It also hides some complexity and helps to keep the code clean.

Benefits:

- make the function shorter (shorter is better)
- it is scalable to other modification (adding the currency symbol on the left or right depending on the locale for example, rounding the price)
