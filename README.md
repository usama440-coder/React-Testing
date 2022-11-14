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

#### Query Multiple elements
* same as above but for multiple elements
* Test of role that it shoudl render all the elements correctly
```
// component
const Skills = ({skills}) => {
    return (
        <>
            <ul>
                skills.map((skill) => {
                    return <li key='skill'>skill</li>
                })
            </ul>
        </>
    )
}

// test
describe('skills', () => {
    const skills = ['React', 'Angular', 'Vue']
    
    test('it renders correctly', () => {
        render(<Skills skills={skills} />)
        const element = screen.getByRole('list')
        expect(element).toBeInTheDocument()
    })

    test('it should have all the list items', () => {
        render(<Skills skills={skills}/>)
        const element = screen.getAllByRole('listitem')
        expect(element).toHaveLength(skills.length)
    })
})
```

#### TextMatch
* First argument in the test could be
    * string
    * regex
    * function
```
screen.getByText('Hello') //string
screen.getByText(/Hello/i) //regex
screen.getByText((content) => content.startsWith('Hello')) //function
```
#### queryBy
* Let a component having if not user, show logged in button, otherwise show start button
```
const Skills = ({skills}) => {
    return (
        const [isLogged, setIsLogged] = useState(false)
        <>
            <ul>
                skills.map((skill) => {
                    return <li key='skill'>skill</li>
                })
            </ul>
        </>
        {
            isLogged ? (<button>Start</button>) : (<button onClick={()=>setIsLogged(true)}>Login</button>)
        }
    )
}

// test
describe('skills', () => {
    const skills = ['React', 'Angular', 'Vue']
    
    test('it renders correctly', () => {
        render(<Skills skills={skills} />)
        const element = screen.getByRole('list')
        expect(element).toBeInTheDocument()
    })

    test('it should have all the list items', () => {
        render(<Skills skills={skills}/>)
        const element = screen.getAllByRole('listitem')
        expect(element).toHaveLength(skills.length)
    })

    test('render login button', () => {
        render(<Skills skills={skills} />)
        const element = screen.getByRole('button',{
            name: 'Login'
        })
        expect(element).toBeInTheDocument()
    })

    test('render start button', () => {
        render(<Skills skills={skills} />)
        const element = screen.queryByRole('button',{
            name: 'Start'
        })
        expect(element).not.toBeInTheDocument()
    })
})
```
#### findBy
* If we have to wait for some event to occur
* Asynchronous requests
```
// Component
const [isLogged, setIsLogged] = useState(false)
useEffect(() => {
    setTimeout(() => {
        setIsLogged(true)
    }, 1001)
},[])

//test
test('render start button', async () => {
        render(<Skills skills={skills} />)
        const element = await screen.findBy('button',{
            name: 'Start'
        }, {
            timeout: 2000
        })
        expect(element).toBeInTheDocument()
    })
```
#### Debugging
* screen.debug() b/w two points
* Testig playground chrome extension
#### User Interations
* user-event testing library
    * comes with ```create-react-app```
* user-event vs fireevent
    * user-event is more superior
    * fire event for one event only unlike user event having multiple interactions with extra checks
#### Pointer interactions
* When user clicks, count should be incremented to 1
```
// component
const Counter = () => {
    const [count, setCount] = useState(0)
    return (
        <>
            <h2>{count}</h2>
            <button onClick={() => setCount(count+1)}>Increment</button>
        </>
    )
}

// test
import {render, screen} from '@testing-library/react'
import {user} from '@testing-library/user-event'
import {Counter} from 'src'

describe('counter', () => {
    test('renders correctly', () => {
        render(<Counter />)
        const element = screen.getByRole('heading')
        expect(element).toBeInTheDocument()
        const incrBtn = screen.getByRole('button', {
            name: 'Increment'
        })
        expect(incrBtn).toBeInTheDocument()
    })

    test('renders a count to be 0', () => {
        render(<Counter />)
        const element = screen.getByRole('heading')
        expect(element).toHaveTextContent('0')
    })

    test('Renders a count 1 after a button is clicked', async () => {
        user.setup()
        render(<Counter />)
        const element = screen.getByRole('button', {
            name: 'Increment'
        })
        await user.click(element)
        expect(element).toHaveTextContent('1')
    })
})
```
#### Keyboard events
* On button click, set the value equals the value in input box
```
const Counter = () => {
    const [count, setCount] = useState(0)
    return (
        <>
            <h2>{count}</h2>
            <input type='number' name='count' value='count' onChange={(e) => setCount(parseInt(e.target.value))}/>
            <button onClick={() => setCount(count)}>Set</button>
        </>
    )
}

// test
test('element equals in input box to be displayed in h2', async () => {
    user.setup()
    render(<Counter />)
    const elementInput = screen.getByRole('spinbutton')
    await user.type(elementInput, '10')
    expect(elementInput).toHaveValue(10)
    const elementBtn = screen.getByRole('button', {
        name: 'Set'
    })
    await user.click(elementBtn)
    const head = screen.getByRole('heading')
    expect(head).toHaveTextContent('10')
})
```
#### Fire Events
* Should be able to type in an input
```
import {fireEvent} from '@testing-library/react'

// second props is let say to have useState hook
test('it should be able to type in value', () => {
    render(<AddInput tasks={[]} setTasks={()=>{}}/>)
    const inputElement = screen.getByPlaceholderText('Add a new task here...')
    fireEvent.change(inputElement, {target: {value:"Add a task"}})
    expect(inputElement.value).toBe("Add a task")
})
```
