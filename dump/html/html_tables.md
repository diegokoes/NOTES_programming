# HTML -> TABLES

Tables are useful for organizing data into rows and columns.
```html
<table>
  <caption>Contact List</caption>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ana</td>
      <td>ana@example.com</td>
    </tr>
    <tr>
      <td>Luis</td>
      <td>luis@example.com</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="2">Total contacts: 2</td>
    </tr>
  </tfoot>
</table>
```
- **`<table>`**: Defines the table.
    
- **`<caption>`**: Title of the table.
    
- **`<thead>`**, **`<tbody>`**, **`<tfoot>`**: Groups sections of the table.
    
- **`<tr>`**: Defines a row.
    
- **`<th>`**: Defines a header cell.
    
- **`<td>`**: Defines a data cell.
    
- Attributes:
    
    - **`colspan`**: Merges cells horizontally.
    - **`rowspan`**: Merges cells vertically.

- - -
#html
````
