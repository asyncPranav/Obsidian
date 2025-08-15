
---

## **1. What is React Hook Form?**

**React Hook Form (RHF)** is a library for managing forms in React using hooks.  
It’s designed to be **fast, minimal, and developer-friendly**, compared to the traditional way of handling forms (using `useState` for every input).

---

### **Without React Hook Form (Manual way)**

- You would:
    
    - Create a `useState` for every input
        
    - Write `onChange` handlers
        
    - Handle validation manually
        
    - Manually gather data on submit
        

Example:

```jsx
const [name, setName] = useState("");
const [email, setEmail] = useState("");

function handleSubmit(e) {
  e.preventDefault();
  if (!name) { /* show error */ }
  if (!email) { /* show error */ }
  console.log({ name, email });
}
```

This works, but for big forms → it’s **too much boilerplate**.

---

### **With React Hook Form**

- You **don’t** need separate `useState` for every field.
    
- You **don’t** need manual `onChange` handlers.
    
- Validation is **built-in** and declarative.
    
- Data gathering is automatic.
    

Example:

```jsx
const { register, handleSubmit } = useForm();

<form onSubmit={handleSubmit(onSubmit)}>
  <input {...register("name")} />
  <input {...register("email")} />
</form>
```

---

## **2. Core Concepts**

### **useForm()**

The main hook from RHF.  
It sets up form state, validation, and helpers.

```jsx
const {
  register,      // Connects inputs to RHF
  handleSubmit,  // Handles validation + triggers onSubmit
  watch,         // Lets you watch input values in real time
  formState: { errors }, // Stores validation errors
} = useForm();
```

---

### **register()**

The `register` function **connects** an input to RHF's internal state.  
You pass it a **field name** and optionally **validation rules**.

Example:

```jsx
<input {...register("userName", { required: true, minLength: 3 })} />
```

- `"userName"` → Key in the final form data object.
    
- `{ required: true, minLength: 3 }` → Validation rules.
    

---

### **handleSubmit()**

This is **not** your submit function.  
It’s a **wrapper** that:

1. Prevents default page refresh
    
2. Validates all registered inputs
    
3. Calls your custom `onSubmit` with the collected data
    

```jsx
<form onSubmit={handleSubmit(onSubmit)}>
```

---

### **formState.errors**

This object holds validation errors.  
You can display them per input.

```jsx
{errors.userName && <p>Name is required</p>}
```

---

### **watch()**

Watches form values in real time. Useful for:

- Live previews
    
- Conditional rendering
    

```jsx
const watchName = watch("userName");
console.log(watchName);
```

---

## **3. Full Example: Basic Form**

```jsx
import { useForm } from "react-hook-form";
import "./App.css";

function App() {
  // Initialize React Hook Form
  const {
    register,
    handleSubmit,
    watch,
    formState: { errors }
	} = useForm();
	
	// Function runs when validation passes
  function onSubmit(data) {
    console.log("Form submitted: ", data);
  }

  return (
    <div>
      <h1>React Hook Form Example</h1>
      <form onSubmit={handleSubmit(onSubmit)}>
        
        {/* Name Field */}
        <div>
          <label>Name: </label>
          <input
            type="text"
            {...register("userName", { required: "Name is required" })}
          />
          {errors.userName && <p>{errors.userName.message}</p>}
        </div>
		
        {/* Email Field */}
        <div>
          <label>Email: </label>
          <input
            type="email"
            {...register("userEmail", {
              required: "Email is required",
              pattern: { value: /^\S+@\S+$/i, message: "Invalid email" }
            })}
          />
          {errors.userEmail && <p>{errors.userEmail.message}</p>}
        </div>
		
        {/* Watching Example */}
        <p>Live Name Preview: {watch("userName")}</p>
		
        <button type="submit">Submit</button>
      </form>
    </div>
  );
}

export default App;
```

---

## **4. How the Flow Works**

1. `useForm()` initializes the form system.
    
2. `register()` tells RHF:
    
    - "Track this input"
        
    - "Use this key in form data"
        
    - "Apply these validation rules"
        
3. `handleSubmit(onSubmit)`:
    
    - Prevents refresh
        
    - Runs validations
        
    - If no errors → calls `onSubmit(data)`
        
4. `errors` object holds any validation failures.
    
5. `watch()` lets you see input values in real time.
    

---

