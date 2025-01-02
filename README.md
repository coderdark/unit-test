# Unit-test
Using vitest (https://vitest.dev/guide/) but this can also be use with Jest testing.

## NOTES
+ Make your test simple and short when possible!!!
+ Dont attach your test to your styles - meaning dont use css styles names to test an element.  css styles name get apply to different elements and the name could change.
+ Use `data-testid` to provide the element a name to be used in testing

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

## Testing Playground Extension
Chrome extension, it is like inspect element but provides the best possible selector for your selected element. It also provides a sample code to use it with `@testing-library`...example `screen.getRole`  - https://chromewebstore.google.com/detail/testing-playground/hejbmebodbijjdhflfknehhcgaklhano

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
It is a library that provides helpers to work in the DOM. Some of those helpers are `screen` (browser window), `render` and `act` (act is waits for a rerender).  You can import it using vanilla js like `'@testing-library/dom';` or react `'@testing-library/react';` or svelte `'@testing-library/svelte';` or vue `'@testing-library/vue';`
+ @testing-library/<lib_name>
```
import { screen, fireEvent } from '@testing-library/dom';
import { createButton } from './button.js';

describe('createButton', () => {
  afterEach(() => {
    document.body.innerHTML = '';
  })

  it('should create a button element', () => {
    document.body.appendChild(createButton());

    const button = screen.getByRole('button', { name: 'Click Me' });

    expect(button).toBeInTheDocument()
  });

  it('should have the text "Click Me"', () => {
    document.body.appendChild(createButton());

    const button = screen.getByRole('button', { name: 'Click Me' });

    fireEvent(button, new MouseEvent('click'));

    expect(button.textContent).toBe('Clicked!');
  });
});
```
+ @testing-library/user-event
This package allows you to perform user events like mouse click, mouse enter. You must use `async`/`await` for your function when using package. It is a layer of abstraction to simulate user events on your web app / browser page. Under the hood, `user-event` package uses `fireEvent` (low level api).  https://stackoverflow.com/questions/64006345/react-testing-library-when-to-use-userevent-click-and-when-to-use-fireevent
```
import { screen, fireEvent } from '@testing-library/dom';
import { createButton } from './button.js';
import userEvent from '@testing-library/user-event';

describe('createButton', () => {
  afterEach(() => {
    document.body.innerHTML = '';
  })

  it('should change the text to "Clicked!" when clicked', async () => {
    document.body.appendChild(createButton());

    const button = screen.getByRole('button', { name: 'Click Me' });

   await userEvent.click(button);
   
    expect(button.textContent).toBe('Clicked!');
  });
});
```

## Testing React using @testing-library/react
If your component does not have a way to identify it for testing, Use `data-testid` to provide the element a name to be used in testing.  __When getting an element using `screen.gtByRole` is a good practice/pattern to use case-insensitive regex incase the casing changes in the button or other changes.__ See example below.
```
//Implementation
import React from 'react';
import { useReducer, useEffect } from 'react';
import { reducer } from './reducer';

export const Counter = ({initialCount = 0}) => {
  const [state, dispatch] = useReducer(reducer, { count: initialCount });
  const unit = state.count === 1 ? 'day' : 'days';

  useEffect(() => {
    window.document.title = `${state.count} ${unit} — Accident Counter`;
  }, [state.count]);

  return (
    <main className="flex h-screen items-center justify-center">
      <div className="space-y-8 rounded-md border border-slate-400 bg-white p-8 text-center shadow-lg">
        <div className="space-y-4">
          <div data-testid="counter-count" className="text-8xl font-semibold">
            {state.count}
          </div>
          <p>
            <span data-testid="counter-unit">{unit}</span> since the last
            JavaScript-related accident.
          </p>
        </div>
        <div className="flex gap-4">
          <button onClick={() => dispatch({ type: 'increment' })}>
            Increment
          </button>
          <button
            onClick={() => dispatch({ type: 'decrement' })}
            disabled={state.count === 0}
          >
            Decrement
          </button>
          <button
            onClick={() => dispatch({ type: 'reset' })}
            disabled={state.count === 0}
          >
            Reset
          </button>
        </div>
      </div>
    </main>
  );
};


//Test
import { act, render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import '@testing-library/jest-dom/vitest';

import { Counter } from './counter';

describe('Counter ', () => {
  it('renders with an initial count of 0', () => {
    render(<Counter />);
    const counter = screen.getByTestId('counter-count');
    expect(counter).toHaveTextContent('0');
  });

  it('disables the "Decrement" and "Reset" buttons when the count is 0', () => {
    render(<Counter />);
    const decrementButton = screen.getByRole('button', { name: /decrement/i }); //case insensitive pattern 
    const resetButton = screen.getByRole('button', { name: /reset/i });  //case insensitive pattern 

    expect(decrementButton).toBeDisabled();
    expect(resetButton).toBeDisabled();
  });

  it('displays "days" when the count is 0', () => {
    render(<Counter />);
    const unit = screen.getByTestId('counter-unit');

    expect(unit).toHaveTextContent('days');
  });

  it('increments the count when the "Increment" button is clicked', async () => {
    render(<Counter />);
    const counter = screen.getByTestId('counter-count');
    const incrementButton = screen.getByRole('button', {
      name: /increment/i,
    });

    //await userEvent.click(incrementButton); //this may not work because the dom needs to be rerendered

    await act(async () => {
      await userEvent.click(incrementButton);
    });

    expect(counter).toHaveTextContent('1');
  });

  it('displays "day" when the count is 1', async () => {
    render(<Counter initialCount={1} />);
    const unit = screen.getByTestId('counter-unit');
    // const button = screen.getByRole('button', {
    //   name: /increment/i,
    // });
    //
    // await act(async () => {
    //   await userEvent.click(button);
    // })

    expect(unit).toHaveTextContent('day');
  });

  it('decrements the count when the "Decrement" button is clicked', async () => {
    render(<Counter initialCount={2} />);
    const counter = screen.getByTestId('counter-count');
    const decrementButton = screen.getByRole('button', {
      name: /decrement/i,
    });

    expect(decrementButton).not.toBeDisabled();

    await act(async () => {
      await userEvent.click(decrementButton);
    });

    expect(counter).toHaveTextContent('1');
  });

  it('does not allow decrementing below 0', async () => {
    render(<Counter />);
    const counter = screen.getByTestId('counter-count');
    const button = screen.getByRole('button', {
      name: /decrement/i,
    });

    await act(async () => {
      await userEvent.click(button);
    });

    expect(counter).toHaveTextContent('0');
  });

  it('resets the count when the "Reset" button is clicked', async () => {
    render(<Counter />);
    const counter = screen.getByTestId('counter-count');
    const ibutton = screen.getByRole('button', {
      name: /increment/i,
    });
    const rbutton = screen.getByRole('button', {
      name: /reset/i,
    });

    await userEvent.click(ibutton);

    expect(counter).toHaveTextContent('1');

    await userEvent.click(rbutton);

    expect(counter).toHaveTextContent('0');
  });

  it('disables the "Decrement" and "Reset" buttons when the count is 0', () => {
    render(<Counter />);
    const counter = screen.getByTestId('counter-count');
    const ibutton = screen.getByRole('button', {
      name: /increment/i,
    });
    const dbutton = screen.getByRole('button', {
      name: /decrement/i,
    });
    const rbutton = screen.getByRole('button', {
      name: /reset/i,
    });

    expect(counter).toHaveTextContent('0');
    expect(dbutton).toBeDisabled();
    expect(rbutton).toBeDisabled();
  });

  it('updates the document title based on the count', async () => {
    const { getByRole } = render(<Counter />);
    const incrementButton = getByRole('button', {
      name: /increment/i,
    });

    await act(async () => {
      await userEvent.click(incrementButton);
    });

    expect(document.title).toEqual(expect.stringContaining('1 day'));
  });
});
```
## Searching Dom
Using `screen` from the `@testing-library/react`, you can use the `find` (Async, keeps retrying depending on the timeout provided) or `get` methods to find an element within the DOM.  
