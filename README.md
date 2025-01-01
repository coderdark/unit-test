# Unit-test
Using vitest (https://vitest.dev/guide/) but this can also be use with Jest testing.

## Skipped Test
To skipped a test you can add .todo to the describe. This will skipped the test.  Removing it will excecute the test.
```
describe.todo('add', () => { //this test is skipped
  it('should add two positive numbers', () => {
    expect(add(1, 2)).toBe(4);
  });
});
```