## **5. Advantages of React Hook Form**

✅ Minimal re-renders (performance-friendly)  
✅ No `useState` for each field  
✅ Built-in validation  
✅ Smaller bundle size compared to Formik  
✅ Easy integration with UI libraries like Material UI, Chakra UI, etc.


---


## 📌 All `formState` Properties

|Property|Type|Description|
|---|---|---|
|**`isDirty`**|boolean|`true` if at least one field value is different from the `defaultValues`.|
|**`dirtyFields`**|object|Keys are the names of fields that have been changed, values are `true`.|
|**`isValid`**|boolean|`true` if all fields pass validation (requires `mode: 'onChange'` or `'all'` for live updates).|
|**`isSubmitted`**|boolean|`true` once the form has been submitted (even once).|
|**`isSubmitting`**|boolean|`true` while `handleSubmit` is running an async function.|
|**`isSubmitSuccessful`**|boolean|`true` if the last submit was successful (onSubmit finished without throwing).|
|**`submitCount`**|number|How many times the form was submitted.|
|**`touchedFields`**|object|Fields that have been focused/blurred at least once.|
|**`isLoading`**|boolean|`true` if async `defaultValues` are still being loaded.|
|**`isValidating`**|boolean|`true` if async validation is still running.|
|**`errors`**|object|Holds validation errors for each field (if any).|

---

## 📌 Example Code Showing All `formState` Fields

```jsx
import { useForm } from "react-hook-form";

export default function App() {
  const {
    register,
    handleSubmit,
    formState: {
      errors,
      isDirty,
      dirtyFields,
      isValid,
      isSubmitted,
      isSubmitting,
      isSubmitSuccessful,
      submitCount,
      touchedFields,
      isLoading,
      isValidating
    }
  } = useForm({
    mode: "onChange", // so isValid updates live
    defaultValues: {
      userName: "",
      userEmail: ""
    }
  });

  async function onSubmit(data) {
    console.log("Form data:", data);
    await new Promise(res => setTimeout(res, 1500)); // simulate delay
    console.log("✅ Submitted");
  }

  return (
    <div>
      <h1>React Hook Form - formState Example</h1>

      <form onSubmit={handleSubmit(onSubmit)}>
        <div>
          <label>Name:</label>
          <input
            {...register("userName", {
              required: "Name is required",
              minLength: { value: 3, message: "Min length is 3" }
            })}
          />
          {errors.userName && <p>{errors.userName.message}</p>}
        </div>

        <div>
          <label>Email:</label>
          <input
            {...register("userEmail", {
              required: "Email is required",
              pattern: {
                value: /^\S+@\S+$/i,
                message: "Invalid email"
              }
            })}
          />
          {errors.userEmail && <p>{errors.userEmail.message}</p>}
        </div>

        <button type="submit" disabled={!isValid || isSubmitting}>
          {isSubmitting ? "Submitting..." : "Submit"}
        </button>
      </form>

      <hr />
      <h2>Form State Debug</h2>
      <pre>{JSON.stringify({
        isDirty,
        dirtyFields,
        isValid,
        isSubmitted,
        isSubmitting,
        isSubmitSuccessful,
        submitCount,
        touchedFields,
        isLoading,
        isValidating,
        errors
      }, null, 2)}</pre>
    </div>
  );
}

```

---

## 📌 How These Fields Help in Real Projects

- **`isDirty`** → Warn users before leaving page if changes are unsaved.
    
- **`dirtyFields`** → Send only updated fields to backend.
    
- **`isValid`** → Enable submit only when valid.
    
- **`isSubmitting`** → Show loading spinner.
    
- **`isSubmitSuccessful`** → Show success message after submit.
    
- **`submitCount`** → Track retries or limit attempts.
    
- **`touchedFields`** → Show validation errors only after user touched a field.
    
- **`isValidating`** → Show "checking..." while async validation runs.
    
- **`errors`** → Display per-field error messages.


---


## `useForm()` main signature

```jsx
const {
  register,
  handleSubmit,
  watch,
  setValue,
  getValues,
  reset,
  resetField,
  trigger,
  setError,
  clearErrors,
  control,
  formState: {
    errors,
    isDirty,
    isValid,
    isSubmitting,
    touchedFields,
    dirtyFields,
    submitCount,
    isSubmitSuccessful,
  }
} = useForm(options);
```


---

## 1️⃣ Functions