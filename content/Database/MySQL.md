> Cú pháp của `MySQL` và `SQL Server` có thể gần giống nhau nhưng cần phải lưu ý trong cú pháp của `MySQL` phải có chấm phẩy đồng thời khi đặt tên bị trùng với command của `MySQL` hoặc tên chứa khoảng trắng thì phải dùng **dấu nháy ngược \` chứ không dùng nhấu ngoặc vuông như SQL Server.**
# Create table
```mysql
CREATE DATABASE IF NOT EXISTS testdb;  -- Tạo Database nếu chưa có

USE testdb;

CREATE TABLE IF NOT EXISTS User (
	UserId INT AUTO_INCREMENT,
	Name NVARCHAR(100) NOT NULL,
	Email NVARCHAR(100) UNIQUE NOT NULL,
	PRIMARY KEY (UserId)
);

CREATE TABLE IF NOT EXISTS `Order` (   -- Dấu nháy ngược vì Order là từ khóa
	OrderId INT AUTO_INCREMENT,
	Address NVARCHAR(100) NOT NULL,
	Country NVARCHAR(100),
	PRIMARY KEY (OrderId)
);

CREATE TABLE IF NOT EXISTS `Order Detail` ( -- Dùng dấu nháy ngược cho tên bảng có khoảng trắng
	UserId INT,
	OrderId INT,
	PRIMARY KEY (UserId, OrderId),
	FOREIGN KEY (UserId) REFERENCES User(UserId),
	FOREIGN KEY (OrderId) REFERENCES `Order`(OrderId)
);
```
# SELECT, INSERT INTO, DELETE
> Ví dụ đề bài cho 2 bảng dữ liệu User và Order.
> 1. Tạo bảng và nhập dữ liệu.
> 2. Thực hiện truy vấn User theo từng Order.
> 3. Xóa 2 bảng.
```mysql
CREATE TABLE User (
	UserId INT AUTO_INCREMENT,
	Name NVARCHAR(100) NOT NULL,
	PRIMARY KEY (UserId)
)

CREATE TABLE `Order` (
	OrderId INT AUTO_INCREMENT,
	UserId INT,
	Country NVARCHAR(100),
	PRIMARY KEY (OrderId)
)
```