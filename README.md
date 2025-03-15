# Technical Writing Assignment

For guidance on setting up and submitting this assignment, refer to the Marcy lab School Docs How-To guide for [Working with Short Response and Coding Assignments](https://marcylabschool.gitbook.io/marcy-lab-school-docs/fullstack-curriculum/how-tos/working-with-assignments#how-to-work-on-assignments).

## Prompt 1 // Caleb

In your own words, explain why React is a popular choice for building user interfaces. Make sure to mention at least one benefit, such as how it simplifies development, supports reusable components, or helps optimize performance. Feel free to include any specific features you find particularly helpful.

### Response 1 
React is a popular choice for building user interfaces due to the fact that rendering data is faster, creating elements is less repetitive, as well as the fact that it is faster and easier to read.

## Prompt 2 // Caleb

Explain how the useState hook is used in React to manage state within functional components. In your response, include an example of how useState might be used in a simple application and why managing state is important in building interactive user interfaces.

### Response 2
useState hook is used in React to manage state through a state value in an array and a setter function. When the state changes, useState makes the component re-render to keep up with said changes. Imagine you have a website that shows you a gallery of pictures and allows you to give a thumbs up to the pictures you like. Whenever another person adds a thumbs up, useState makes the component re-render the data so that the number of thumbs up is updated. Managing state is very imporant in building interactive user interfaces due to the fact that you probably don't want the data shown on your application to be outdated, incorrect, or anything of the like.


## Prompt 3 // Xavier

Describe the different ways the useEffect hook can be triggered in a React component. Include an explanation of how the dependency array influences its behavior. If possible, provide a code example for each scenario to illustrate your explanation.

### Response 3
The useEffect hook in React is used to handle side effects in functional components, and it can be triggered in several ways depending on how its dependency array is programmed. When useEffect is used without a dependency array, it runs after every render. When it is used with an empty dependency array ([]), it only runs once after the initial render. If specific values are provided in the dependency array, the effect will run only when those dependencies change, allowing you to control precisely when the side effect is re-triggered. Lastly, if a cleanup function is returned inside the useEffect, it will run before the component unmounts or before the effect runs again. This behavior makes useEffect useful for managing subscriptions, timers, API calls, or any side effects that depend on the component lifecycle or state changes.
#### Running with no dependency array
```jsx
function Example1() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    console.log('Effect ran (every render)');
  });
  return <button onClick={() => setCount(count + 1)}>Click {count}</button>;
}
```
#### Running with an empty dependency array
```jsx
function Example2() {
  useEffect(() => {
    console.log('Effect ran (only once on mount)');
  }, []);
  return <div>Hello!</div>;
}
```
#### Running with a specific depndency changes
```jsx
function Example3() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');
  useEffect(() => {
    console.log('Effect ran (count changed)', count);
  }, [count]);
  return (
    <>
      <button onClick={() => setCount(count + 1)}>Increment {count}</button>
      <input onChange={(e) => setName(e.target.value)} value={name} />
    </>
  );
}
```

## Prompt 4 // Xavier

The component below makes a mistake when using useEffect. When running this code, we will get an error from React! Please fix this code.

```js
const DogDisplay = () => {
  const [imgSrc, setImgSrc] = useState('https://images.dog.ceo/breeds/hound-english/n02089973_612.jpg');

  useEffect(async () => {
    try {
      const response = await fetch('https://dog.ceo/api/breeds/image/random');
      if (!response.ok) throw new Error(`Error: ${response.status}`)
      const data = await response.json();
      setImgSrc(data.message);
    } catch (error) {
      console.error(error);
    }
  }, []);

  return <img src={imgSrc} />
}
```

After fixing the code provide and explanation to what you fixed and why it needed to be fixed.

### Response 4

### Fixed code:
```jsx
const DogDisplay = () => {
  const [imgSrc, setImgSrc] = useState('https://images.dog.ceo/breeds/hound-english/n02089973_612.jpg');
  useEffect(() => {
    const fetchDogImage = async () => {
      try {
        const response = await fetch('https://dog.ceo/api/breeds/image/random');
        if (!response.ok) throw new Error(`Error: ${response.status}`);
        const data = await response.json();
        setImgSrc(data.message);
      } catch (error) {
        console.error(error);
      }
    };
    fetchDogImage();
  }, []);
  return <img src={imgSrc}/>;
};
```
### Reason for the fixes
- The original code was incorrect because useEffect cannot directly accept an async function, but an async function always returns a Promise.
### Fixes
- Instead of making the useEffect callback itself async, I defined an inner async function inside the useEffect, and then called it synchronously. This keeps the useEffect callback synchronous while still allowing me to use await inside the inner function.
