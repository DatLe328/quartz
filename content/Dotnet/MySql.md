# Connect to MySQL
```csharp
	string connectionString = "server=localhost;user=root;password=your_password;database=testdb";
	using (MySqlConnection conn = new MySqlConnection(connectionString))
	{
		try
		{
			conn.Open();
			string createTableQuery = @"
			CREATE TABLE IF NOT EXISTS users (
				id INT AUTO_INCREMENT PRIMARY KEY,
				name VARCHAR(100) NOT NULL,
				email VARCHAR(100) UNIQUE NOT NULL
			)";

			using (MySqlCommand cmd = new MySqlCommand(createTableQuery, conn))
			{
				cmd.ExecuteNonQuery();
				Console.WriteLine("Bảng users đã được tạo!");
			}
		}
		catch (Exception ex)
		{
			Console.WriteLine("Lỗi kết nối: " + ex.Message);
		}
	}
```
# Select, Insert, Delete, Update
## Select
```csharp
void ReadUsers()
{
    using (MySqlConnection conn = new MySqlConnection(connectionString))
    {
        conn.Open();
        string query = "SELECT * FROM users";
        
        using (MySqlCommand cmd = new MySqlCommand(query, conn))
        using (MySqlDataReader reader = cmd.ExecuteReader())
        {
            while (reader.Read())
            {
                Console.WriteLine($"ID: {reader["id"]}, Name: {reader["name"]}, Email: {reader["email"]}");
            }
        }
    }
}
```
## Insert
```csharp
void InsertUser(string name, string email)
{
    using (MySqlConnection conn = new MySqlConnection(connectionString))
    {
        conn.Open();
        string query = "INSERT INTO users (name, email) VALUES (@name, @email)";
        
        using (MySqlCommand cmd = new MySqlCommand(query, conn))
        {
            cmd.Parameters.AddWithValue("@name", name);
            cmd.Parameters.AddWithValue("@email", email);
            cmd.ExecuteNonQuery();
            Console.WriteLine("Thêm thành công!");
        }
    }
}
```
## Update
```csharp
void UpdateUser(int id, string newName)
{
    using (MySqlConnection conn = new MySqlConnection(connectionString))
    {
        conn.Open();
        string query = "UPDATE users SET name = @newName WHERE id = @id";
        
        using (MySqlCommand cmd = new MySqlCommand(query, conn))
        {
            cmd.Parameters.AddWithValue("@newName", newName);
            cmd.Parameters.AddWithValue("@id", id);
            cmd.ExecuteNonQuery();
            Console.WriteLine("Cập nhật thành công!");
        }
    }
}
```
## Delete
```csharp
void DeleteUser(int id)
{
    using (MySqlConnection conn = new MySqlConnection(connectionString))
    {
        conn.Open();
        string query = "DELETE FROM users WHERE id = @id";
        
        using (MySqlCommand cmd = new MySqlCommand(query, conn))
        {
            cmd.Parameters.AddWithValue("@id", id);
            cmd.ExecuteNonQuery();
            Console.WriteLine("Xóa thành công!");
        }
    }
}

```