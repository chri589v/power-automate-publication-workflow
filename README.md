## Flow Explanation
This flow automates the creation of structured tasks in Microsoft Planner based on form input.

## High-level logic

1. A user submits a Microsoft Form
2. The flow retrieves the response
3. The data is formatted into a structured text block
4. A Planner task is created using the formatted data

## Description generation (key logic)
The task description is generated using a **single Compose action with a custom expression**.
This is the most important part of the flow.

### Why use an expression?

Instead of using multiple conditions or Compose steps, this approach:

- Produces consistent formatting
- Avoids unnecessary empty lines
- Reduces flow complexity
- Works reliably with Planner and Teams

## Important limitation (line breaks)
The Planner extension in Power Automate does not handle line breaks like standard text or HTML.

You cannot use:

- \n
- <br>

Instead, line breaks must be created using:

- Plain TextdecodeUriComponent('%0A')``Vis flere linjer

### Example expression

text'''
trim(concat(

if(empty(trim(outputs('Get_response_details')?['body/<AUTHORS_FIELD_ID>'])), '', concat('Authors:', decodeUriComponent('%0A'), outputs('Get_response_details')?['body/<AUTHORS_FIELD_ID>'], decodeUriComponent('%0A%0A'))),

if(empty(trim(outputs('Get_response_details')?['body/<PAGE_COUNT_FIELD_ID>'])), '', concat('Page count:', decodeUriComponent('%0A'), outputs('Get_response_details')?['body/<PAGE_COUNT_FIELD_ID>'], decodeUriComponent('%0A%0A'))),

if(empty(trim(outputs('Get_response_details')?['body/<SUMMARY_FIELD_ID>'])), '', concat('Summary:', decodeUriComponent('%0A'), outputs('Get_response_details')?['body/<SUMMARY_FIELD_ID>'], decodeUriComponent('%0A%0A')))

))'''

### How it works

For each field:

- If a value exists:
  - Add label
  - Add line break (%0A)
  - Add value
  - Add spacing (%0A%0A)

- If the field is empty:
  - Skip it

## Example output
text'''
Authors:
John Doe

Page count:
45

Summary:
This publication analyzes...
'''

### Why not use conditions?
Using Conditions + multiple Compose steps leads to:

- messy formatting
- empty lines
- harder-to-maintain flows

The expression-based approach avoids these issues and keeps the output clean.

## Customization

To adapt the flow:

- Replace <FIELD_ID> with your own form fields
- Add or remove sections as needed
- Modify labels to fit your use case
