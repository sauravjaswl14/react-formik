Forms are vital part of any business application. We use forms to register, login, submit a request,place an order, schedule an appointment and perform countless other tasks.

While developing forms it's important to create an experience that guides the user efficiently and effectively through the workflow.

As developers we need to:
-Handle form data
-validation
-visual feedback with error messages
-Form submission

formik: Formik is a small library that helps you deal with forms in React and React Native.

WHY WOULD YOU WANT TO USE FORMIK?

Well formik helps you with three main parts

-> Managing the form data i.e getting the values from different form fields in and out of the form state
-> Form Submission :- formik helps you easily handle form submissions
-> Form validation and displaying error messages

Formik helps you deal with forms in a scalable, performant and easy way.

## COURSE STRUCTURE

-Build a simple form
-useFormik Hook
-Manage the form state
-Handle form submission
-Form form submission
-Form Validation
-Formik Components : we'll see various components that formik provides to abstract some repetition and the code
-Few handy features
-Reusable components for input, textarea, select, radio buttons and checkboxes
Build a user registration form

import {useFormik} from 'formik'

//A hook is basically a function, we need to invoke it inside our component

export default function YoutubeForm(){
//useFormik hook accepts an object as a parameter

//this hook will then return an object which contains variety of useful properties and methods that we could use with our form

const formik = useFormik({})

//now this formik object is what is going to help us with the three points:

1. Managing the form state
2. Handling form submission
3. Validation and error messages

}

MANAGING FORM STATE

let's understand how formik helps us with managing our form state, now what do we mean by that

We know that our youtube form has three fields: Name, Email and Channel

At the moment thought we're not tracking the values of these form fields, when we type in something the value of the form field changes and in React if a value changes we need a state variable for that. We need state variables for Name, Email and channel. Collectively we can call that form states

Form State is nothing but an object which maintains the value of different form fields

if we change the name to "Vishwas" it should be reflected in the form state object, if we change the email to "v@example.com", the form state again should be updated, similarly change the channel to "Codeevolution", the same should be reflected in our form state.

if we are able to manage this form state we can eventually submit this data when the user click on the submit button.

import React from 'react'
import {useFormik} from 'formik'

function YoutubeForm(){

const formik = useFormik({})

return (

  <div>

    <form>

      <label htmlFor='name'>Name</label>
      <input type='text' id='name' name='name'/>

      <label htmlFor='email'>Email</label>
      <input type='email' id='email' name='email'/>

      <label htmlFor='channel'>Channel</label>
      <input type='text' id='channel' name='channel'/>

      <button>Submit</button>

    </form>

  </div>

)
}

The first step is to pass in a property called initialValues in the object we pass to the useFormik Hook

the initialValues property is an object

const formik = useFormik({
initialValues: {
name: '',
email: '',
channel: ''
}
})

this object contains the initial values for all our form fields

It is very important to note that the properties for initial values correspond to the name attribute of the individual fields

STEP 2: WE NEED TO ADD THE onChange and the value prop for each of the form fields, this is required to ensure the form fields are tracked in react by formik

this is where the formik constant comes into picture

<input type='text' id='name' name='name' onChange={formik.handleChange} value={formik.values.name}/>

Once you do this the formik will automatically track the form field values for you.

<input type='email' id='email' name='email' onChange={formik.handleChange} value={formik.values.email}/>

console.log('Form values', formik.values)

so we want to take peek into what formik.values actually is which we're using here for our value prop

formik.values is a object where the key corresponds to the name attribute of the form field and then the value of that form field

formik.values is an object which always reflects the state of the form, this is what we are feeding back as the value to the form fields

handleChange method is formik's helper to update the values object

step1: we pass in the initial values to our useFormik hook. the properties must match the name attribute of the form fields. This initialValues object is mandatory

step2: we need to add the onChange and the value attribute or props to all the fields that need to be tracked. the onChange prop is equal to formik helper method called handleChange, this method will basically read the name attribute and update the corresponding property in the values object. the values object will contain values for all the form fields. we then access each of them individually and assign it to value prop on each of the form fields

Flow:-

First, take the values from initialValues object and set that in the values object. Then when you change the form field values the onChange event is triggered which fires the handleChange method which will update the values object.

When the values object updates, it's passed back into the form field. This cycle ensures that the form state is managed correctly

## Handling Form submissions

Recap:

we learned how to manage our form state using formik, We learned that we need to specify the initialValues in the object passed to the useFormik Hook. We learnt that we need to add the onChange prop to all our form fields. and we also learnt that formik.values contains all of the form field values which can then be assigned individually to the form fields. let's learn how to get hold of the form state when the user clicks on the subit button.

-> Handling Form submissions with formik is really simple. there are two steps.

For the first step we need to specify the onSubmit handler on the form tag and this is going to be equal to formik.handleSubmit

<form onSubmit={formik.handleSubmit}>

So again our formik constant is giving us a helper method to handle form submission.

for the second step we need to revisit the object we pass in to the useFormik Hook

As it turns out apart from initial values we can specify another property called onSubmit. this property is a method which automatically receives the form state as its argument.

onSubmit we are going to have a arrow function which receives value as it's argument.

function YoutubeForm(){
const formik = useFormik({
initialValues: {
name: 'Vishwas',
email: '',
channel: ''
},
onSubmit: values => {
console.log('Form data', values)
}
})
}

What we need to notice is that values is pretty much what we were refering to so far using formik.values

And the best thing here is when you click on the submit button, formik is going to automatically execute this onSubmit method.

<button type="submit">Submit</button>

First step, on the form tag specify onSubmit prop set it equal to formik.handleSubmit. What this will do is automatically tie the onSubmit method to the submit event.
the onSubmit method at all times will receive the latest values of the form data as it's argument. this can then be used as per the requirement

## Form Validation

What formik does is let you define a validation function. And that validation function needs to be assigned to a property called validate in the object that we pass to the useFormik hook. so just like onSubmit we specify a third property called validate and this again just like the onSubmit property is a function which automatically recieves the values object as it's argument. and we know that values is an object which contains three properties with three form fields. so we pretty much hanve values.name, values.email and values.channel

this validate function is a function tha must satisfy some conditions for formik to work as intended.

The first condition is that this function must return an object

the second condition is that the keys for this error object should be similar to that of the values object i.e errors.name, errors.email, errors.channel

Again these keys will corespond to the name attribute for the three form fields.

the third condition is that the values of these keys should be a string indicating what the error message should be for that particular field.

for example errors.name = "This field is required"

so if name is a mandatory field this is the error message we want to be displayed.

function YoutubeForm(){
const formik = useFormik({
initialValues: {
name: 'Vishwas',
email: '',
channel: ''
},
onSubmit: values => {
console.log('Form data', values)
}
})

validate: values => {
let errors = {}

    return errors

}
}

validate: values => {
const errors = {}

if (!values.name) {
errors.name = 'Required'
}

if (!values.email) {
errors.email = 'Required'
} else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
errors.email = 'Invalid email format'
}

if (!values.channel) {
errors.channel = 'Required'
}

return errors
}

lets refactor the code:

const initialValues = {
name: 'Vishwas',
email: '',
channel: ''
}

const onSubmit = values => {
console.log('Form data', values)
}

const validate = values => {
const errors = {}

if (!values.name) {
errors.name = 'Required'
}

if (!values.email) {
errors.email = 'Required'
} else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
errors.email = 'Invalid email format'
}

if (!values.channel) {
errors.channel = 'Required'
}

return errors
}

function YoutubeForm(){
const formik = useFormik({
initialValues,
onSubmit,
validate
})
}

## Displaying error messages

previously, we created the validate function that checks the form field values and returns the appropriate error message for each of those fields, in this section let's see how to display these error messages.

We've already learnt that the formik object returned by useFormik() contains alot of useful properties and helper methods to manage our form.

It is this very object that also helps us to get hold of the error messages.

let's revisit one of the properties formik provides.

Formik also provides us with another property called errors and this again just like the values object is an object with the key value pairs for each of the form fields

console.log('Form errors', formik.errors)

you can see that the errors object is now populated with the same keys as the value object with the value itself for each of the keys is the error message that we have specified in our validate function

Form errors {name: "Required", email: "Required", channel: "Required"}

so basically, formik runs the validate function on change of your youtube form and then populate this formik.errors object all we have to do now is access these error messages using the object dot notation in js

<form onSubmit={formik.handleSubmit}>
  <label htmlFor='name'>Name</label>
  <input type='text' id='name' name='name' onChange={formik.handleChange} value={formik.values.name}/>
  {formik.errors.name ? <div>{formik.errors.name}</div> : null}
</form>

## Visited Fields

we learnt how to display a error message for particular field but we did have a setback. while our form works and the user sees error, it's not a great user experience for them.

Our validation function runs on each key stroke against the entire forms values and our errors object contains all validation errors at any given moment and in our youtubeform jsx we are just checking if an error exists and then immediately displaying it to the user.

