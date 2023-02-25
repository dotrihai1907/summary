# Testing React

## React Testing Library
- is a light-weight solution for testing React components.
- helps with:
  + rendering components into Virtual Dom (VDom)
  + searching VDom
  + interacting with VDom (clicking or entering text)
- needs a test runner (to find tests, run tests and make assertions) (`Jest`)

## Jest
- is recommened by `React Testing Library` (effective and easy to use)
- `Jest` watch mode:
  + watches for changes in files since last commit
  + only runs tests related to these files
  + no changes => no tests
  
## TTD (Test Driven Development)
 - write tests before writing code
 - determine functionality => write tests => tests fail => write code => tests pass
  
## Unit test
- `unit test` if: 
    + complex logic difficult to test via `functional test`
    + too many edge cases
 
- when to `unit test`:
    + could be covered by functional tests
    + for more complicated functions, `unit test` helps with:
      - covering all possible edge case
      - determining what caused functional test to fail

## React Testing Library
### Common Methods
- `render()`: mounts the component into `document.body`
```
render(<Component />)
```
- `unmount()`: causes the rendered component to be unmounted (using when your component is removed from the page)
```
  const { unmount } = render(<Options />);
  const scoopsSubtotal = screen.getByText("Scoops total: $", { exact: false });
  expect(scoopsSubtotal).toHaveTextContent("0.00");
  unmount();
```
- `act()`: makes sure all update related to `rendering`, `userEvent`, `data fetching` have been processed and applied to the DOM before making any assertions
```
act(() => {
  // render components
});
// make assertions
```

### Queries (screen query methods)
- Syntax: *command*[All]By`QueryType`
- *command*:
  + `get`: expect element to be in DOM
  + `query`: expect element not to be in DOM
  + `find`: expect element to appear `async`
  
- [All]:
  + (exclude): expect only one match
  + (include): expect more than one match
 
- `QueryType`:
  + `Role` (most prefered)
  + `AltText` (images)
  + `Text` (display element - non-interactive element)
  + `PlaceholderText`, `LabelText`, `DisplayValue` (form element)
  
```
expect(screen.getByRole('heading')).toHaveTextContent('hello there')
expect(screen.getByRole('button')).toBeDisabled()
```
### Async
- `async methods` return `Promises` => use `await` or `.then`
- `waitFor`: wait for any period of time, wait for your expectations to pass
```
await waitFor(() => expect(mockAPI).toHaveBeenCalledTimes(1))
```
- `waitForElementToBeRemoved`: wait for the removal of element(s) from the DOM
```
await waitForElementToBeRemoved(() =>
  screen.queryByText(/testing react/i)
);
```
### Events
- `fireEvent` trigger DOM event.
- `fireEvent.*` helpers for default event types:
  + `fireEvent.click(elementt)`
  + `fireEvent.mouseOver(element)` `(mouseMove | mouseDown | mouseUp)`
  + `element.focus()`
```
  const checkbox = screen.getByRole("checkbox", {
    name: /testing react/i,
  });
  await user.click(checkbox);
  expect(confirmButton).toBeEnabled();
```