# E-commerce-dataset
CREATE DATABASE OnlineShopping;
GO

USE OnlineShopping;
GO

-- Customer Table
CREATE TABLE Customer (
    CustomerID INT IDENTITY(1, 1) PRIMARY KEY,
    FirstName VARCHAR(255) NOT NULL,
    LastName VARCHAR(255) NOT NULL,
    DOB DATE NOT NULL,
    Email VARCHAR(255) NOT NULL,
    Password VARCHAR(255) NOT NULL,
    Contact VARCHAR(255) NOT NULL
);

-- Country Table
CREATE TABLE Country (
    CountryID INT IDENTITY(1, 1) PRIMARY KEY,
    CountryName VARCHAR(255) NOT NULL
);

-- Province Table
CREATE TABLE Province (
    ProvinceID INT IDENTITY(1, 1) PRIMARY KEY,
    ProvinceName VARCHAR(255) NOT NULL
);

-- City Table
CREATE TABLE City (
    CityID INT IDENTITY(1, 1) PRIMARY KEY,
    CityName VARCHAR(255) NOT NULL
);

-- ZipCode Table
CREATE TABLE ZipCode (
    ZipCodeID INT IDENTITY(1, 1) PRIMARY KEY,
    CityID INT NOT NULL FOREIGN KEY REFERENCES City(CityID),
    ProvinceID INT NOT NULL FOREIGN KEY REFERENCES Province(ProvinceID),
    CountryID INT NOT NULL FOREIGN KEY REFERENCES Country(CountryID)
);

-- Address Table
CREATE TABLE Address (
    AddressID INT IDENTITY(1, 1) PRIMARY KEY,
    HouseNo VARCHAR(255) NOT NULL,
    Street INT NOT NULL,
    CustomerID INT NOT NULL FOREIGN KEY REFERENCES Customer(CustomerID),
    ZipCodeID INT NOT NULL FOREIGN KEY REFERENCES ZipCode(ZipCodeID),
    Area VARCHAR(255) NOT NULL
);

-- Additional Tables You Referenced (Define them as well)

CREATE TABLE Category (
    CategoryID INT IDENTITY(1, 1) PRIMARY KEY,
    CategoryName VARCHAR(255) NOT NULL
);

CREATE TABLE Product (
    ProductID INT IDENTITY(1, 1) PRIMARY KEY,
    ProductName VARCHAR(255) NOT NULL,
    CategoryID INT FOREIGN KEY REFERENCES Category(CategoryID)
);

CREATE TABLE VendorProduct (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    VendorID INT NOT NULL,
    ProductID INT NOT NULL FOREIGN KEY REFERENCES Product(ProductID)
);

CREATE TABLE Orders (
    OrderID INT IDENTITY(1, 1) PRIMARY KEY,
    CustomerID INT,
    OrderDate DATE,
    TotalAmount DECIMAL(10,2),
    PhoneNumber VARCHAR(20)
);

CREATE TABLE Review (
    ReviewID INT IDENTITY(1, 1) PRIMARY KEY,
    Rating INT,
    Comment TEXT,
    ProductID INT,
    CustomerID INT
);

-- Insert Sample Data
INSERT INTO Category (CategoryName) VALUES ('Android Smart TV Box/Air Mouse');
INSERT INTO City (CityName) VALUES ('Karachi');
INSERT INTO Province (ProvinceName) VALUES ('Punjab');

-- Note: Add appropriate values for foreign keys
-- Sample Query
SELECT CategoryName
FROM Category
WHERE CategoryID IN (
    SELECT DISTINCT CategoryID
    FROM Product
    WHERE ProductID IN (
        SELECT ProductID
        FROM VendorProduct
        WHERE VendorID = 3
    )
);
