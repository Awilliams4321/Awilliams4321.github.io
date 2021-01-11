---
layout: post
title:      "Flatiron Project #4: Rails API Backend/JavaScipt Frontend"
date:       2021-01-11 14:37:39 -0500
permalink:  flatiron_project_4_rails_api_backend_javascipt_frontend
---


In this module, we covered a completely new programming language that is unlike anything we have learned thus far, JAVASCRIPT! JavaScript is used for front-end web programming and allows you to update content and servers based on events.

For this modules project, I built a bill organizer web application that has a Rails API backend and a JavaScript frontend. Seeing it come together really helped me get a feel for the flow of a web app and get familiar with JS concepts. I began my app by using the rails command 
```
rails new bill-organizer-backend --api
```
This command sets up your rails backend while removing any middleware primarily used by browser applications. It also configures the generators to skip generating views, helpers, and assets when you generate a new resource.


Once i setup my Rails models, tables, and controllers, I then created a frontend directory for my JavaScript files to exist in. I immediately added script tags for the .js files i would be needing for each of my models. Script tags exist in the index.hmtl file and is used to embed/connect .js files to the sever.
```
<script src="./src/bill.js"></script>
<script src="./src/category.js"></script>
```

After running the command 'open index.html' in my terminal to view my web page, and checking my developer tools to make sure the.js files were connected, the foundation was set for me to begin working on my project. Below are some JS methods that truly came in handy while working through my project:

**.addEventListener()  ** 

.addEventListener is a function that you call on a JS object that takes in two arguments, an event + a callback function. I used this method as a submit event for my bill form, the JS object i called it on is the variable i set equal to the Id i coded into the form in my ** index.html** file:


```
<form id="bill-form" >
        <label for="categories">Choose a category:</label>

        <select name="categories" id="category-id">
        <option value="1">Debt</option>
        <option value="2">Housing</option>
        <option value="3">Insurance</option>
        <option value="4">Medical</option>
        <option value="5">Personal</option>
        <option value="6">School Loans</option>
        <option value="7">Subscriptions</option>
        <option value="8">Utilties</option>
        <option value="9">Vehicle</option>
        <option value="10">Other</option>
        </select><br><br>

        <label for="name">Bill Name:</label>
        <input type="text" id="bill-name" name="name"><br><br>
        <label for="creditor">Creditor:</label>
        <input type="text" id="creditor" name="creditor"><br><br>
        <label for="balance_owed">Balance Owed:</label>
        <input type="text" id="balance-owed" name="balance_owed"><br><br>
        <label for="monthly_payment">Monthly Payment Amount:</label>
        <input type="text" id="monthly-payment" name="monthly_payment"><br><br>
        <label for="due_date">Due Date:</label>
        <input type="text" id="due-date" name="due_date"><br><br>
        <input type="submit" value="Submit" id="bill-submit">
    </form><br><br>
```

in** index.js file**(.addEventListener()):
```
const submitBillForm = document.getElementById("bill-form")

submitBillForm.addEventListener("submit", Bill.postBill)
```

When the form is submitted(or "clicked") on the web page, the event will be triggered and sent to the method Bill.postBill method below:

```
static postBill(e) {
        e.preventDefault()
        const billName = document.getElementById("bill-name")
        const creditor = document.getElementById("creditor")
        const balanceOwed = document.getElementById("balance-owed")
        const monthlyPayment = document.getElementById("monthly-payment")
        const dueDate = document.getElementById("due-date")
        const categoryId = document.getElementById("category-id")

        fetch(billsUrl, {
            method: "POST",
            headers: {
                "Content-type": "application/json",
                "Accept": "application/json"
            },
            body: JSON.stringify({
                name: billName.value,
                creditor: creditor.value,
                balance_owed: balanceOwed.value,
                monthly_payment: monthlyPayment.value,
                due_date: dueDate.value,
                category_id: parseInt(categoryId.value)
            })
        })
        .then(response => response.json())
        .then(bill => {
            let newBill = new Bill(bill.data)
        })
    }
		```
		
The method above contains JS variables set to the ids of the inputs in the bill form, which will allow me to access the data stored inside via the varible name. I then made a fetch request that POST's the submitted data to my database. In order for Rails to understand the data it is being sent, I have to define within the fetch request, the "Content-type" the data is and what data type should be "Accept"ed. I then stringify the JSON objects and set the Bill models attributes equal to the value of the inputs submitted in the bill form. As you can see, the parseInt() funciton was needed for the category_id attribute because in my html form, the category name values are set to number which are returned as strings. Rails cant read/accept data in string format so parseInt alllowed me to parse the string submitted and return the integer value.

These are just some of the basic JS functions i got to work with. I cant wait to continue practicing and add more features to my app!
