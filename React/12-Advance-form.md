
---
# React Advanced Form: Radio, Checkbox, and Select

This example shows a React form with **radio buttons**, **checkbox**, and **select dropdown** using a single state object.

---

## Code: `AdvanceForm.js`

```javascript
import { useState } from "react";

const AdvanceForm = () => {
  const [formData, setFormData] = useState({
    gender: "",
    country: "India",
    agree: false,
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(formData);
  };

  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;
    setFormData((prevData) => ({
      ...prevData,
      [name]: type === "checkbox" ? checked : value,
    }));
  };

  return (
    <form onSubmit={handleSubmit}>
      <h2>Advance Form Component : checkbox, radio, select</h2>
      <label>
        <input
          type="radio"
          name="gender"
          value="male"
          checked={formData.gender === "male"}
          // onChange={(e) => setFormData({...formData, gender: e.target.value})}
          onChange={handleChange}
        />
        Male
      </label>
      <br />
      <label>
        <input
          type="radio"
          name="gender"
          value="female"
          checked={formData.gender === "female"}
          // onChange={(e) => setFormData({...formData, gender: e.target.value})}
          onChange={handleChange}
        />
        Female
      </label>
      <br />
      <br />
      <label>
        Select your country:
        <select
          name="country"
          value={formData.country}
          // onChange={(e) => setFormData({ ...formData, country: e.target.value })}
          onChange={handleChange}
        >
          <option value="USA">USA</option>
          <option value="India">India</option>
          <option value="Japan">Japan</option>
          <option value="Bhutan">Bhutan</option>
          <option value="Australia">Australia</option>
          <option value="Thailand">Thailand</option>
        </select>
      </label>
      <br />
      <br />
      <label>
        <input
          type="checkbox"
          name="agree"
          checked={formData.agree}
          // onChange={(e) => setFormData({ ...formData, agree: e.target.checked })}
          onChange={handleChange}
        />
        I agree to the terms and conditions
      </label>
      <br />
      <br />
      <button type="submit">Submit</button>
    </form>
  );
};

export default AdvanceForm;
```


### 📘 Understanding Your `AdvanceForm` Component

---

## 1️⃣ The State: `formData`

```jsx
const [formData, setFormData] = useState({
  gender: "",
  country: "India", //can be empty too, ""
  agree: false,
});
```

### Explanation:

- **`formData`**: This is a state object that stores **all form values**.
    
- Initial values:
    
    - `gender: ""` → no radio button selected initially
        
    - `country: "India"` → dropdown defaults to India
        
    - `agree: false` → checkbox unchecked initially
        
- **`setFormData`**: This is a function that updates the state when the user changes something in the form.
    

💡 Why use one object for all inputs?

- Cleaner code
    
- Easy to handle multiple inputs with one `onChange` function
    
- Scales well when the form gets bigger
    

---

## 2️⃣ Universal `handleChange` Function

```jsx
const handleChange = (e) => {
  const { name, value, type, checked } = e.target;

  setFormData((prevData) => ({
    ...prevData,
    [name]: type === "checkbox" ? checked : value,
  }));
};
```

### Let’s understand this line by line:

1. **`e` (event)**:
    
    - This is the event object that React passes automatically when a user interacts with an input.
        
    - Example: when user clicks a radio button, types in a select, or checks a checkbox.
        
2. **Destructuring `e.target`**:
    

```jsx
const { name, value, type, checked } = e.target;
```

- `name` → the `name` attribute of the input.
    
    - Radio buttons, checkbox, and select all have `name` in your form.
        
    - Example: `"gender"` or ``or `"agree"`.
        
- `value` → the value of the input.
    
    - Example: for radio button `<input value="male" />`, value is `"male"`.
        
- `type` → the type of input: `"radio"`, `"checkbox"`, `"select-one"`, `"text"`.
    
- `checked` → `true` or `false` for checkboxes (and also works for radio buttons internally).
    

3. **Update state using `setFormData`**:
    

