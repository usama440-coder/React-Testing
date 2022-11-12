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

#### Test Driven Development (TDD)
* Write test before code (3 steps)
    1. Test
    2. Code
    3. Refactor
```
// test that component should print hello only and when props are given,
also print that props

// first case
test('This is our first test', () => {
    render(<Component />)
    const element = screen.getByText('Hello')
    expect element.toBeInTheDocument()
})

//second case
test('This is our first test', () => {
    render(<Component name='Usama'/>)
    const element = screen.getByText('Hello Usama')
    expect element.toBeInTheDocument()
})

//Component Code
export const Component = ({name}) => {
    return (
        <div>Hello {name}</div>
    )
}
```

#### Grouping Tests
* Nesting is possible
* Can use ```.only``` and ```.skip```
* 1 file is a suit
```
describe('test name', () => {
    <test 1>
    <test 2>
})
```
#### Filename Conventions
* ```filename.test.jsx```
* ```filename.spec.jsx```
    * ```it``` instead of ```render```
    * ```fit``` instead of ```only```
    * ```xit``` instead of ```skip```
* any filename inside ```__tests__``` filder
#### Code Coverage
* How much code is tested
    * Statement Coverage
    * Function Coverage
    * Line Coverage
    * Branch Coverage
* Add a script
```"coverage": "npm test --coverage --watchAll"``` 
* If you want to include just js or jsx file inside a particular folder
```
"coverage": "npm test --coverage --watchAll --collectCoverageFrom='src/components/**/*.{ts,tsx}'"
```
* Now do not include particular files
* Like exclude 'types.tx' file inside component folder
```
"coverage": "npm test --coverage --watchAll --collectCoverageFrom='!src/components/**/*.{types,stories,constants}.{ts,tsx}'"
```
* We want a threshold so, add the following in json file
```
"jest":{
    "coverageThreshold": {
        "global": {
            "branches": 80,
            "functions": 80,
            "lines": 80,
            "statements": -10
        }
    }
}
```
