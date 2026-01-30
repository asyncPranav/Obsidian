
---
# React Advanced Form Handling: Radio, Checkbox, and Select

In this tutorial, we'll go through a React form that includes **radio buttons**, **checkboxes**, and **select dropdowns**, and explain how to handle all these inputs using `useState`.

---

## Complete Code: `AdvanceForm.js`

```jsx
import { useState } from "react";

const AdvanceForm = () => {
  // 1. Define form state using useState
  const [formData, setFormData] = useState({
    gender: "",     // For radio buttons
    country: "India", // For select dropdown
    agree: false,   // For checkbox
  });

  // 2. Handle form submission
  const handleSubmit = (e) => {
    e.preventDefault(); // Prevent page reload
    console.log(formData); // Display form data in console
  };

  // 3. Handle input changes
  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;

    // Update state based on input type
    setFormData((prevData) => ({
      ...prevData,
      [name]: type === "checkbox" ? checked : value,
    }));
  };

  return (
    <form onSubmit={handleSubmit}>
      <h2>Advance Form Component: checkbox, radio, select</h2>

      {/* Radio Buttons */}
      <label>
        <input
          type="radio"
          name="gender"
          value="male"
          checked={formData.gender === "male"}
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
          onChange={handleChange}
        />
        Female
      </label>

      <br /><br />

      {/* Select Dropdown */}
      <label>
        Select your country:
        <select
          name="country"
          value={formData.country}
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

      <br /><br />

      {/* Checkbox */}
      <label>
        <input
          type="checkbox"
          name="agree"
          checked={formData.agree}
          onChange={handleChange}
        />
        I agree to the terms and conditions
      </label>

      <br /><br />

      <button type="submit">Submit</button>
    </form>
  );
};

export default AdvanceForm;
