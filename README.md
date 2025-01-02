# Unit-test
Using vitest (https://vitest.dev/guide/) but this can also be use with Jest testing. Make your test simple and short when possible!!!

## Resources
+ JEST (Expect): https://jestjs.io/docs/expect
+ VITEST (Expect): https://vitest.dev/api/expect.html

## TDD (Test Driven Development)
Test driven development involves writing a test for a function (hopefully a small function), running the test to fail (since you write the test first before the function implementation) and then writing the code to make the test pass. 
+ Tests primarily unit tests, focusing on low-level functionality and edge cases.
+ Write your test first which it will fail - Red
+ Write you function for the test and should pass - Green
+ Refactor the code for efficiency and clarity - Refactor
+ Unhappy Paths
  + Undefined
  + Edge Cases
  + Be pessimistic

## BDD (Behavior Driven Development)
This test are written by developer, product manager or any stakeholder and the test engineer
+ Tests higher-level acceptance tests focusing on business outcomes and user interactions.
+ Derive acceptance criteria to write the test
+ Write test using Gherkin
+ Convert the Gherkin test to executable tests

## Test Coverage
+ Easy to get to 100%
+ Only counts the code that you test

## Run Test
+ `npm test` - runs all the test
+ `npm test button` - will run only the button files (in vitest, need to confirm in jest)

## Skipped Test
To skipped a test you can add `.todo` to the describe. This will skipped the test.  Removing it will excecute the test.
```
describe.todo('add', () => { //this test is skipped
  it('should add two positive numbers', () => {
    expect(add(1, 2)).toBe(4);
  });
});
```

## Failed Test
To fail a test you can add `.fails` to the test.  This will fail the test. 
```
test.fails('add two positive numbers', () => { //this test fails
    expect(add(1, 2)).toBe(4);
});
```

## Random
+ `expect.stringContaining`
To test random values, you can use `expect.stringContaining(<string_to_find>)`. Test the random value if possible.  If not possible then test what you can.
```
describe('person', () => { 
  it('should create a new person', () => {
    expect(person).toEqual({
      id: expect.stringContaining('person-'),
      name: 'Josh Allen'
    });
  });
});
```
+ `expect.any(Number|Date)`

## Object Properties (Example API responses)
To test SOME object properties in an object, you can use `expect.objectContaining`. See example below to see two different ways
```
describe('Player', () => {
  it('should create a player with name, last and role', () => {
    const player = new Player('Josh', 'Allen', 'Quarterback');

    //// You can do it this way
    expect(character.firstName).toBe('Josh');
    expect(character.lastName).toBe('Allen');
    expect(character.role).toBe('Quarterback');

    //// You can do it this way
    expect(player).toEqual(
      expect.objectContaining({
        firstName: 'Josh',
        lastName: 'Allen',
        role: 'Quarterback',
      }),
    );
  });

  it.todo('should allow you to increase the level', () => {});

  it.todo('should update the last modified date when leveling up', () => {});
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

## Referencial Equality
+ toBe - `expect('a').toBe('a')`
  + does not work for objects
+ toEqual - `expect({id:1,name:'Josh'}).toEqual({id:1,name:'Josh'})`
  + works with objects
  + test what you can test
 
## Before or After Hooks
If you need to repeat the creation of an object in each test or clean some settings after each test runs, you can use `beforeEach`, `beforeAll` or `afterEach`, `afterAll` respectevly. You can use `async` or `await` in these hooks.
```
const player = null;

beforeEach(() => {
  // creates a player before each test
   player = new Player('Josh', 'Allen', 'Quarterback');
})

afterEach(() => {
  // creates a player before each test
   player = null
})
```

## ASYNC / AWAIT
You can use `async` and `await` in your tests

## DOM
Your test run on node, node does not have the DOM api like the browser does.  In tests you have to fake the DOM, it is design to act like a browser. May is have some issues with browswer-specific stuff.  Jest uses `JSDOM`, vitest uses `JSDOM` or `HAPPY DOM` (Small and Lightweight). You may need to install these packages individually.  Vitest allows the user to choose between JSDOM or HAPPY DOM the default environment is `NODE`.
+ Setting Up Vitest
```
export default defineConfig({
  test: {
    globals: true,
    environment: 'jsdom', //or 'happy-dom'
    setupFiles: ['@testing-library/jest-dom/vitest'],
  },
});
```

## Testing-Library
It is a library that provides helpers to work in the DOM. Some of those helpers are `screen`, `render` and etc.  You can import it using vanilla js like `'@testing-library/dom';` or react `'@testing-library/react';` or svelte `'@testing-library/svelte';` or vue `'@testing-library/vue';`
