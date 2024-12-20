## Preventing SQL Injection When Coding - Input Validation
*By Logan Limbaugh*

### <ins>Overview</ins><br>
Input Validation is the process of validating or filtering a users input to remove malicious characters or text and creating a system of regulation to ensure a users input is correct!

For the purpose of this guide, all examples will be given in Python.

--- 

### <ins>Vulnerable Code</ins><br>
The code below is vulnerable to SQL injection as it directly takes the user input and places it within the query. For this code, assume that the user_input variable is collected from a form, and the SQL code is the code designed to process their search with the database.<br>


    user_input = "Decorations'; DROP TABLE Products; --"
	query = f"SELECT ProductName, ProductDesc, Price FROM Products WHERE ItemName = '{user_input}'"
	cursor.execute(query)

This code is vulnerable to injection as it would create a query that looks like this, succesfully injecting into the database and deleting the table "Products".<br>

    SELECT ProductName, ProductDesc, Price FROM Products WHERE ItemName = 'Decorations'; DROP TABLE Products; --'
    
---

### <ins>Code with Input Validation</ins><br>
To add input validation to our code, we can simply create a system of filtering through the user input and removing common characters or words used to perform SQL injection. Below I have created a simple function that ensures that the users input only contains uppercase, lowercase, and numeric characters. I have also included parameterization, which we learned in the previous guide! In the code below, the query would fail to execute as it would be flagged, therefore printing our error message!
	
	user_input = "Decorations'; DROP TABLE Products; --" 

	def validate_input(user_input): 
		if user_input.isalnum(): 
			return True 
		else: 
			return False
			
	if validate_input(user_input):
		query = "SELECT ProductName, ProductDesc, Price FROM Products WHERE ItemName = ?"
		cursor.execute(query, (user_input,)) 
	else: 
		print("Error: Invalid input!")

---

#### Return to the Main Page -> [Here](https://github.com/Loganhl/SQL-Injection-Prevention/blob/main/README.md)
