# Given, When, Then pattern

## Theory

In opposite of comments or documentation, tests are challenged every time to ensure they match the reality of the code.
So tests are the up to date explaination about how to use your functions. It is then very important to keep them readable.

A good approach is to use the pattern `Given`, `When`, `Then`.

This pattern consists in splitting your test in 3 steps clearly visible:

- `Given`: this step includes all the code to prepare an initial state. For pure functions, this step shouldn't exist but for React component, database, or more you will need this state. This step may often be common to several tests.
- `When`: this step includes the actions that are changing the initial state and should be tested
- `Then`: this step should include the code describing the expectations of somes changes coming from the `When` step

Everything described above is often necessary and is already there most of the time but the specificy of this pattern is to enforce
the visibility of these 3 different steps in your tests.

It is important to have these 3 steps clearly visible in the test since they have different goals.

The `Given` step describes the requirements of your function/components. The `When` step describes how to use your function/component. The `Then` describes what is the result of your function/component. So depending on what the developer is looking for while reading your tests, he might focus directly on one of these steps.

> This pattern is also named [Arrange, Act and Assert (AAA) Pattern](https://medium.com/@pjbgf/title-testing-code-ocd-and-the-aaa-pattern-df453975ab80).

## Practical examples

We can sometimes see tests like:

```ts
it('should change the state', () => {
  const input = mockInput()
  const worker = new Worker(input)
  const parameters = { foo: 'bar' }
  worker.optimize(parameters)
  const expectedData = mockData()
  expect(worker.getData()).toEqual(expectedData)
})
```

In order to follow the `Given, When, Then` pattern, you should do:

```ts
it('should change the state', () => {
  const input = mockInput()
  const parameters = { foo: 'bar' }
  const expectedData = mockData()
  const worker = new Worker(input)

  worker.optimize(parameters)

  expect(worker.getData()).toEqual(expectedData)
})
```

The separation between the 3 steps should be visible and one line between is a good solution. Just don't forget to not add any other line separator in your test all you will ruin this visibility. Don't do like below:

```ts
it('should change the state', () => {
  const input = mockInput()

  const parameters = { foo: 'bar' }
  const expectedData = mockData()
  const worker = new Worker()

  worker.optimize()

  expect(worker.getData()).toEqual(expectedData)
})
```

Sometimes we can also see, developers adding comments:

```ts
it('should change the state', () => {
  // GIVEN
  const input = mockInput()
  const parameters = { foo: 'bar' }
  const expectedData = mockData()
  const worker = new Worker(input)

  // WHEN
  worker.optimize(parameters)

  // THEN
  expect(worker.getData()).toEqual(expectedData)
})
```

This way is quite overkilled but still really better than no visibility at all.

If you tests seems to repeatitive, a good practice is to write intermediate functions. You can then prefix the function with the specific word to show more the separation between the 3 steps.

```ts
function givenAWorker() {
  const input = mockInput()
  return new Worker()
}

function whenWorkerOptimize(worker: Worker) {
  const parameters = { foo: 'bar' }
  worker.optimize(parameters)
}

function thenGetDataShouldReturnOptimizedData(worker: Worker) {
  const expectedData = mockData()
  expect(worker.getData()).toEqual(expectedData)
}

it('should change the state', () => {
  const worker = givenAWorker()

  whenWorkerOptimize(worker)

  thenGetDataShouldReturnOptimizedData(worker)
})
```
