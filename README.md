# How to Write Test Cases in React using Jest and Enzyme.

## Getting started

In this File, we will write the test cases for our React component from scratch using Jest and Enzyme. Also, 
we will learn more about Jest and Enzyme.

## What is Testing

Testing is a line-by-line review of how your code will execute. A suite of tests for an application comprises various bit of code to verify whether an application is executing successfully and without error. Testing also comes in handy when updates are made to code. After updating a piece of code, you can run a test to ensure that the update does not break functionality already in the application.

## Why Testing ?
It’s good to understand why we doing something before doing it. So, why test, and what is its purpose?

1--->The first purpose of testing is to prevent regression. Regression is the reappearance of a bug that had previously been fixed. It makes a feature stop functioning as intended after a certain event occurs.
2--->Testing ensures the functionality of complex components and modular applications.
3--->Testing is required for the effective performance of application or product.

## What Is Jest?
Jest is a unit testing framework to write test cases for JavaScript and React codes. Jest was created by Facebook.
The command create-react-app installs Jest and its set of dependencies, so we don’t have to worry about that.

## What Is Enzyme?
Enzyme is a JavaScript utility to test React. With the help of Jest, Enzyme becomes more powerful and takes full control of the React app to test each and every aspect of React components. Enzyme is specifically used for testing React components.

## Installing and Configuring Jest
we will be installing Jest and writing tests, then will be using Create React App, because it is ready for use and ships with Jest.

npx create-react-app my-app

We need to install Enzyme ****and enzyme-adapter-react-16 with react-test-renderer

npm install --save-dev enzyme enzyme-adapter-react-16 react-test-renderer

Now that we have created our project with both Jest and Enzyme, we need to create a setupTest.js file in the src folder of the project. The file should look like this:

import { configure } from "enzyme";
import Adapter from "enzyme-adapter-react-16";
configure({ adapter: new Adapter() });

This imports Enzyme and sets up the adapter to run our tests.

let’s learn some basics, What is 'it or test' , 'describe' , 'expect' , 'mount' , 'shallow' ?
1. it or test --You would pass a function to this method, and the test runner would execute that function as a block of tests.
2. describe --This optional method is for grouping any number of it or test statements.
3. expect --This is the condition that the test needs to pass. It compares the received parameter to the matcher. It also gives you access to a number of matchers that 4. let --you validate different things. You can read more about it in the documentation.
5. mount --This method renders the full DOM, including the child components of the parent component, in which we are running the tests.
6. shallow --This renders only the individual components that we are testing. It does not render child components. This enables us to test components in isolation.

## Creating A Test File
How does Jest know what’s a test file and what isn’t? The first rule is that any files found in any directory with the name __test__ are considered a test. If you put a JavaScript file in one of these folders, Jest will try to run it when you call Jest, for better or for worse. The second rule is that Jest will recognize any file with the suffix .spec.js or .test.js. It will search the names of all folders and all files in your entire repository.

Create our first test, for a React mini-application created for this Documentation.
Run npm install to install all of the packages, and then npm start to launch the app. 

Let’s open App.test.js to write our first test. First, check whether our app component renders correctly and whether we have specified an output:

it("renders without crashing", () => {
  shallow(<App />);
});

it("renders Account header", () => {
  const wrapper = shallow(<App />);
  const welcome = <h1>Display Active Users Account Details</h1>;
  expect(wrapper.contains(welcome)).toEqual(true);
});


In the test above, the first test, with shallow, checks to see whether our app component renders correctly without crashing. Remember that the shallow method renders only a single component, without child components.
The second test checks whether we have specified an h1 tag output of “Display Active User Account” in our app component, with a Jest matcher of toEqual.

Now, run the test:

npm run test 
/* OR */
npm test

The output in your terminal should like this:

  PASS  src/App.test.js
  √ renders without crashing (34ms)
  √ renders Account header (13ms)

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        11.239s, estimated 16s
Ran all test suites related to changed files.

Watch Usage: Press w to show more.

As you can see, our test passed. It shows we have one test suite named App.test.js

## Testing React Components
 Testing involves making sure the output of a component hasn’t unexpectedly changed to something else. Constructing components in the right way is by far the most effective way to ensure successful testing.

One thing we can do is to test components props — specifically, testing whether props from one component are being passed to another. Jest and the Enzyme API allow us to create a mock function to simulate whether props are being passed between components.
We have to pass the user-account props from the main App component to the Account component. We need to give user-account details to Account in order to render the active account of users. This is where mocking comes in handy, enabling us to test our components with fake data.

Let’s create a mock for the user props:

const user = {
  name: "Adeneye David",
  email: "david@gmail.com",
  username: "Dave",
};

We have created a manual mock function in our test file and wrapped it around the components. Let’s say we are testing a large database of users. Accessing the database directly from our test file is not advisable. Instead, we create a mock function, which enables us to use fake data to test our component.

describe("", () => {
  it("accepts user account props", () => {
    const wrapper = mount(<Account user={user} />);
    expect(wrapper.props().user).toEqual(user);
  });
  it("contains users account email", () => {
    const wrapper = mount(<Account user={user} />);
    const value = wrapper.find("p").text();
    expect(value).toEqual("david@gmail.com");
  });
});


We have two tests above, and we use a describe layer, which takes the component being tested. By specifying the props and values that we expect to be passed by the test, we are able to proceed.

In the first test, we check whether the props that we passed to the mounted component equal the mock props that we created above.

For the second test, we pass the user props to the mounted Account component. Then, we check whether we can find the <p> element that corresponds to what we have in the Account component. When we run the test suite, the test runs successfully.

We can also test the state of our component. Let’s check whether the state of the error message is equal to null:

it("renders correctly with no error message", () => {
  const wrapper = mount();
  expect(wrapper.state("error")).toEqual(null);
});


In this test, we check whether the state of our component error is equal to null, using a toEqual() matcher. If there is an error message in our app, the test will fail when run.

## Writing Test Cases Functional Components

Import the necessary dependencies and the component you want to test:

import React from 'react';
import { shallow } from 'enzyme';
import MyComponent from './MyComponent';

Write a test case using Jest's test function:

test('renders correctly', () => {
  const wrapper = shallow(<MyComponent />);
  expect(wrapper).toMatchSnapshot();
});

This example uses shallow rendering to render the component and compares the rendered output to a snapshot. The first time you run this test, it will create a snapshot file. On subsequent test runs, it will compare the output with the saved snapshot.

You can also write assertions to test specific behavior or props:

test('displays the correct message', () => {
  const wrapper = shallow(<MyComponent message="Hello, world!" />);
  expect(wrapper.find('p').text()).toEqual('Hello, world!');
});
This example asserts that the rendered component contains a <p> element with the text "Hello, world!".

## Writing Test Cases Class Components

Import the necessary dependencies and the component you want to test:

import React from 'react';
import { shallow } from 'enzyme';
import MyClassComponent from './MyClassComponent';

Write a test case using Jest's test function:

test('renders correctly', () => {
  const wrapper = shallow(<MyClassComponent />);
  expect(wrapper).toMatchSnapshot();
});

Just like with functional components, this example uses shallow rendering and compares the rendered output to a snapshot.

You can also write assertions to test specific behavior or instance variables:

test('increments counter on button click', () => {
  const wrapper = shallow(<MyClassComponent />);
  const button = wrapper.find('button');
  button.simulate('click');
  expect(wrapper.state('counter')).toEqual(1);
});

This example asserts that clicking the button increments the component's counter state variable.

## Running Tests

To run the tests, use the following command:

npm test






