# Formik

表單組件：

{% embed url="https://formik.org/docs/overview" %}

簡單範例:

```javascript
import { Formik, Form, Field, ErrorMessage } from 'formik';

const initialValues = { email: '', password: '' };
const Basic = () => (
  <div>
    <h1>Anywhere in your app!</h1>
    <Formik
      initialValues={initialValues}
      validate={values => {
        const errors = { email: '', password: '' };
        if (!values.password || values.password.length < 3) {
          errors.password = 'Required > 3';
        } else {
          errors.password = '';
        }
        if (!values.email) {
          errors.email = 'Required';
        } else if (
          !/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i.test(values.email)
        ) {
          errors.email = 'Invalid email address';
        } else {
          errors.email = ''
        }
        return errors.email || errors.password ? errors : {};
      }}
      onSubmit={(values, { setSubmitting }) => {
        console.log(123123, values)
        setTimeout(() => {
          alert(JSON.stringify(values, null, 2));
          setSubmitting(false);
        }, 400);
      }}
    >
      {({
        values,
        errors,
        touched,
        handleChange,
        handleBlur,
        handleSubmit,
        isSubmitting,
        /* and other goodies */
      }) => (
          <form onSubmit={handleSubmit}>
            <input
              type="email"
              name="email"
              onChange={handleChange}
              onBlur={handleBlur}
              value={values.email}
            />
            <div style={{ color: 'red', marginBottom: '5px' }}>{errors.email && touched.email && errors.email}</div>
            <input
              type="password"
              name="password"
              onChange={handleChange}
              onBlur={handleBlur}
              value={values.password}
            />
            <div style={{ color: 'red', marginBottom: '5px' }}>{errors.password && touched.password && errors.password}</div>
            <button type="submit" disabled={isSubmitting}>
              Submit
          </button>
          </form>
        )}
    </Formik>
  </div>
);

function App() {
  return (
    <div className="App">
      <Basic />
    </div>
  );
}

export default App;
```

1. &lt;Form /&gt; 可以讓 onSubmit 省略 

   ```javascript
   <form onSubmit={handleSubmit}>

   <Form />
   ```

   2. &lt;Field /&gt; 可以省略 onChange, onBlur, value

```javascript
<input
  type="email"
  name="email"
  onChange={handleChange}
  onBlur={handleBlur}
  value={values.email}
/>

<Field   
  type="email"
  name="email" 
/>
```

3.ErrorMessage

```javascript
<div style={{ color: 'red', marginBottom: '5px' }}>{errors.email && touched.email && errors.email}</div>

<ErrorMessage 
  name="email"
  render={msg => <div style={{ color: 'red', marginBottom: '5px' }}>{msg}</div>} 
/>
```

## 完整版：加上驗證 Yup

```javascript
import { Formik, Form, Field, ErrorMessage } from 'formik';
import * as Yup from 'yup';

const initialValues = { email: '', password: '' };
const Basic = () => (
  <div>
    <Formik
      initialValues={initialValues}
      validationSchema={Yup.object({
        password: Yup.string()
          .min(5, 'Must be 5 characters')
          .required('Required'),
        email: Yup.string().email('Invalid email address').required('Required'),
      })}
      onSubmit={(values, { setSubmitting }) => {
        setTimeout(() => {
          alert(JSON.stringify(values));
          setSubmitting(false);
        }, 400);
      }}
    >
      {({
        isSubmitting,
        /* and other goodies */
      }) => (
          <Form>
            <Field
              type="email"
              name="email"
            />
            <ErrorMessage
              name="email"
              render={msg => <div style={{ color: 'red', marginBottom: '5px' }}>{msg}</div>}
            />
            <Field
              type="password"
              name="password"
            />
            <ErrorMessage
              name="password"
              render={msg => <div style={{ color: 'red', marginBottom: '5px' }}>{msg}</div>}
            />
            <button type="submit" disabled={isSubmitting}>
              Submit
          </button>
            <button type="reset"> Reset</button>
          </Form>
        )}
    </Formik>
  </div>
);

function App() {
  return (
    <div className="App">
      <Basic />
    </div>
  );
}

export default App;

```

## 客製化 \(useField, useFormik\)

```javascript
 const MyCheckbox = ({ children, ...props }) => {
   // React treats radios and checkbox inputs differently other input types, select, and textarea.
   // Formik does this too! When you specify `type` to useField(), it will
   // return the correct bag of props for you
   const [field, meta] = useField({ ...props, type: 'checkbox' });
   return (
     <div>
       <label className="checkbox">
         <input type="checkbox" {...field} {...props} />
         {children}
       </label>
       {meta.touched && meta.error ? (
         <div className="error">{meta.error}</div>
       ) : null}
     </div>
   );
 };
```