this is awkward since we are going to show error messages for fields that the user hasn't even visited yet.

If there are 20 fields we should not display error messages for all the 20 fields if the user has just interacted with the first field.

Most of the time we only want to show a field's error message after our user is done typing in that field

Now the question is how do we keep track of the fields the user has interacted with or in simpler terms how do we keep track of the visited fields in a form, well that is where formic again comes to our help

if we have to track whether a form field has been visited we have to add the onBlur prop on the form input element and to this prop we pass in formik.handleBlur

Again handleBlur is a helper method that we get from our formic constant

<form onSubmit={formik.handleSubmit}>
<div className='form-control'>
<label htmlFor='name'>Name</label>
<input 
type='text' 
id='name'
name='name'
onChange={formik.handleChange}
onBlur={formik.handleBlur}
value={formik.values.name}
/>
{formik.errors.name ? (
  <div className='error'>{formik.errors.name}</div>
  ): null}
</div>

So we have done what formik requires us to do, keep track of the visited fields. But where does formik stores this information. It stors it in the object called touched. And that object is of the same shape as values object.
So similar to formik.values and formik.errors we also have formik.touched

console.log('Visited fields', formik.touched)

so the touched object gives us information if the field has been visited or not if it has been visited the property would be present with the value of true

## Improving validation UX

We learnt about the touched object that formik maintains to let us know the visited fields in a form.
let's use that object to improve the validation user experience

if you take a look at formik.touched object it maintains a key for every field, all we have to do now is render the error messages only if the field's error message exists and the user has visited that particular field

<form onSubmit={formik.handleSubmit}>
<div className='form-control'>
<label htmlFor='name'>Name</label>
<input 
type='text' 
id='name'
name='name'
onChange={formik.handleChange}
onBlur={formik.handleBlur}
value={formik.values.name}
/>
/**only if the field has been visited and the error message exists then show the error message**/
{formik.touched.name && formik.errors.name ? (
  <div className='error'>{formik.errors.name}</div>
  ): null}
</div>
</form>

To quickly summarize what we have done is handled the onBlur event of the input fields, we pass in a formik helper method called handleBlur as the event handler. What this does is store the information on whether a particular field has been visited or not and that information is stored in an object called touched. This touched object can then can be used to conditionally render the error message for a particular field.

This is pretty much the fundamentals about form validation with formik in react but as it turns out formik provides an alternate way of specifying the validation rules for any given form

## schema validation with Yup

let's discuss an alternate way of writing validation rules with formik. This alternate way depends on the library called yup.

-> npm i yup

import \* as Yup from 'yup'

Now what we're not going to do is understand how yup works. Instead we're just going to see how to rewrite our validate function with yup. once we understand what yup can do you can then explore on your own what yup is capable of.

For our first step we need to write a validation schema object. This is pretty much what yup is for: object schema validation.

so after the validate function, let's create a new constant called validationSchema, to this we assign a yup object schema.

const validationSchema = Yup.object()

to this we need to pass in an object which contains the rules for each of our form fields.

const validationSchema = Yup.object({
name: Yup.string().required('Required'),
email: Yup.string().email('Invalid email format').required('Required'),
channel: Yup.string().required('Required')
})

the second step is to pass this schema into our useFormik Hook

function YoutubeForm(){
const formik = useFormik({
initialValues,
onSubmit,
validationSchema
//validate
})
}

we're going to see how we can further simplify our code with formik by reducing boilerplate

if we look at our form fields, we can see that three props are sort of similar across the three fields:

onChange={formik.handleChange}
onBlur={formik.handleBlur} and
value which is accessed from formik.values using the name attribute

example:- value={formik.values.name}

So, formik sees this and tells us, "hey you sort of seem to be adding the same lines of code every time you create a new field, in other words there is some boilerplate code for every field in this form, let me help you with that, i am goiing to provide you a helper method called 'getFieldProps' which behind the scenes will add these props for you"

So what we can do is replace the three lines of code with a single call to formik.getFieldProps()

to this method we pass one argument which is the name of the form field. for our first field the string "name" is the name attribute

formik.getFieldProps('name')

Also for this helper method we need to add the spread operator

{...formik.getFieldProps('name')}
{...formik.getFieldProps('email')}
{...formik.getFieldProps('channel')}

<div className='form-control'>
  <label htmlFor='email'>E-mail</label>
  <input
    type='email'
    id='email'
    name='email'
    {...formik.getFieldProps('email')}
  />

{formik.touched.email && formil.errors.email ? (

<div className='error'>{formik.errors.email}</div>
): null}

</div>

And that pretty much is our first refactor

## Formik Components

We learnt about getFieldProps() helper method with which we could get rid of some boilerplate code. Although that is decent we still have to manually pass each input the getFieldProps() helper method

To save us even more time formik actually provides a few components that implicitly use react context to make our life easier and our code less verbose

Out of the several components that the formik library provides , we are going to take a look at four of them to refactor our YoutubeForm component:

Formik
Form
Field
ErrorMessage

The Formik component is a replacement to the useFormik hook. The argument which we pass to useFormik as an object will be passed in as props to the Formik Component

Let's see how to replace useFormik hook with the formic component step-by-step

first step: import Formik instead of useFormik

import {Formik} from 'formik'

step2 we remove the call to use formik

step 3 we wrap our entire form with this Formik component

final step: pass in the different props to this formik component and these are the same which we had specified when calling the useFormik hook

<Formik 
initialValues={initialValues} 
validationSchema={validationShema} 
onSubmit={onSubmit}>

  <form onSubmit={formik.handleSubmit}></form>
</Formik>

and that pretty much is the code refactoring for the formik component. It infact behaves as a context provided component that provides the different properties and helper methods for the other three components we are going to use .

## Form Component

We used the Formik component in our code, although that component doesnot necessarily reduce the number of lines or simplify our code it is needed for the remaining three components that do simplify our code

let's take a look at the Form component

first step: import form

import {Formik, Form} from 'formik'

second step: replace the html form element with the Form component

Third and final step: remove the onSubmit prop

We're able to do this because Form component is a small wrapper around the html form element that automatically hooks into formik's handleSubmit method

So the Form component helps us by automatically linking the onSubmit method to our form's submit event.

## Field Component

This component simplifes the code for a form field. Right now for every field we specify the getFieldProps() helper method passing in the name attribute value. Formik sees this as a common pattern which can be abstracted and provides us with the Field component to simplify the code.

first step: import Field from formik

import {Formik, Form, Field} from 'formik'

second step: replace each input tag with the Field component

third and final step: get rid of getFieldProps() helper method from each of the fields, and we are able to do this because the field component does three things

first, it will behind the scenes hookup inputs to the top-level Formik component

second, it uses the name attribute to match up with the formik state

third, by default field will render an input element which is what out youtubeform had as well

## ErrorMessage Component

At the moment for displaying error messages we check if the field has been visited, check if the error exists and if it does we render that error. This again seems like a pattern across the different form fields and when there is a pattern, formik wants to help us out.
And it is for this particular scenario formik provides the ErrorMessage Component

step 1: import ErrorMessage from formik

import {Formik, Form, Field, ErrorMessage} from 'formik'

step 2: Replace the block of code rendering the error message with the ErrorMessage Component

step 3: pass in a name prop which is equal to the name attribute on the Field Component

<ErrorMessage name='name'>

Now this replacement is possible because the error message component behind the scenes will take care of rendering the error message for particular field indicated by name prop, only if the field has been visited and if the error exists.

Again it is abstraction over what we were achieving manually.

## Let us quickly summarize the steps to refactor the form

first, import Formik and then wrap your entire form with the Formik component. this Formik component accepts initialValues, ValidationSchema and onSubmit handler as props.

next, replace the form html tag with the Form component from formik. This will automatically link the onSubmit event to the onSubmit method passed into Formik

Replace each of the form fields with the Field component. This Field component hooks into formik using the name attribute. It'll takecare of handling the value, handling onChange and the onBlur event

Finally, for your error message use the ErrorMessage component which conditionally renders the error corresponding to a form field only if the field has been visited and if the error exists

## Journey so Far

We started off by creating a simple form with three fields. That form though was not connected to react in any way. So we then installed the formik library and used the useFormik Hook to connect the form to react.

We needed to achieve three things:

1. Managing form state
2. Handling form submission
3. form validation

To handle form state we used the initialValues object and the handleChange helper method provided by formik.

To handle form submission, we used the onSubmit method and the handleSubmit helper method provided by formik

Validation wise we had a few more concepts to understand

we learnt about the validate function which returns an object containg the error messages for all the form fields

We also had a look at an alternate way of specifying the validation rules which was the validationSchema object, this was possible with the yup library

once we had the rules we could display them using the errors and touched objects accessing each individual field

Once we had a fully functional form we then refactored the code to use the components provided by the formik library.