`setFormData((prevData) => ({   ...prevData,   [name]: type === "checkbox" ? checked : value, }));`

- `prevData` → the current state
    
- `...prevData` → copy all existing fields
    
- `[name]: type === "checkbox" ? checked : value` → update only the changed field
    
    - If checkbox, use `checked` (true/false)
        
    - Else, use `value` (string)
        

💡 This makes **one function work for all input types**. No need for multiple `onChange` functions.

---

## 3️⃣ Handling Radio Buttons

Radio buttons allow **only one option to be selected at a time**.

`<input   type="radio"   name="gender"   value="male"   checked={formData.gender === "male"}   onChange={handleChange} />`

- `type="radio"` → React knows it’s a radio button
    
- `name="gender"` → important! All radio buttons of same group must have the same name.
    
- `value="male"` → the value stored when this radio button is selected
    
- `checked={formData.gender === "male"}` → controlled input
    
    - Only checked if state matches value
        
- `onChange={handleChange}` → updates the state when user clicks
    

---

### Why `checked={formData.gender === "male"}`?

- React **controls the radio button** through state.
    
- If `formData.gender` is `"male"`, the Male radio button is checked.
    
- If `"female"`, the Female radio button is checked.
    
- This ensures **one source of truth**.
    

---

## 4️⃣ Handling Select Dropdown

`<select   name="country"   value={formData.country}   onChange={handleChange} >   <option value="USA">USA</option>   <option value="India">India</option>   <option value="Japan">Japan</option> </select>`

- `value={formData.country}` → controlled input
    
- `onChange={handleChange}` → updates state
    
- `name="country"` → tells `handleChange` which field to update
    
- Example flow:
    
    1. User selects `"Japan"`
        
    2. `handleChange` runs
        
    3. `formData.country` becomes `"Japan"`
        
    4. React re-renders dropdown with new `value`
        

💡 **Tip**: Always include `value` in controlled select.

---

## 5️⃣ Handling Checkbox

`<input   type="checkbox"   name="agree"   checked={formData.agree}   onChange={handleChange} />`

- `checked={formData.agree}` → controlled by state (true/false)
    
- `onChange={handleChange}` → updates state automatically
    
- Checkbox stores **boolean**, not string
    

---

## 6️⃣ Handling Form Submission

`const handleSubmit = (e) => {   e.preventDefault(); // stop page reload   console.log(formData); };`

- `e.preventDefault()` → prevents the page from reloading
    
- `console.log(formData)` → shows **all form data at once**
    

---

## 7️⃣ What Happens Step by Step

**Example**: User selects Female, country Japan, and checks checkbox.

1. Clicks **Female radio**
    
    - `handleChange` runs
        
    - `name = "gender"`, `value = "female"`, `type = "radio"`
        
    - State updates: `formData.gender = "female"`
        
2. Selects **Japan** in dropdown
    
    - `handleChange` runs
        
    - `name = "country"`, `value = "Japan"`, `type = "select-one"`
        
    - State updates: `formData.country = "Japan"`
        
3. Checks **agree checkbox**
    
    - `handleChange` runs
        
    - `name = "agree"`, `checked = true`, `type = "checkbox"`
        
    - State updates: `formData.agree = true`
        
4. Clicks Submit
    
    - `handleSubmit` runs
        
    - `formData` contains:
        

`{   gender: "female",   country: "Japan",   agree: true }`

---

## ✅ Key Beginner Tips

1. Always give **name attribute** to inputs
    
2. Use **checked** for checkbox/radio, **value** for text/select
    
3. Use **one universal handleChange** for multiple inputs
    
4. **Do not mutate state directly**. Always use `setFormData`
    
5. `onChange` is mandatory for controlled inputs
    

---

## 💡 Visual Summary

|Input Type|Attribute to use|State type|Notes|
|---|---|---|---|
|Text/Email/Password|`value`|string|use `onChange`|
|Radio|`checked`|string|same `name` for group|
|Checkbox|`checked`|boolean|onChange stores `true/false`|
|Select|`value`|string|controlled like text input|