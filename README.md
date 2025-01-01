# Unit-test
Using vitest (https://vitest.dev/guide/) but this can also be use with Jest testing.

## TDD (Test Driven Development)
+ Write your test first which it will fail - red
+ Write you function for the test and should pass - green

## Test Coverage
+ Easy to get to 100%
+ Only counts the code that you test

## Unhappy Path
+ Undefined
+ Edge Cases
+ Be pessimistic

## Skipped Test
To skipped a test you can add .todo to the describe. This will skipped the test.  Removing it will excecute the test.
```
describe.todo('add', () => { //this test is skipped
  it('should add two positive numbers', () => {
    expect(add(1, 2)).toBe(4);
  });
});
```
## Throw Error
Here the `toThrow` is looking for `is not`.  As long as the error being thrown has `is not` withing the string it throws, the test pass.
Also notice in the test that the expect is not calling the add function directly but it is using an annonymous function that calls the add function since we are throwing an error.
```
//Implementation
export const add = (a, b) => {
  if (typeof a === 'string') a = Number(a);
  if (typeof b === 'string') b = Number(b);

  if (isNaN(a)) throw new Error('The first argument is not a number');
  if (isNaN(b)) throw new Error('The second argument is not a number');

  return a + b;
};

//Test
it('should get really angry if a first string param can not be parse into a number', () => {
    expect(() => add('potato', 1)).toThrow( //annonymous function is calling the add function here since we are throwing an error
      'is not',
    );
  });
```
