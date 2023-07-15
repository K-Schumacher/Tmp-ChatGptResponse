Me: How to avoid SQL injection when allowing raw user input into a query in C# or JavaScript

ChatGPT: In C# and JavaScript, you can also use parameterized queries to avoid SQL injection when allowing raw user input into a query. Here's how you can do it in both languages:

1. C# (using ADO.NET):
```csharp
using System.Data.SqlClient;

// Establish a connection to the database
string connectionString = "Data Source=server;Initial Catalog=mydatabase;User ID=username;Password=password;";
using (SqlConnection connection = new SqlConnection(connectionString))
{
    connection.Open();

    // Example query with a parameter
    string userInput = "John";
    string query = "SELECT * FROM users WHERE name = @Name";
    using (SqlCommand command = new SqlCommand(query, connection))
    {
        command.Parameters.AddWithValue("@Name", userInput);
        using (SqlDataReader reader = command.ExecuteReader())
        {
            // Fetch and process the results
            while (reader.Read())
            {
                string name = reader.GetString("name");
                // Process the retrieved data
            }
        }
    }
}
```

In the above example, the `@Name` parameter is used in the query, and the user input is added as a parameter value using `command.Parameters.AddWithValue("@Name", userInput)`. This way, the user input is properly sanitized and treated as data, preventing SQL injection.

2. JavaScript (using Node.js and mysql2 package):
```javascript
const mysql = require('mysql2/promise');

// Create a connection pool
const pool = mysql.createPool({
  host: 'localhost',
  user: 'username',
  password: 'password',
  database: 'mydatabase'
});

// Example query with a parameter
const userInput = 'John';
const query = 'SELECT * FROM users WHERE name = ?';

(async () => {
  // Get a connection from the pool
  const connection = await pool.getConnection();

  try {
    // Execute the query with the parameter
    const [rows] = await connection.query(query, [userInput]);

    // Fetch and process the results
    for (const row of rows) {
      console.log(row.name);
      // Process the retrieved data
    }
  } finally {
    // Release the connection back to the pool
    connection.release();
  }
})();
```

In this JavaScript example, the `?` placeholder is used in the query, and the user input is passed as an array in the `connection.query` function call. The query execution is properly handled by the `mysql2` package, preventing SQL injection.

By using parameterized queries in both C# and JavaScript, you ensure that user input is treated as data and not executable SQL code, thereby protecting your application from SQL injection attacks.