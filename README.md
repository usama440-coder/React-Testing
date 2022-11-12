# React Testing

#### First Test
* Inside any component folder, make a file <compoenent.test.jsx>
```
// methods provided by RTL
import {render, screen} from '@testing-library/react'
import <Component> from '<dest.>'

// first argument is test name and second one is function
// render component (VDOM) and get 'hello' text on the screen
// It should be in the document
test('This is our first test', () => {
    render(<Component />)
    const element = screen.getByText(/hello/i)
    expect element.toBeInTheDocument()
})
```

