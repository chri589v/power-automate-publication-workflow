## Flow Explanation

This flow automates the creation of structured tasks in Microsoft Planner based on form input.

---

## Flow Structure

![Flow structure](Structure.PNG)

The flow consists of:

1. **When a new response is submitted**
2. **Get response details**
3. **Compose (description logic)**
4. **Create a task**
5. **Update task details**

---

## High-level Logic

1. A user submits a Microsoft Form  
2. The flow retrieves the response  
3. The data is formatted into a structured text block  
4. A Planner task is created using the formatted data  

---

## Description Generation (Key Logic)

The task description is generated using a **single Compose action with a custom expression**.  
This is the most important part of the flow.

---

## Why Use an Expression?

Instead of using multiple conditions or Compose steps, this approach:

- Produces consistent formatting  
- Avoids unnecessary empty lines  
- Reduces flow complexity  
- Works reliably with Planner and Teams  

---

## Important Limitation: Line Breaks

The Planner connector in Power Automate does **not** handle line breaks like standard text or HTML.

## Use This Instead

```text
trim(
  concat(
    if(
      empty(trim(outputs('Get_response_details')?['body/<AUTHORS_FIELD_ID>'])),
      '',
      concat(
        'Authors:',
        decodeUriComponent('%0A'),
        outputs('Get_response_details')?['body/<AUTHORS_FIELD_ID>'],
        decodeUriComponent('%0A%0A')
      )
    ),

    if(
      empty(trim(outputs('Get_response_details')?['body/<PAGE_COUNT_FIELD_ID>'])),
      '',
      concat(
        'Page count:',
        decodeUriComponent('%0A'),
        outputs('Get_response_details')?['body/<PAGE_COUNT_FIELD_ID>'],
        decodeUriComponent('%0A%0A')
      )
    ),

    if(
      empty(trim(outputs('Get_response_details')?['body/<SUMMARY_FIELD_ID>'])),
      '',
      concat(
        'Summary:',
        decodeUriComponent('%0A'),
        outputs('Get_response_details')?['body/<SUMMARY_FIELD_ID>'],
        decodeUriComponent('%0A%0A')
      )
    )
  )
)
```

## How It Works