We used Formik which works as the context provider, Form which takes care of the regular form tag, Field which takes care of the input fields and finally the ErrorMesssage component which takes care of validation.

## Field Revisited

By default Field component renders an HTML input element

Behind the scenes, Field Component hooks up input elemrnt to formik, i.e it hooks up handleChange, handleBlur and value of the form field

let's discuss few additional points about this Field component

Field component will pass through any additional props that you specify.

At the moment we are passing in the id attribute to all the field components, if you go to the browser and inspect we can see that the same has been passed through to the input element as well.

Similarly if we were to specify a placeholder prop on the channel field, the same would get passed through to the input element and you will be able to see the placeholder as well.

-> Any additional props in the Field components will be passed through

-> The ability to render a different element other than the input element.

let's say we want to add a textarea field to our Youtube form

first add it to the initial value object

const initialValues = {
name: 'Vishwas',
email: '',
channel: '',
comments: ''
}

To instruct formik to render Field component as textarea we simply need to add the as prop passing in 'textarea'. this is now going to render Field component as textarea element

<div className="form-control">
<label htmlFor="comment">Comments</label>
<Field as='textarea' id='comments' name='comments'/>
</div>

Now this as prop can accept as it's value either input or textarea or select or a custom react component as well. It's default value is input which is why we don't have to specify the prop when rendering an input element.

We'll learn more about rendering select drop-down and custom react components later on.

Field component accepts an as prop to decide what element to render

The third and final point of discussion is around the render props pattern now this is a slightly more advanced pattern which will give you more fine-grained control over the rendering and behavior of your form field.

We're not going to discuss in detail how to use each of the props that the render props object will provide, but we'll see how to use this render props pattern to create a field similar to what we already have.

let's say we need to render another input element to collect the user's address, we could do this the way we are rendering name or email or channel but we want to learn the render props way.

let's start with the initial values, we're going to add a property called address

const initialValues = {
name: 'Vishwas',
email:'',
channel:'',
comments: '',
address: ''
}

let's add the JSX

<div className='form-control'>
  <label htmlFor='address'>Address</label>
  <Field name = 'address'/>
</div>

Now this address field would work perfectly fine as it is but we definitely want the render props way

With the render props pattern we us a function as children to the component. So now our Field component will now have opening and closing tags
<Field></Field>

NExt as children we pass in a function, and this will be an arrow function. The arrow function will return JSX which in our case is an input element for the user to enter their address

<Field>
  {
    () => {
      return <input id='address' />
    }
  }
</Field>

but as it stands this input element is not hooked into formik in any way. This is where the props for this arrow function comes into picture.

<Field>
  {
    (props) => {
      console.log('Render props',props)
      return <input id='address' />
    }
  }
</Field>

We can see that we have three properties: field, form and meta

Render props { field:{...}, form: {...}, meta: {...}}

if we expand further, the field contains name, value, onChange, onBlur everything formik requires to manage the state.

we then have the form object, this is basically the formik constant that the useFormik hook used to return, it has all the properties and helper methods to modify your form in any way required.

in fact we have seen the few things before: errors object, values object, touched object and so on. You can alsoo see some few helper methods: handleChange, handleSubmit, handleBlur and so on.

The third prop provided is the meta prop, this gives you information on whether the particular field has been visited or not, if there is an error or not and ofcourse the value. This is what can be used to render your error messages.

Out of the rhree props, field and meta are more concerned at an individual field level. let's see how to make use of them with our address field.

let's begin by destructuring the render props

<Field>
  {
    (props) => {
      const {field, form, meta} = props
      console.log('Render props',props)
      return <input id='address' />
    }
  }
</Field>

To hook the input with formik we need to spread the field props

<Field>
  {
    (props) => {
      const {field, form, meta} = props
      console.log('Render props',props)
      return <input id='address' {...field} />
    }
  }
</Field>

This will take care of the name, value, handleChange, handleBlur

Next, we can use meta prop to render an error if there was one. Right now we dont have the validation rule, but let's see how to render error anyway.

<Field>
  {
    (props) => {
      const {field, form, meta} = props
      console.log('Render props',props)
      return (
        <div>
          <input type='text' id='address' {...field} />
          {meta.touched && meta.error ? <div>{meta.error}</div>:null}
        </div>
      )
    }
  }
</Field>

this is pretty much what the ErrorMessage component also does.

When you want to use custom components in your form and you want them to be hooked into formik this pattern will definitely come in handy

## ErrorMessage Revisited

if you can recollect the ErrorMessage component accepts a name prop and renders the error message for that particular field if the field has been visited and an error message exist for that field.

if you inspect the error message, you can see that it is just a text node, it's not trapped inside any HTML element let's see how to change that.

To inform the ErrorMessage component to wrap the error message with an HTML element, we need to use the component prop. let's set component prop equal to div.

<ErrorMessage name='name' component='div'/>

if you take a look at the browser, you can see that the error message is now wrapped within a div tag. so you can set it to any HTML element you want to.

Not only that you can even set it to a custom react component.

At the moment the error message is not in the red color, let's create a component that renders it's text in red color and then pass that component as the component prop of the ErrorMessage component.

create a new component file in components folder of your project directory

/components/TextError.js

import React from 'react

function TextError(props){
return (

<div className='error'>
{props.children}
</div>
)
}

<div>
  <label htmlFor='name'>Name</label>
  <Field type='text' id='name' name='name'/>
  <ErrorMessage name='name' component={TextError}>
</div>

An Alternative to this is to use render props pattern, so let's do that to the email field.

<div className='form-control'>
  <label htmlFor='email'>Email</label>
  <Field type='email' id='email' name='email'>
  <ErrorMessage name='email'>
  {
    (errorMsg) => {
        return <div className='error'>{errorMsg}</div>
    }

}
</ErrorMessage>

</div>

This function gets the error message as the prop

## Nested Objects

let's talk about nested objects in formik.

If you take a look at the youtube form currently, we can understand from the initialValues object that there are five fields. And we track our form values in the same structure as these key value pairs.

But sometimes you might want to group together some data into its own seperate object. The reason could be that API accepts the data in such a matter or the database stores the data in particular fashion. Whatever might be the reason we want to group together some of the fields in our form. For that we can make use of the nested objects.

let's understand with an example.

Suppose our youtube form should also collect information about the social presence that the user has. So the form will ask the information about fb and twitter profiles. Since they are related we want them to be grouped and stored as a nested object. let's see how to achieve that in two simple stpes.

First step, on the initialValues object, we're going to specify a property called 'social' and this is going to be equal to an object.

const initialValues = {
name: 'Vishwas',
channel: '',
comments: '',
address: '',
social: {
facebook: '',
twitter: ''
}
}

Second step: let's add the form fields in our JSX

<div className='form-control'>
  <label htmlFor='facebook'>Facebook profile</label>
  <Field type='text' id='facebook' name='social.facebook'/>
  //this time name is equal to social.facebook, this is important because we're dealing with nested objects.
</div>

<div className='form-control'>
  <label htmlFor='twitter'>Twitter profile</label>
  <Field type='text' id='twitter' name='social.twitter'/> 
</div>

## Arrays

let's take a look at managing the field state as an array.

let's say we need to collect the user's phone number, we want to collect their primary and secondary phone number. But while storing the data we dont really need any clearer distinction, we just want them to be stored as an array of a phone numbers under the same label, let's see how to do that.

first step: we need to add a property to our initialValues object.

const initialValues = {
name: 'Vishwas',
email: '',
channel: '',
comments: '',
address: '',
social: {
facebook: '',
twitter: ''
}
phoneNumbers: ['', '']
}

second step: add the JSX

<div className='form-control'>
  <label htmlFor='primaryPh'>Primary phone number</label>
  <Field type='text' id='primaryPh' name='phoneNumbers[0]'/>
</div>

<div className='form-control'>
  <label htmlFor='secondaryPh'>Secondary phone number</label>
  <Field type='text' id='secondaryPh' name='phoneNumbers[1]'/>
</div>

## FieldArray component

We're going to take a look at another component that the formik library provides, which is the FieldArray component.

It is a component that helps with a common scenario, namely dynamic form controls.

You can say that FieldArray component is meant to help you with common array or list manipulations. Let's understand how this component works with an example.

We learnt how to manage the state of a form field as an array.
In our initialValues object, we added a property called phone Numbers and assigned an array with two empty strings. In JSX we had the field component for each of those indexes, phoneNumbers[0] and phoneNumbers[1]

This is fine to collect two phone numbers from the users. Typically though collecting multiple phone numbers or multiple addresses is handled through dynamic form controls. What we mean by that is to begin with we only render one field for the user to enter their phone number, we then give them the option to add or provide more numbers if they wish to. the list of phone numbers will be managed as an array in our form state.

let's see how to implement the dynamic form control to collect the user's phone numbers using the FieldArray component.

