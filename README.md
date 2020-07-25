# mui-rff-template

> An easy template/generator that allows users to quicky create a responsive form with mui and rff. Uses Yup for validation

[![NPM](https://img.shields.io/npm/v/mui-rff-template.svg)](https://www.npmjs.com/package/mui-rff-template) [![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

## Install

```bash
npm install --save mui-rff-template
```

# Docs

This utility allows any user to quickly bootstrap a responsive with minimal overhead.
The validationSchema is written through Yup. The Frontend framework is MaterialUI.

# Requirements

- yup
- @material-ui/core
- @material-ui/lab
- @material-ui/pickers
- react-final-form
- final-form
- mui-rff

## data (Array) [Required]

An array of objects that represent fields in your form. Each object has subfields:

### name (String) [Required]

Name assigned to the field in your form. This name must match the corresponding keys in defaultValues and validation schema

### type (String) [Required]

The type of your form field. Currently only supports 'text' or 'select'

### label (String) [Required]

The label for the field

### options (Array) [Required: type === 'autocomplete' || 'select' || 'radio' || 'checkbox']

Required if your type is 'select' or 'autocomplete'. This is an array of of objects, where each object is an option in your dropdown. The object must contain EXACTLY 'label' and 'value' keys if the type is checkbox or radio. If the type is autocomplete or select, the keys can be overwritten with the props below. If you do not provide a 'value' key the label key will also be treated as the value.

### optionLabelKey (String) [Required]

Will overwrite the 'label' key for objects in the options array.

```js
#Default

{
...
options: [{ label: 'SomeName', value: 'SomeValue'}]
}

#Using optionLabelKey

{
...
optionLabelKey: 'realName'
options: [{ realName: 'SomeName', value: 'SomeValue'}]
}

```

### optionValueKey (String) [Optional]

Will overwrite the 'value' key for objects in the options array.

```js
#Default

{
...
options: [{ label: 'SomeName', value: 'SomeValue'}]
}

#Using optionValueKey

{
...
optionValueKey: 'realValue'
options: [{ label: 'SomeName', realValue: 'SomeValue'}]
}

```

### multiple (String) [Optional: type === 'autocomplete']

Will allow a multiselect option for autocomplete fields. This requires that the initial autoc0omplete value passed in is an array.

## validationSchema [Required]

A Yup object that has the same keys as defaultValues and the same names as the objects in the data prop

```js
validationSchema = Yup.object().shape({
  firstName: Yup.string()
    .min(2, 'Too Short!')
    .max(50, 'Too Long!')
    .required('Required'),
  lastName: Yup.string()
    .min(2, 'Too Short!')
    .max(50, 'Too Long!')
    .required('Required'),
  email: Yup.string().email('Invalid email').required('Required')
})
```

## cancel [Optional]

Determines whether or not there will be a cancel button.

## handleSubmit [Required]

A function that is to be executed upon submission of the form.

## handleCancel [Optional]

A function that is to be executed upon canceling the form. If this prop is not provided, it will just reset the form.

## initialValues [Required]

An object of defaultValues to be assigned to the form. This is a REQUIRED prop.
The keys should match up with thosei n the validationSchema and the names of the objects in the data prop.

```js
{ firstName: "", lastName: "", email: "" }
```

# Basic Example

```js
import * as Yup from 'yup'

export const data = [
  {
    name: 'username',
    label: 'Username',
    type: 'text'
  },
  { name: 'password', label: 'Password', type: 'password' },
  { name: 'date', label: 'Date', type: 'date' },
  { name: 'time', label: 'Time', type: 'time' },
  {
    name: 'email',
    label: 'Email',
    type: 'select',
    options: [
      {
        realName: 'skanadian3@gmail.com'
      }
    ],
    optionLabelKey: 'realName'
  },
  {
    name: 'autocomplete',
    label: 'Autocomplete',
    type: 'autocomplete',
    options: [
      { title: 'Movie' },
      { title: 'TV Show' },
      { title: 'Videogame' },
      { title: 'Good TV Show' },
      { title: 'Bad Movie' }
    ],
    optionLabelKey: 'title'
  },

  {
    name: 'checkbox',
    type: 'checkbox',
    options: [{ label: 'Remember Me?', value: true }]
  },
  {
    name: 'Radio',
    type: 'radio',
    options: [
      { label: 'Mustard', value: 'Mustard' },
      { label: 'Ketchup', value: 'Ketchup' },
      { label: 'Relish', value: 'Relish' }
    ]
  }
]

export const validationSchema = Yup.object().shape({
  username: Yup.string()
    .min(2, 'Too Short! ')
    .max(50, 'Too Long! ')
    .required('Required'),
  password: Yup.string()
    .min(2, 'Too Short! ')
    .max(50, 'Too Long! ')
    .required('Required'),
  email: Yup.string().email('Invalid email').required('Required'),
  date: Yup.date().required('Required').nullable(),
  time: Yup.date().required('Required').nullable(),
  autocomplete: Yup.string().required('Required')
})

export const initialValues = {
  username: '',
  password: '',
  email: '',
  date: null,
  time: null,
  autocomplete: ''
}
```

```js
import React, { Component } from 'react'
import FormTemplate from './FormTemplate'

import Paper from '@material-ui/core/Paper'
import Grid from '@material-ui/core/Grid'

import {
  data,
  validationSchema,
  initialValues
} from './forms/form-template-example'

class FormTemplateExample extends Component {
  handleSubmit = (data) => {
    console.log(data)
  }

  render() {
    return (
      <Grid container justify='center'>
        <Paper style={{ padding: 16, width: '50%' }}>
          <FormTemplate
            data={data}
            handleSubmit={this.handleSubmit}
            validationSchema={validationSchema}
            initialValues={initialValues}
          ></FormTemplate>
        </Paper>
      </Grid>
    )
  }
}

export default FormTemplateExample
```

## License

MIT © [Naveed-Naqi](https://github.com/Naveed-Naqi)
