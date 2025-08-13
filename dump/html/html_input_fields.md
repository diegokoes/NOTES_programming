# HTML -> INPUT FIELDS

## INPUTS 
The `<input>` tag is versatile and used for different types of data. Depending on the `type` attribute, its behavior changes.

Common types:

- **`text`**: Short text field.
- **`password`**: Password field (hides characters).
- **`email`**: Validates that the format is an email.
- **`number`**: Allows only numbers.
- **`date`**, **`time`**, **`datetime-local`**, **`month`**, **`week`**: Date and time selection.
- **`url`**: Validates that the format is a URL.
- **`tel`**: Phone number field.
- **`color`**: Color picker.
- **`checkbox`**: Checkboxes (multiple selection).
- **`radio`**: Radio buttons (exclusive selection).
- **`range`**: Slider control for selecting numeric values.
- **`file`**: Allows the user to select files to upload.
- **`hidden`**: Invisible field, useful for sending hidden data.

Useful attributes:

- **`required`**: Mandatory field.
- **`placeholder`**: Helper text inside the field.
- **`min`**, **`max`**: Minimum and maximum values.
- **`pattern`**: Regular expression to validate the format.
- **`step`**: Increments by which the value increases (useful for numbers and dates).
- **`autofocus`**: Automatically focuses the field when the page loads.
- **`checked`**: Indicates that a checkbox or radio button is selected by default.
- **`value`**: Initial value of the field.

```html
<label for="age">Age:</label>
<input type="number" id="age" name="age" min="0" max="120" required>

<label>
  <input type="checkbox" name="interests" value="music"> Music
</label>
<label>
  <input type="checkbox" name="interests" value="sports" checked> Sports
</label>
<label>
  <input type="checkbox" name="interests" value="art"> Art
</label>

<label>
  <input type="radio" name="gender" value="male" checked> Male
</label>
<label>
  <input type="radio" name="gender" value="female"> Female
</label>

<label for="file">Upload file:</label>
<input type="file" id="file" name="file">
```

### **CHECKBOXES (`checkbox`)**

- Allow multiple selections.
- The **`checked`** attribute indicates that the checkbox is selected by default.
```html
<!-- Multiple selection of interests -->
<label>
  <input type="checkbox" name="interests" value="cinema"> Cinema
</label>
<label>
  <input type="checkbox" name="interests" value="reading" checked> Reading
</label>
<label>
  <input type="checkbox" name="interests" value="travel"> Travel
</label>
```

### **RADIO BUTTONS (`radio`)**

- Allow selecting only one option among several (exclusive selection).
- Ensure that all radio buttons in the same group have the same `name` attribute value.
- The **`checked`** attribute indicates which one is selected by default.
```html
<!-- Exclusive selection of gender -->
<label>
  <input type="radio" name="gender" value="male"> Male
</label>
<label>
  <input type="radio" name="gender" value="female" checked> Female
</label>
<label>
  <input type="radio" name="gender" value="other"> Other
</label>
``` 

### **SELECT FIELD (`<select>`)**

Allows creating dropdown menus to select one or multiple options.

```html
<label for="country">Country:</label>
<select id="country" name="country">
  <option value="es" selected>Spain</option>
  <option value="mx">Mexico</option>
  <option value="ar">Argentina</option>
  <option value="co">Colombia</option>
</select>
```
- **`<option>`**: Defines each option in the dropdown menu.
- The **`value`** attribute in `<option>` defines the value sent to the server.
- The **`selected`** attribute in `<option>` indicates that the option is selected by default.
#### **MULTIPLE SELECTION IN `<select>`**

- Add the **`multiple`** attribute to `<select>` to allow selecting multiple options.
- You can define the number of visible options with the **`size`** attribute.
```html
<label for="fruits">Choose your favorite fruits:</label>
<select id="fruits" name="fruits" multiple size="4">
  <option value="apple">Apple</option>
  <option value="banana" selected>Banana</option>
  <option value="orange">Orange</option>
  <option value="strawberry" selected>Strawberry</option>
</select>
```

To send all selected options to the server, ensure that the **`name`** attribute ends with `[]` (array).
```html
<select id="fruits" name="fruits[]" multiple size="4">
  <!-- Options -->
</select>
```