first step: import FieldArray

import {Formik, Form, Field, ErrorMessage, FieldArray} from 'formik'

second step: we need to add a property to our initialValues object.

const initialValues = {
name: 'Vishwas',
email: '',
channel: '',
comments: '',
address: '',
social: {
facebook: '',
twitter: ''
},
phoneNumbers: ['', ''],
phNumbers: ['']
}

the value is going to be an array with one empty string, remember we start off by asking just one phone number, so only one value to begin with.

step3: we need to add the JSX.

step 3.1: Add the div tag and the label

<div className='form-control'>
  <label>List of phone numbers</label>
</div>

step 3.2: if you take a look at the previous form fields, after the label we have the Field components.

This time though since we're working with the list of form fields that are dynamic we'll use the FieldArray component. To this component we have to specify the name prop just like the Field component

<div className='form-control'>
  <label>List of phone numbers</label>
  <FieldArray name='phNumbers'>
  
  </FieldArray>
</div>

step 3.3 : to be in control of this dynamic form control, we need to use the render props pattern for this FieldArray component. Now this is similar to the render props pattern that the field components also provides which we have a look at for the address form field in one of the earlier sections.

If the term Render props is confusing to you just think of it as function as children, so in between the opening and closing tags of the field component, we have a function.

This function will automatically get some props and then using those props we will return some JSX. This is what we will do for our FieldArray Component as well.

function as children, so in between the opening and closing tags of the FieldArray component.

<div className='form-control'>
  <label>List of phone numbers</label>
  <FieldArray name='phNumbers'>
    {
      () => {
          //This function will automatically get some props that we can use in our JSX, let's call those props as fieldArrayProps
      }
    }
  </FieldArray>
</div>

<div className='form-control'>
  <label>List of phone numbers</label>
  <FieldArray name='phNumbers'>
    {
      (fieldArrayProps) => {
         console.log("FieldArrayprops",fieldArrayProps)
         return <div>Field Array</div>
      }
    }
  </FieldArray>
</div>

In the console if we expand the object, we can see that we have a few properties and a lot of functions or methods. There are methods like push , pop, remove, unshift which seem like common array manipulation operations. We also have the form object which is basically everything we need to manage entire form.

For our example we are intrested in just two functions and one property.

The two functions are push and remove

push we will use to add a new phone number

Remove we will use to remove an existing phone number, pretty straightforward.

form property on the other hand we need to go a few levels deep. If we expand the form object as well, we can see the values property and here is where we would find the values for our phNumbers fields. We need this particular property to render our JSX

<div className='form-control'>
  <label>List of phone numbers</label>
  <FieldArray name='phNumbers'>
    {
      (fieldArrayProps) => {
         console.log("FieldArrayprops",fieldArrayProps)
         const {push, remove, form} = fieldArrayProps
         const {values} = form
         const {phNumbers} = values
         return (
          <div>
            {phNumbers.map((phNumber, index) => {
              return (
                <div key={index}>
                  <Field name={`phNumbers[${index}]`}>
                  {index > 0 && (
                    <button type='button' onClick={() => remove(index)}>-</button>
                  )
                  }
                  <button type='button' onClick={() => push('')}>+</button>
                </div>
              )
            })}
          </div>
         )
      }
    }

  </FieldArray>
</div>

Now we have the field component, but what we also need are buttons for the user to add or remove fields dynamically

the click handlers for these buttons we are going to make use of remove and push functions

## FastField component

we're going to take a look at the last component that the formik library provides which is the FastField component and this component is mainly meant for performance optimization. According to the documentation it is probably worth considering this if your form has more than 30 fields or if there are fields with very complex validation requirements.

FastField is an optimezed version of the Field component which internally implements the the ship component lifecycle method to block all additional rerenders unless there are direct updates to the FastField form control itself.

so if you feel that a particular field is independent of all other fields in your form then you can use the FastField component

<div className='form-control'>
  <label htmlFor='address'>Address</label>
  <FastField name='address'>
    {props => {
      const {field, form, meta} = props
      return (
        <div>
          <input type='text' id='address' {...field} />
          {meta.touched && meta.error ? <div>{meta.error}</div> : null}
        </div> 
      )
    }}
  </FastField>
</div>

## when does validation run

what we know so far is that once the validation rules are run formik auto populates the formik.errors object with the error messages. For this section we can use that to monitor when exactly validation runs.

we need to pick a field that implements the render porps pattern which will give us access to formik.errors object.

We have two choices: we could use the address field or the phone numbers field. Since the address field is a FastField component, it's behaviour is not exactly what we want, so let's go with the phone numbers.

In the phone numbers FieldArray component, we can see that we already get access to the form object all we have to do now is add log statement

