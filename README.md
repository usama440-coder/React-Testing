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
#### Assertions
* ```expect```
* function is the matcher one comes with jest
    * Jest DOM library for more matchers
```
test('This is our first test', () => {
    render(<Component />)
    const element = screen.getByText(/hello/i)
    expect element.toBeInTheDocument()
})
```
#### What to Test
* Component renders
* Component renders with props
* Component renders with different states
* Component reacts to events
* Don't test
    * Implementation details
    * Third party code
    * Not important to user POV

#### RTL Queries
* Methods that testing library provides
* For single element
    * getBy
    * queryBy
    * findBy
* For multiple elements
    * getAllBy
    * queryAllBy
    * findAllBy
* Add a suffix
    * Role
    * LabelText
    * PlaceHolderText
    * Text
    * DisplayValue
    * AltText
    * Title
    * TestId
#### Query Methods
```
const element = screen.getByText('Hello')
```
* **getByRole**
    * like h1 has heading Role
    * https://www.w3.org/TR/html-aria/#docconformance
```
//for input type text
const element = screen.getByRole('textbox')

//for input type select
const element = screen.getByRole('combobox')

//for input type button
const element = screen.getByRole('button')
```
* Methods other than 'All', match just one element
    * Suppose 2 heading elements, it will throw an error
    * Remedy is 'name' or 'level'
```
const element = screen.getByRole('heading', {
    name: 'Heading 1'
})
```
* For different levels
```
const element = screen.getByRole('heading', {
    level: 1
})
```
* **getByLabelText**
```
<lable htmlFor='username'>Username</lable>
<input type='text' id='username' /

const element = screen.getByLabelText('Username')

// if one is name for text and one is for select
const element = screen.getByLabelText('Name', {
    selector: 'input'
})
```
* **getByPlaceholderText**
```
const element = screen.getByPlaceholderText('Email')
```
* **getByText**
```
const element = screen.getByText('Email')
```
* **getByDisplayValue**
```
<input type='text' value='Usama' onChange={() => {}}/>
const element = screen.getByDisplayValue('Usama')
```
* **getByAltText**
```
<img src='some' alt='image'/>
const element = screen.getByAltText('image')
```
* **getByTitle**
```
<span title='close'>Close</span>
const element = screen.getByTitle('close')
```
* **getByTestId**
```
<div data-testid='custom-element'></div>
const element = screen.getByTestId('custom-testid')
```
#### Priority Order
* Which query method to use and when
    1. getByRole
    2. getByLabelText
    3. getByPlaceholderText
    4. getByText
    5. getByDisplayValue
    6. getByAltText
    7. getByTitle
    8. getByTestId (specially when dynamic)




