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