console.log('Form errors, form.errors)

let's inspect the page, on the page load we can see that errors object is empty, so the form validation is not yet run

now let's understand the different scenarios in which this errors object gets populated.

The first scenario is when a change event has occured. So in the channel field if we start typing in something, you can see that the errors object is now populated.

so first scenario, formik runs validation after any change event in the form.

let's reload the form and take a look at the second scenario. On page load again the error object is empty, now if we click inside the channel field and then click outside you can see that again the errors object is populated.

second scenario, formik runs validation after any blur event in the form,
i.e when you blur out from a form field.

let's reload the page and take a look at the third scenario. on page load again errors object is empty, now without interacting with any of the form fields we're going to click on the submit button, once again we can see that the errors object is populated.

so whenever form submission is attempted, formik runs the validation

And what is great here , if the validation doesnot pass for all the fields, the onSubmit handler never gets executed. so we dont have to worry about manually handling form submission only if all the validation rules pass, formik will takecare of that for you.

so, on change, on blur and attempt to submit the form. These are the three cases in which validation will be run.

Now based on the complexity of your form or even just to meet your application requirements, sometimes we might not want formik to automatically run the validation function for us. so what formik does is provide two props to control the first two scenarios.

So on the Formik component, we can specify a prop called validateOnChange and set it to false. this will instruct formik to not run the validation function on a change event.

Now similar to validateOnChange, on the formik component we can pass in another prop called validateOnBlur and set it to false. validation will never be run even on the blur event.

<Formik
initialValues={initialValues}
validationSchema={validationSchema}
onSubmit={onSubmit}
validateOnChange={false}
validateOnBlur={false}

>

## Field level validation

If you can recollect from the old youtube form we had, there are two ways to specify the validation rules for our entire form

1. We pass in a custom validation function using the validate prop
2. we specify the yup object schema using the validation schema prop

What is important to note though is that both these props are available on the top level Formik component

Formik also allows us to specify a validation function at a field level

let's take a look at an example

right now we have a validation rules on the first three fields: name, email and channel

similarly let's try to add a required field validation against the fourth field which is comments

<div className='form-control'>
  <label htmlFor='comments'>Comments</label>
  <Field as='textarea' id='comments' name='comments'/>
</div>

Now if we were to specify this rule using validation schema, we already know how to do that.

As it turns out specifying validation rule at a field level is very similar to the custom validate function we had a look earlier in the old youtube form. Field level validation is almost the same but even simpler

first, we need to define a validate function that works only for the comments field. This method will automatically receive the value of the comments field.

within the function body we declare a variable called error. We then check if the value is empty and assign the message required and finally we return that error.

const validateComments = value => {
let error
if(!value){
error = 'Required'
}
return error
}

Now on the Field component for comments, we can pass in the validate prop and assign this validate function.

<div className='form-control'>
  <label htmlFor='comments'>Comments</label>
  <Field
   as='textarea'
   id='comments'
   name='comments' 
   validate={validateComments}
  />
  <ErrorMessage name='comments' component={TextError}>

//this component basically adds red color to the error message

</div>

## Manually triggering validation

we are going to learn how to manually trigger both form and field level validations with formik

To be able to trigger validations manually formik provides us with two helper methods.

In order to access these methods though we need to use the render props pattern on the entire form itself.

If you take a look at the address form control you can see that we have a function as children. This function automatically receives some props and then returns some jsx.

similarly if we take a look at the field array component for phone numbers we again have the same pattern. function as children, function receives props and returns jsx.

Now though we are going to have function as children on the top level formik component.

The function will receive some props which we are going to call as formik and then return the entire Form component.

<Formik
 initialValues={initialValues}
 validationSchema={validationSchema}
 onSubmit={onSubmit}>

{formik => {
return (

  <Form>...</Form>
  )
  }}
</Formik>

Now that was pretty straightforward but what that did is give us access to this formik props which again lets us control everything that has to do with our form.

lets log this to the console and see what exactly this Formik props is providing.

<Formik
 initialValues={initialValues}
 validationSchema={validationSchema}
 onSubmit={onSubmit}>

{formik => {
console.log('Formik props', formik)
return (

    <Form>...</Form>
    )

}}
</Formik>

Formik props
{values: {...}, errors: {...}, touched: {...}, status: undefined, isSubmitting:false...}

if we expand the object we can see the different properties and methods. we have errors, touched, handleChange, handleBlur, handleSubmit and so on...

Now this object seems very familiar because we have looked at this very object quite a few times already, the first time was when we used the useFormik hook. The formik object that was returned is nothing but this prop we have right now and even more recently in the render props pattern for Field and FieldArray, you can see that we get a prop called form, this again is the same as formik props that we are seeing right now.

It's a good question to ask why do we have the same props at the form level and the field level. Use the field level object when you have to deal with that inidividual field.
But if there is something that has to be done for the entire form use the form level formik props.

For this section we are going to use this top level formik props to show you that you can control both form and field level functionality.

Now before the submit button we're going to add two more buttons

<button type='button'>Validate comments</button>
<button type='button'>Validate all</button>
<button type='submit'>Submit</button>

The first button is to validate the comments field since we have returned a field level validate function for the comments field.
The second button is to validate the entire form

Now let's head back to the console and see what does formik provide to implement these button click handlers

If we expand the formik props object we can see that we have two methods:- validateField, validateForm

<button type='button' onClick={() => formik.validateField('comments')}>Validate comments</button>
<button type='button' onClick={ () => formik.validateForm()}>Validate all</button>

On page load we can see that we don't have any errors being displayed, now let's click on the validate comments button, we can see that we still don't have any error messages being displayed seems strange let's see if the other button works. When we click on validate all button and still we don't see any error messages. let's reload the page and try to understand what is happening. if we click on validate comments we have the formik object logged in the console again. If we take a look at the errors object we have the comments property but if we take a look at the touched object it is empty and if you remember our condition to show an error message is that the field has to be touched in addition to containing an error message. This is the reason we dont see any error message displayed in the UI. The same case holds for validate all as well. We click on the button, we get a new log statement, inspect errors and we have all the errors, inspect touched though and it is empty, which is again the reason why none of the error messages are being displayed.

We can fix this though, if you take a look at the formik object again we have two more helper methods:- setFieldTouched and setTouched.

As the name indicates, setFieldTouched will add that particular field to the touched object and setTouched will add multiple fields to the touched object.

<button type='button' onClick={() => formik.setFieldTouched('comments')}>visit comments</button>
<button type='button' onClick={ () => formik.setTouched({
name: true,
email: true,
channel: true,
comments: true
})}>visit fields</button>

ofcourse if you set it to false it will appear that way in the touched object.

## Disabling submit

The scenarios under which you want the submit button to be disabled is completely dependent on your own requirements and how you want the user experience to be. We will go through two scenarios but you are free to disable the submit button based on your app requirements.

The two scenarios are:

1. Disabling the submit button based on the Validity of the form state
2. Disabling the submit button when Form submission is in progress

Now the first scenario we want the submit button to be disabled if the form state is invalid. To implement this functionality we need to understand few properties in the formik props object, remember we implemented the render props pattern for the formik component which gives us access to this formik props. if you take a look at this in the browser console, you can see that it is an object helps us manage our entire form.

In this list of properties our first focal point is the isValid property, let's understand how it works.

Now isValid is a read-only property that is set to true if the errors object is empty. so on page load you can see that the errors object is empty so isValid is set to true.
now we can simply click inside the channel field and click outside, this will populate the errors object and since the errors object is not empty anymore, isValid is set to false.

So isValid is a property that lets us know if the form has no errors at any given time. WE can use this property to disable the submit button.

<button type='submit' disabled={!formik.isValid}>Submit</button>

so if the form is not valid disable the submit button.

On page load you can see that the submit button is not disabled now this is a bit strange since we know that email, channel and comments are all required fields but they are currently empty. However the submit button still being enabled is not a bug, it is simply following our instructions. We have told the button to be disabled only if isValid is false which in turn is telling disable the submit button if the errors object is not empty, on page load the errors object is in fact empty. So obviously, isValid is true, a form state is valid and the submit button is enabled.

If you click on the submit button it now populates the errors object which in turn sets isValid to false and now our submit button is disabled.

If we resolve all the errors, the submit button is enabled again and we can submit the form.

However there would be some client who wants the submit button to be disabled until all validations have been met right from the get-go. Now there are two ways to solve this.

1. to add validateOnMount prop on the Formik component and set it to true

<Formik
initialValues={initialValues}
validationSchema={validationSchema}
onSubmit={onSubmit}
validateOnMount

>

On page load, as soon as the form mounts on the DOM formik will run the validations against each field and populate the errors object, if the errors object is not empty isValid is false. If isValid is false, the form state is invalid and hence the submit button is disabled.

Although this option seems to work fine there is a drawback. If you have a form with 20 or 30 fields with complex validations, it really doesn't make sense to run all the validation rules even before the user has typed in a single letter, so this first option of using validateOnMount is perhaps suitable for a form with very few fields with simple validations.

2. To use another property present in the formik props object. if we go back to the console, inspect the formik props object, you can see that there is a property called dirty and this basically is a boolean value which indicates if at least one of the form fields value has changed since it was initialized.

On page load, dirty is set to false so none of the values have changed. Now we click on channel and then blur out. if you take a look at the updated formik props, the errors object is populated but dirty is still set to false. If you however change any of the field values to something different from what the initial value is, it gets set to true. so if we reload the page again and remove a letter from 'vishwas' you can see that dirty is set to true.

Let's see how we can use this property in conjunction with the isValid property to disable the submit button

<button type='submit' disabled={!(formik.dirty && formik.isValid)}>Submit</button>

We are telling formik to disable the submit button if the user has changed any field value and the form is not in a valid state

Although this seems to be a much better way there is again a drawback.

So what's to be noted here is that this option of using dirty to disable the submit button is based on the assumption that on page load that is without the user changing any of the form field values, the form state is always invalid. If you know for a fact the user will interact with your form and enter values which will never be exactly the same values as the initialValues object, then you can stick to this option. A lot of the times a user would always interact with the form without trying to click on submit, so this definitely is a good option.

## Disabling the submit button when Form submission is in progress

First scenario was disabling the submit button based on the form state being valid or invalid.

The second scenario is disabling the submit button when the form is submitting in the background. For example let's say you hava a user registration form, when you fill in the details and click on submit an API call is made in the background to register the user. During this time it is really important to disable the submit button, if not the user might end up clicking the submit button twice or more number of times and this could be even more troublesome if the form is a checkout form in an e-commerce site.

So the second scenario is to disable the submit button till the background operation completes.

To implement this we again have to go back to the formik props. if you go through the properties we can find a property called isSubmitting. This is a boolean property which formik will set to true if a form submission has been attempted. so all we have to do is check if isSubmitting is true and if it is disable the submit button.

<button type='submit' disabled={formik.isSubmitting}>Submit</button>

on page load the submit button is enabled which is fine. When we click on submit there are couple of things that happen for which we need to take a look at the formik prop.
Click on submit and you can see that we have a few logs. We're just going to inspect the first and last one. If you inspect the the first one, you can see that isSubmitting is set to true and if we inspect the last one, isSubmitting is set to false.

So when you click on submit, formik will automatically set isSubmitting to true that would disable the submit button. Once the validation completes if at all there were errors, formik will automatically set isSubmitting to false and that is the reason the submit button is enabled again, it happens so quickly that we can't notice it but you can take it for granted it's working as expected.

Now hang on, i mentioned that formik will set isSubmitting to false only if there are errors.But what happens if there are no errors, let's quickly try that out. So we're going to fill in the values for the first four fields and then click on submit, the data gets submitted but out submit button is still disabled. And this is an intended behavior because formik does not know when your API is going to respond back. so we have to manually set isSubmitting to false again. And the way to do that is in the onSubmit method.

It turns out the onSubmit receives a second prop, let's call it onSubmitProp and then let's log it to the console.

const onSubmit = (values, onSubmitProps) => {
console.log('Form data', values)
console.log('submit props', onSubmitProps)
}

if you save the file and go back to the browser, fill in the values and then click on submit button, inspect the submit props, we see a few methods that formik provides. Of these what we want is the setSubmitting method

const onSubmit = (values, onSubmitProps) => {
console.log('Form data', values)
console.log('submit props', onSubmitProps)
onSubmitProps.setSubmitting(false)
}

This will update isSubmitting to false which will in turn enables the submit button.

let's give this a go, save the file head to the browser, fill in the values and click on submit. The data is submitted and the button is enabled again. Ofcourse in a real scenario you would wait for the API response and then call the setSubmitting function.

<button type='submit' disabled={!formik.isValid || formik.isSubmitting}>Submit</button>

So to summarize the disabling of submit button, we make use of two properties from the formik props.

isValid :- which indicates the state of our form being valid or invalid
isSubmitting:- which indicates whether the form is currently being submitted or not

## Load Saved Data

we're going to learn how to load saved data back into our form. If you're working on small forms like user registration or login, you probably don't have a need to load save data. However when you deal with forms that are broken into sections with several fields, you often want to allow the user to save their progress, come back at a later time and continuing filling their form. In this scenario apart from rendering the form we also need to fetch the saved data and fill those values into our form, let's understand how to do that.

One point to mention though is that we are not really going to implement saving the form data or loading the form data from an API, instead we're going to mock the loading of data as if it were to come from a database through an API.

First step let's define the saved data object. This has to be of the same structure as the initial values object.

const savedValues = {
name: 'Vishwas',
email: 'v@example.com',
channel: 'codevolution',
comments: 'Welcome to formik',
address: '221b Baker Street',
social: {
facebook: '',
twitter: ''
},
phoneNumbers: ['',''],
phNumbers: ['']
}

let's just assume this is the users progress with our YouTube form.

for our second step, we're going to add a button to load this saved data

<button type='button'>Load saved data</button>

What we want to do is on click of this button, change formik from reading initialValues to reading savedValues.

To do this let's create a state variable and update that

step 3: import useState from react and declare a new state variable with an initial value of null

function YoutubeForm (){
const [formValues, setFormValues] = useState(null)
}

now on click of this load saved data button, we are going to set the form values state variable to the savedValues

<button type='button' onClick={() => setFormValues(savedValues)}>Load saved data</button>

step 4: we are going to change the value for the initialValues prop on the formik component

function YoutubeForm (){
const [formValues, setFormValues] = useState(null)

return (
<Formik
initialValues={formValues || initialValues}
validationSchema={validationSchema}
onSubmit={onSubmit}

>

...

  </Formik>
)
}

And the final step is to add enableReinitialize prop on Formik component. This prop is really important because it decides whether your form can change initialValues after the form has been initialized once.

function YoutubeForm (){
const [formValues, setFormValues] = useState(null)

return (
<Formik
initialValues={formValues || initialValues}
validationSchema={validationSchema}
onSubmit={onSubmit}
enableReinitialize

>

...

  </Formik>
)
}

let's check in the browser. On page load you can see that we have just vishwas as the initial values for the name field, everything else is empty. This is the reflection of the initialValues object.
Now we're going to click on the Load saved data button, if you now take a look at the fields, we have the savedValues object showing up in the form.All the five fields have different set of values.

let's go with the steps one more time, first we created the savedValues object which is mocking data that would be received from an API. Then we add a button at the bottom to mimic the fetch action of an API. We created a state variable initialized to null and on click of this button we populate that state variable with the savedValues object. We then inform formik to use the saved values if it is present and if it is not present use the initialValues. Finally we tell formik to enable reinitialization using a prop.
This is how you load saved data into a formik form.

Now the scenario we have here is that of when a form has already been loaded and then the user manually loads in saved data on a button click.

A possible use case is if there is a previously filled form and you want to allow the user to load that data into a new application form that would make the user experience much better.

A second scenario which is much simpler scenario would be to fetch a select drop-down values from an API, in that case you definetely could show a loading indicator till your api call is done and once the API call is done update the state variable with the response and only after that is done render the formik component.

## Reset Form Data

We're going to learn how to reset the form data. There are two scenarios of handling form reset.

First scenario is resetting the form data with a reset button and this is pretty straight forward.

<button type='reset'>Reset</button>

So resetting the form data is basically setting the form values to the initalValues object. Based on your requirement you can choose to add the reset button if you want to allow the user to reset their form data.

The second scenario is resetting the form data after form submission has completed. Often if you are able to successfully submit your form, you see the success message and the form would have reset the values to facilitate another submission. To handle this scenario, we use the submit props in the onSubmit method.

So after setting isSubmitting property to false we can clear out the form data using the method resetForm()

const onSubmit = (values, onSubmitProps) => {
console.log('Form data', values)
console.log('submit props', onSubmitProps)
onSubmitProps.setSubmitting(false)
onSubmitProps.resetForm()
}

## Reusable Formik controls

We had a look at the fundamentals of formik as well as a few properties and methods that could come in handy based on the form requirement but what we still need is a clear picture of how we can put it all together and write code that can be deployed to production. So now what we are going to do for the remaining second half of the series is build a set of reusable formik controls that can then be applied across a variety of forms such as registration, login and so on. We're going to break this down and take one step at a time.

We're going to start off by creating a FormikContainer component that we will use to test the other components we will be creating in the subsequent sections. This component is basically a simple formik wrapper which we will see in a bit.

In the next section we are going to talk about a common formik control component that is capable of rendering the different types of form elements. once we have that out of the way we will dive into the core form elements which are input, textarea, select drop down, radio buttons, checkboxes and date picker. Once we have all these in place we will see how to put together a user registration form, a user login form and a course enrollment form. This will give you a really good idea of all the six input types along with a few validations and ofcourse form submission. Lastly we will see how to use a UI component library with our reusable formik controls.

FormikContainer component
FormikControl component
Input
TextArea
Select
RadioButtons
Checkboxes
DatePicker
Registration, Login and Course Enrollment
UI Component library

/components/FormikContainer

import {Formik, Form} from 'formik'
import \* as Yup from 'yup'

function FormikContainer(){
const initialValues = {}
const validationSchema = Yup.object({})
const onSubmit = values => console.log('Form data', values)
return (
//for the jsx we are going to return the Formik component with the render props pattern.

<Formik
initialValues={initialValues}
validationSchema={validationSchema}
onSubmit={onSubmit}

> {

      formik => (
      <Form>
        <submit type='submit'>Submit</submit>
      </Form>)

}
</Formik>
)
}

## FormikControl component

In the last section we created the formik container component which we will soon use to test the different form fields.

Now the functionality of this component is really simple, this component is going to decide which of the different form fields have to be rendered based on one particular prop, let's call that prop as control.

/components/FormikControl.js

function FormikControl(props){
const {control} = props

switch(control){

}

}

export default FormikControl

Now the way to decide which component to render is a simple switch case. we are going to look at six different form controls

function FormikControl(props){
const {control} = props

switch(control){
case 'input':
case "textarea':
case "select":
case "radio":
case "checkbox":
case "date":
default: return null

}

}

Now all we have left is to create the actual components that would be returned for each of these cases

## Input component

We are going to build our first formik control which is the input component.

since this is our first component , let's sort of break it down into individual elements which will help us better understand the props that we need to pass into the component as well.

To implement this input component let's take a look at the props we will need

First and foremost we set the control prop to input which is required to determine the type of formik control we need to render.

control='input'

second, we need a label prop which will be the label text for the form field.

Third, we pass in the all-important name prop which is required by formik for the field as well as their message components

finally we pass in the type attribute which is email for our example. it could also be text or password based on the form field

## we are going to approach every formik control in three steps

first step: we write the code in new component specific to the field type, in our case an input component

second step: we write the code in FormikControl component

third step: we write the code on the FormikContainer component which will also help us test the code we write in the browser.

We need to create a new component for the input formikControl. so within the component folder create a new file called input.js

for this input we would need a Field and ErrorMessage component form formik

import React form 'react'

function TextError(props){
return (

<div className='error'>
{props.children}
</div>
)
}

export default TextError

/components/Input.js

import React from 'react'
import {Field, ErrorMessage} form 'formik'
import TextError from './TextError'

function Input(props){
const {label, name, ...rest} = props
return (

<div className='form-control'>
<label htmlFor={name}>{label}</label>
<Field id={name} name={name} {...rest} />
<ErrorMessage name={name} component={TextError} />
</div>
)
}

export default Input

## second step is to add code in our FormikControl component

import Input from './Input'

function FormikControl(props){
const {control, ...rest} = props

switch(control){
case 'input': <Input {...rest} />
case "textarea':
case "select":
case "radio":
case "checkbox":
case "date":
default: return null

}

## for the third and final step we add code in the FormikContainer component so that we can test the code we have written in the first two steps

}

/components/FormikContainer

first we add a property in the initialValues object

import {Formik, Form} from 'formik'
import \* as Yup from 'yup'
import FormikControl from './FormikControl'

function FormikContainer(){
const initialValues = {
email : ''
}
const validationSchema = Yup.object({
email: Yup.string().required('Required')
})
const onSubmit = values => console.log('Form data', values)
return (
//for the jsx we are going to return the Formik component with the render props pattern.

<Formik
initialValues={initialValues}
validationSchema={validationSchema}
onSubmit={onSubmit}

> {

      formik => (
      <Form>
        <FormikControl
          control='input'
          type='email'
          label='Email'
          name='email'
        />
        <button type='submit'>Submit</button>
      </Form>)

}
</Formik>
)
}

## Textarea component

In this section we're going to build our second formikControl which is the textarea component. We are going to be following the exzct same steps we implemented in the previous section.

There are three distinct elements: a form label which is nothing but a label HTML element, a form input which is the Field component from formik which in turn should render a textarea HTML element and finally the field error which is the ErrorMessage component from formik.

To implement this textarea component let's take a look at the props required

props:

control = 'textarea'

first and foremost we set the control prop to textarea which is required to determine the type of formikControl we need to render

label='Description'

second, we need a label prop which will be the label text for the form field

name='description'

third, we pass in the all-important name prop which is required by formik for the field as well as the ErrorMessage component

We're going to implement this formik control again in three simple steps:

first step: we write the code in a new component specific to the field type, in our case the textarea component

second step: we write the code in the FormikControl component

third and final step: we write the code in the formik container component which will help us test the code we write in the browser.

first step:

/components/Textarea.js

import {Field, ErrorMessage} from 'formik'
import TextError from './TextError'

function Textarea(props){
const {label, name, ...rest} = props
return (

<div className='form-control'>
<label htmlFor={name}>{label}</label>
<Field as='textarea' id={name} name={name} {...rest} />
<ErrorMessage name={name} component={TextError}>
</div>
)
}

export default Textarea

second step: Add the code in our FormikControl component

/components/FormikControl.js

import Input from './Input'
import Textarea from './Textarea'

function FormikControl(props){
const {control, ...rest} = props

switch(control){
case 'input':
return <Input {...rest} />
case "textarea': return <Textarea {...rest}/>
case "select":
case "radio":
case "checkbox":
case "date":
default: return null

}

Third step: Add code in the FormikContainer component to test the code we have written in the first two steps

first we add the property in initialValues object

/components/FormikContainer

first we add a property in the initialValues object, then add the required validation to this field

import {Formik, Form} from 'formik'
import \* as Yup from 'yup'
import FormikControl from './FormikControl'

function FormikContainer(){
const initialValues = {
email = '',
description: ''
}
const validationSchema = Yup.object({
email: Yup.string().required('Required')
description: Yup.string().required('Required')
})
const onSubmit = values => console.log('Form data', values)
return (
//for the jsx we are going to return the Formik component with the render props pattern.

<Formik
initialValues={initialValues}
validationSchema={validationSchema}
onSubmit={onSubmit}

> {

      formik => (
      <Form>
        <FormikControl
          control='input'
          type='email'
          label:'Email'
          name='email'
        />


        <FormikControl
          control='textarea'
          label:'Description'
          name='description'
        />


        <button type='submit'>Submit</button>
      </Form>)

}
</Formik>
)
}

## Select Dropdown Component

In this section we are going to build our third FormikControl which is the Select dropdown component, we are going to be following the exact same steps we implemented in the previous videos.

In the Ui a Select FormikControl would have three distinct elements: a form label which is nothing but a label HTML element, a form input which is the Field component from formik which in turn should render a select HTML element and finally the field error which is the ErrorMessage component again from formik.

The field though on click of the form field renders a drop down with a list of options to choose from.

To implement this select component let's take a look at the props required:

props:

control='select'

first and foremost we set the control prop to select which is required to determine the type of formik control we need to render

label='Select a topic'

second we need a label prop which will be the label text for this form field

name='selectOption'

Third we pass in the all-important name prop which is required by formik for the field as well as the ErrorMessage component

options=[{key, value}]

another important prop is the options prop which is basically an array of objects. Each object will contain key value pairs which we use to populate the drop-down for our select component.

We're going to implement this formik control again in three simple steps:

first step: We write the code in a new component specific to the field type, in our case a select component

second step: we write the code in the FormikControl component

third and final step: we write the code in the formik container component which will help us test the code in the browser.

let's begin with the step one, for step one we need to create a new component for the select formik control.

/components/Select.js

for a select dropdown is similar to what we have seen in input and textarea but this time we also have to specify the list of options as children to the field component

import React form 'react'
import {Field, ErrorMessage} from 'formik'
import TextError from './TextError'

function Select(props) {
const {label, name, options, ...rest} = props
return (

<div className='form-control'>
<label htmlFor={name}>{label}</label>
<Field as='select' id={name} name={name} {...rest}>
{options.map(option => {
return (
<option key={option.value} value={option.value}>
{option.key}
</option>
)
})}
</Field>
<ErrorMessage name={name} component={TextError} />
</div>
)
}

export default Select

The second step is to add code in our FormikControl component

/components/FormikControl.js

import Input from './Input'
import Textarea from './Textarea'
import Select from './Select'

function FormikControl(props){
const {control, ...rest} = props

switch(control){
case 'input':
return <Input {...rest} />
case "textarea':
return <Textarea {...rest}/>
case "select":
return <Select {...rest} />
case "radio":
case "checkbox":
case "date":
default: return null

}

the third and final step is we add code in the FormikContainer component to test the code we have written for the first two steps.

Unlike the previous two sections our forst sub step in FormikContainer we are going to be defining a new constant called options which will contain the list of options for our drop-down

/components/FormikContainer

first we add a property in the initialValues object, then add the required validation to this field

import {Formik, Form} from 'formik'
import \* as Yup from 'yup'
import FormikControl from './FormikControl'

function FormikContainer(){

const dropdownOptions = [
{key:'Select an option', value: ''},
{key: 'option 1', value: 'option1'},
{key: 'option 2', value: 'option2'},
{key: 'option 3', value: 'option3'}
]

const initialValues = {
email = '',
description: '',
selectOption: ''
}

const validationSchema = Yup.object({
email: Yup.string().required('Required')
description: Yup.string().required('Required')
selectOption: Yup.string().required('Required')
})

const onSubmit = values => console.log('Form data', values)
return (
//for the jsx we are going to return the Formik component with the render props pattern.

<Formik
initialValues={initialValues}
validationSchema={validationSchema}
onSubmit={onSubmit}

> {

      formik => (
      <Form>
        <FormikControl
          control='input'
          type='email'
          label:'Email'
          name='email'
        />


        <FormikControl
          control='textarea'
          label:'Description'
          name='description'
        />


        <FormikControl
          control='select'
          label:'Select a topic'
          name='selectOption'
          options={dropdownOptions}
        />


        <button type='submit'>Submit</button>
      </Form>)

}
</Formik>
)
}

## Radio Buttons

we're going to build our fourth formik control which is the radio buttons component.

To implement a group of radio buttons we need to use the render props pattern for the field component. so it is not as straightforward as the previous three components but at the same time it's not difficult either.

In the UI radio buttons formik control would look like this. There are three distinct elements:

A form label which is nothing but a label HTML element

A form input which is the Field component from formik

And finally the field error which is the ErrorMessage component again from formik.

The field though is actually a list of HTML input and label elements which allow you to make only one selection.

This has to be considered when creating our radio buttons component.

let's take a look at the props that would be required to implement this component

props:

control='radio'

first and foremost we set the control props to radio which is required to determine the type of formik control we need to render

label='Pick one option'

second will be the label prop which will be the label text for form field.

name='radioOption'

And third we pass in the all-important name prop which is required by formik for the field as well as the error message components

options=[{key,value}]

Another essential prop is the options prop which is basically an array of objects. Each object will contain key-value pairs which we use to render the individual radio buttons for the component

We're going to implement this formik control again in three simple steps

first step we write the code in a new component specific to the field type, in our case a radio buttons component

second step we write code in the FormikControl component

third and final step we write the code in the FormikContainer component which will help us test the code in the browser

let's begin with step one

for step one we need to create a new component for the radio buttons formik control

/components/RadioButtons.js

import React from 'react'
import {Field, ErrorMessage} from 'formik'
import TextError from './TextError'

function RadioButtons(props){
const {label, name, options, ...rest} = props
return (

<div className='form-control'>
<label>{label}</label>
<Field name={name} {...rest}>
{
      ({field}) => {
        return options.map(option => {
          return (
            <React.Fragment key={option.key}>
              <input 
                 type='radio'
                 id={option.value}
                 {...field}
                 value={option.value}
                 checked={field.value === option.value} 
                />
                <label htmlFor={option.value}>{option.key}</label>
            </React.Fragment>
          )
        })
        }
      }
      </Field>
      <ErrorMessage name={name} component={TextError} />
    </div>

)
}

export default RadioButtons

The second step is to add code in our FormikControl component

/components/FormikControl.js

import Input from './Input'
import Textarea from './Textarea'
import Select from './Select'
import RadioButtons from './RadioButtons'

function FormikControl(props){
const {control, ...rest} = props

switch(control){
case 'input':
return <Input {...rest} />
case "textarea':
return <Textarea {...rest}/>
case "select":
return <Select {...rest} />
case "radio":
return <RadioButtons {...rest} />
case "checkbox":
case "date":
default: return null

}

For the third and final step we add code in the FormikContainer component to test the code we have written for the forst two steps

we're going to start off by defining a new constant called radio options which will contain the list of options for our radio buttons

/components/FormikContainer

second we add a property in the initialValues object, then add the required validation to this field

import {Formik, Form} from 'formik'
import \* as Yup from 'yup'
import FormikControl from './FormikControl'

function FormikContainer(){

const dropdownOptions = [
{key:'Select an option', value: ''},
{key: 'option 1', value: 'option1'},
{key: 'option 2', value: 'option2'},
{key: 'option 3', value: 'option3'}
]

const radioOptions = [
{key: 'Option 1', value: 'rOption1'},
{key: 'Option 2', value: 'rOption2'},
{key: 'Option 3', value: 'rOption3'},
]

const initialValues = {
email = '',
description: '',
selectOption: '',
radioOption: ''
}

const validationSchema = Yup.object({
email: Yup.string().required('Required')
description: Yup.string().required('Required')
selectOption: Yup.string().required('Required')
radioOption: Yup.string().required('Required')
})

const onSubmit = values => console.log('Form data', values)
return (
//for the jsx we are going to return the Formik component with the render props pattern.

<Formik
initialValues={initialValues}
validationSchema={validationSchema}
onSubmit={onSubmit}

> {

      formik => (
      <Form>
        <FormikControl
          control='input'
          type='email'
          label:'Email'
          name='email'
        />


        <FormikControl
          control='textarea'
          label:'Description'
          name='description'
        />


        <FormikControl
          control='select'
          label:'Select a topic'
          name='selectOption'
          options={dropdownOptions}
        />

        <FormikControl
          control='radio'
          label:'Radio Topic'
          name='radioOption'
          options={radioOptions}
        />


        <button type='submit'>Submit</button>
      </Form>)

}
</Formik>
)
}

## Checkbox Group

We're going to buid our fifth formik control which is the checkbox group component. The implementation of this component is very similar to the radio buttons component we implemented in the last section.

In The UI a checkbox group formik control would look like this:

There are again three distinct elements:

A form label which is nothing but a label HTML element.

A form input which is the field component from formik and finally the field error which is the ErrorMessage component again from formik.

The field though is a list of HTML input and label element which allows you to make one or more selections and this is a key difference from the radio buttons component.

Radio buttons are single selection whereas checkbox group is multi selection. This has to be considered when creating our component.

let's take a look at the props that would be required to implement a checkbox group:

props:

control='checkbox'

first and foremost we set the control prop to checkbox which is required to determine the type of formik control we need to render.

label='Pick options'

second we need a label prop which will be the label text for the form field

name='checkboxOption'

third, we pass in the all-important name prop which is required by formik for the field as well as the ErrorMessage component

options=[{key,value}]

Another essential prop is the options prop which is basically an array of objects, each onject will contain key-value pairs which we will use to render the individual checkboxes for the component

we're going to implement this formik control again in three simple steps, however the steps are going to be in the reverse order to what we have seen so far, for this section the reverse order makes more sense.

so first step, we write the code in the FormikContainer component which is our starting point

second step we write the code in FormikControl component

third and final step we write the code in a new component specific to the field type which in our case is the checkbox group component.

In the FormikContainer component we're going to create a new set of options.

/components/FormikContainer

second we add a property in the initialValues object, then add the required validation to this field

import {Formik, Form} from 'formik'
import \* as Yup from 'yup'
import FormikControl from './FormikControl'

function FormikContainer(){

const dropdownOptions = [
{key:'Select an option', value: ''},
{key: 'option 1', value: 'option1'},
{key: 'option 2', value: 'option2'},
{key: 'option 3', value: 'option3'}
]

const radioOptions = [
{key: 'Option 1', value: 'rOption1'},
{key: 'Option 2', value: 'rOption2'},
{key: 'Option 3', value: 'rOption3'},
]

const checkboxOptions = [
{key: 'Option 1', value: 'cOption1'},
{key: 'Option 2', value: 'cOption2'},
{key: 'Option 3', value: 'cOption3'},
]

const initialValues = {
email = '',
description: '',
selectOption: '',
radioOption: '',
checkboxOption: [] // this is a very important point to keep in mind. A checkbox group control let's you pick multiple values, we store the list of values in an array. Initially no value will be selected and hence the initialValue is an empty array
}

const validationSchema = Yup.object({
email: Yup.string().required('Required')
description: Yup.string().required('Required')
selectOption: Yup.string().required('Required')
radioOption: Yup.string().required('Required')
checkboxOption: Yup.array().required('Required')
})

const onSubmit = values => console.log('Form data', values)
return (
//for the jsx we are going to return the Formik component with the render props pattern.

<Formik
initialValues={initialValues}
validationSchema={validationSchema}
onSubmit={onSubmit}

> {

      formik => (
      <Form>
        <FormikControl
          control='input'
          type='email'
          label:'Email'
          name='email'
        />


        <FormikControl
          control='textarea'
          label:'Description'
          name='description'
        />


        <FormikControl
          control='select'
          label:'Select a topic'
          name='selectOption'
          options={dropdownOptions}
        />

        <FormikControl
          control='radio'
          label:'Radio Topic'
          name='radioOption'
          options={radioOptions}
        />

        <FormikControl
          control='checkbox'
          label:'Checkbox Topics'
          name='checkboxOption'
          options={checkboxOptions}
        />


        <button type='submit'>Submit</button>
      </Form>)

}
</Formik>
)
}

for the second step we add the code in FormikControl component. We dont have the checkbox group component yet but let's add the code assuming that we do.

/components/FormikControl.js

import Input from './Input'
import Textarea from './Textarea'
import Select from './Select'
import RadioButtons from './RadioButtons'
import CheckBoxGroup from './CheckboxGroup'

function FormikControl(props){
const {control, ...rest} = props

switch(control){
case 'input':
return <Input {...rest} />
case "textarea':
return <Textarea {...rest}/>
case "select":
return <Select {...rest} />
case "radio":
return <RadioButtons {...rest} />
case "checkbox":
return <CheckboxGroup {...rest}/>
case "date":
default: return null

}

the final step is to create this CheckboxGroup component

/components/CheckboxGroup

import React from 'react'
import {Field, ErrorMessage} from 'formik'
import TextError from './TextError'

function CheckboxGroup (props){
const {label, name, options, ...rest} = props
return (

<div className='form-control'>
<label>{label}</label>
<Field name={name} {...rest}>
{
      ({field}) => {
        return options.map(option => {
          return (
            <React.Fragment key={option.key}>
              <input 
                 type='checkbox'
                 id={option.value}
                 {...field}
                 value={option.value}
                 checked={field.value.includes(option.value)}

                 //we now basically check if the value for the checkbox is present in the array of values for this entire field, if it is present checked is set to true
                />
                <label htmlFor={option.value}>{option.key}</label>
            </React.Fragment>
          )
        })
        }
      }
      </Field>
      <ErrorMessage name={name} component={TextError} />
    </div>

)
}

export default CheckboxGroup

## Date Picker

we're going to build our sixth and final formik control which is the date picker component.

The implementation of this component is similar to the radio buttons and checkbox component in the sense that we will again be using the render props pattern for the field component. However we will use a new method that formik provides to set a fields value and since we have already seen in detail how that works, in this section we're going to focus more on how to use a date picker library with formik

the package which we'll be using is called react-datepicker

we have a DatePicker component and we have two props: selected which indicates the selected date and the onChange prop

the onChange handler receives the updated date which is then packed back into the selected prop through a state variable

Make sure you keep these two props in mind because our datepicker component is pretty much this with some formik code

we're going to implement this formik control again in three simple steps:

first step: we write the code in a new component specific to the field type, in our case a date picker component

second step: we write the code in the FormikControl component

third and final step : we write the code in the formContainer component which will help us test the code in the browser

so let's begin with step one

for step one we need to create a new component for the date picker formikControl

/components/DatePicker.js

npm i react-datepicker

once the installation completes we need to import the component and the CSS

we've called the default export from the library as DateView since we're already calling the component as DatePicker

import React from 'react'
import DateView from 'react-datepicker'
import 'react-datepicker/dist/react-datepicker.css'
import {Field, ErrorMessage} from 'formik'
import TextError from './TextError'

function DatePicker(props){

const {label, name, ...rest} = props

return (

<div className='form-control'>
  <label htmlFor={name}>{label}</label>
  <Field name={name}>
  {
    ({form, field}) => {
      const {setFieldValue} = form
      const {value} = field

      return(
        <DateView
          id={name}
          {...field}
          {...rest}
          selected={value}
          onChange={val => setFieldValue(name, val)}
        />
      )
    }

}
</Field>

<ErrorMessage name={name} components={TextError} />

</div>
)
}

In the previous two sections, we used to only destructure the field property from the render props, for this component though we also need to destructure form in addition to field.

From the form prop we are further going to destructure a method called setFieldValue. This is the new method we are seeing for the first time in the series.

What this method does is it allows you to programmatically set a field's value in the formik state

After this, from the field prop we are going to destrucure the value property. This basically gives the value of the date picker at any given time

Now the render props pattern needs to return the JSX which is the DateView component from the library