### **OPTION GROUPING (`<optgroup>`)**

- Allows grouping options within a `<select>`.
```html
<label for="city">City:</label>
<select id="city" name="city">
  <optgroup label="Spain">
    <option value="madrid">Madrid</option>
    <option value="barcelona">Barcelona</option>
  </optgroup>
  <optgroup label="Mexico">
    <option value="cdmx">Mexico City</option>
    <option value="guadalajara">Guadalajara</option>
  </optgroup>
</select>
```

## `<label>` TAG

- Associated with a form control to improve accessibility.
- Clicking on the label focuses the associated field.
- The association can be done in two ways:
    - Using the **`for`** attribute in `<label>` and the **`id`** attribute in the field.
    - Wrapping the field within `<label>`.
```html
<!-- Method with 'for' and 'id' -->
<label for="user">User:</label>
<input type="text" id="user" name="user">

<!-- Method by wrapping the field -->
<label>
  <input type="checkbox" name="accept" required> I accept the terms and conditions
</label>
```

## BUTTONS

- **`<button>`**: More flexible than `<input type="button">`, allows including content inside (text, images).
- Common attributes:
    - **`type`**: Defines the button type (`submit`, `reset`, `button`).
    - **`value`**: Value sent to the server (when it's a submit button).
```html
<button type="submit">Submit</button>
<button type="reset">Reset</button>
<button type="button" onclick="alert('Hello')">Click Here</button>
```

## TEXT AREA (`<textarea>`)

Field for long text inputs. You can control its size with the **`rows`** and **`cols`** attributes, or preferably with CSS.
```html
<label for="comments">Comments:</label>
<textarea id="comments" name="comments" rows="5" cols="40" placeholder="Write your comments here"></textarea>
```

## DATA LIST SELECTORS (`<datalist>`)

Provides a list of predefined options for an input field, allowing the user to choose or enter a custom value.

```html
<label for="browser">Choose your browser:</label>
<input list="browsers" id="browser" name="browser" placeholder="Select or type">
<datalist id="browsers">
  <option value="Chrome">
  <option value="Firefox">
  <option value="Edge">
  <option value="Safari">
  <option value="Opera">
</datalist>
```

## GROUPING CONTROLS (`<fieldset>` AND `<legend>`)

Used to group related elements within a form, improving organization and accessibility.
```html
<fieldset>
  <legend>Personal Information</legend>
  
  <label for="firstName">First Name:</label>
  <input type="text" id="firstName" name="firstName" required>

  <label for="lastName">Last Name:</label>
  <input type="text" id="lastName" name="lastName" required>
</fieldset>

<fieldset>
  <legend>Contact Details</legend>
  
  <label for="email">Email:</label>
  <input type="email" id="email" name="email" required>

  <label for="phone">Phone:</label>
  <input type="tel" id="phone" name="phone">
</fieldset>
```
## SELECTORS (`<select>` AND `<option>`)

Allows creating dropdown menus.

```html
<label for="country">Country:</label>
<select id="country" name="country">
  <option value="es">Spain</option>
  <option value="mx">Mexico</option>
  <option value="ar">Argentina</option>
</select>
```

## GAUGES (`<progress>` AND `<meter>`)

### **`<progress>`**

Indicates the progress of an ongoing task, like a progress bar.

- **`value`**: Current progress.
- **`max`**: Maximum value (default is 1).
```html
<label for="downloadProgress">Download Progress:</label>
<progress id="downloadProgress" value="30" max="100">30%</progress>
```

### **`<meter>`**

Represents a scalar value within a known range, such as a measurement or rating.

- **`value`**: Current value.
- **`min`**, **`max`**: Minimum and maximum values.
- **`low`**, **`high`**: Defines low and high ranges.
- **`optimum`**: Optimal value.
```html
<label for="batteryLevel">Battery Level:</label>
<meter id="batteryLevel" value="0.6">60%</meter>
```

## FORM SUBMISSION

- **`<input type="submit">`**: Button to submit the form.
- **`<input type="reset">`**: Button to reset the form fields.
- **`<button type="submit">`**: More flexible alternative to submit the form.
```html
<input type="submit" value="Send">
<input type="reset" value="Reset">

<button type="submit">Submit Form</button>
```

## FORM VALIDATION

Modern browsers provide basic validation:

- **`required`**: Mandatory field.
- **`pattern`**: Validates the field with a regular expression.
- **`min`**, **`max`**: Minimum and maximum values.
- **`step`**: Defines increments (e.g., by 10).
```html
<label for="username">Username:</label>
<input type="text" id="username" name="username" required pattern="[A-Za-z0-9_]{5,15}" title="Between 5 and 15 alphanumeric characters">

<label for="age">Age:</label>
<input type="number" id="age" name="age" min="18" max="100" required>
```

## USE OF PLACEHOLDERS AND TITLES

- **`placeholder`**: Shows helper text inside the field.
- **`title`**: Provides additional information when hovering over the field.
```html
<input type="email" name="email" placeholder="email@example.com" required title="Enter a valid email">
```

## HIDDEN FIELDS (`<input type="hidden">`)

Used to send information to the server without the user seeing or being able to modify it.
```html
<input type="hidden" name="token" value="1234567890">
```

## FORM ATTRIBUTES

- **`novalidate`**: Disables form validation upon submission.

```html
<form action="process.php" method="post" novalidate>
  <!-- Form fields -->
</form>
```

**`autocomplete`**: Controls whether the browser can automatically complete the fields. Values: `on` (default) or `off`.

```html 
<input type="text" name="username" autocomplete="off">
```

## `<output>` TAG

Displays the result of a calculation or action.

```html
<label for="quantity">Quantity:</label>
<input type="number" id="quantity" name="quantity" oninput="calculateTotal()">

<label for="price">Unit Price:</label>
<input type="number" id="price" name="price" value="10" readonly>

<label>Total:</label>
<output id="total">0</output>

<script>
function calculateTotal() {
  const quantity = document.getElementById('quantity').value;
  const price = document.getElementById('price').value;
  const total = quantity * price;
  document.getElementById('total').value = total;
}
</script>
```

## `form` ATTRIBUTE

Allows associating controls with a specific form using the `form` attribute with the form's `id`, even if they are outside the `<form>`.

```html
<form id="myForm" action="process.php">
  <!-- Form fields -->
</form>

<input type="text" name="extraField" form="myForm">
```

## `disabled` ATTRIBUTE

Disables a form control, preventing interaction and submission to the server.

```html
<input type="text" name="username" disabled placeholder="Disabled field">
```

## `readonly` ATTRIBUTE

The field is read-only; it cannot be modified but is still submitted to the server.

```html
<input type="text" name="code" value="ABC123" readonly>
```

## ACCESSIBLE FORMS

- Use `<label>` tags for all fields.
- Provide clear information in `placeholders` and `titles`.
- Ensure that fields are properly grouped and structured.

## COMPLETE FORM EXAMPLE

```html
<form action="register.php" method="post">
  <fieldset>
    <legend>User Registration</legend>
    
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" required>

    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>

    <label for="password">Password:</label>
    <input type="password" id="password" name="password" required>

    <label for="confirm_password">Confirm Password:</label>
    <input type="password" id="confirm_password" name="confirm_password" required>

    <label>Gender:</label>
    <label>
      <input type="radio" name="gender" value="male"> Male
    </label>
    <label>
      <input type="radio" name="gender" value="female"> Female
    </label>
    <label>
      <input type="radio" name="gender" value="other"> Other
    </label>

    <label for="country">Country:</label>
    <select id="country" name="country">
      <option value="es">Spain</option>
      <option value="mx" selected>Mexico</option>
      <option value="ar">Argentina</option>
    </select>

    <label>Interests:</label>
    <label>
      <input type="checkbox" name="interests[]" value="technology"> Technology
    </label>
    <label>
      <input type="checkbox" name="interests[]" value="music" checked> Music
    </label>
    <label>
      <input type="checkbox" name="interests[]" value="sports"> Sports
    </label>

    <label for="biography">Biography:</label>
    <textarea id="biography" name="biography" rows="4" cols="50" placeholder="Tell us about yourself"></textarea>

    <label>
      <input type="checkbox" name="terms" required> I accept the terms and conditions
    </label>

    <button type="submit">Register</button>
  </fieldset>
</form>
```

- - -
#html