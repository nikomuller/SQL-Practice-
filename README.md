# SQL-Practice-
This is just a repository containing small queries to practice my SQL.


CREATE OR REPLACE TYPE CustomerType AS OBJECT 
(
    CustomerID INT,
    CustomerName VARCHAR2(20),
    DOB DATE,
    Gender VARCHAR2(20),
    MainSport VARCHAR2(20)
);

CREATE TABLE CUSTOMERObjTable OF CustomerType
(
    CustomerID PRIMARY KEY,
    CustomerName NOT NULL
);

CREATE OR REPLACE TYPE SportsGearType AS OBJECT
(
    SportsGearID INT,
    SportsGearName VARCHAR2(20),
    SizeOfGear VARCHAR2(20),
    Price INT
);


CREATE TYPE SPORTSGEAR_TABLE_TYPE AS TABLE OF SportsGearType;

CREATE OR REPLACE TYPE InvoiceCType AS OBJECT
(
    InvoiceID INT,
    InvoiceDate DATE,
    CustDiscountAmt INT,
    TotalInvoiced INT,
    StoreName VARCHAR2(20),
    Customer REF CustomerType,
    SportsGear SPORTSGEAR_TABLE_TYPE
);

CREATE TABLE INVOICESObjTable OF InvoiceCType
(
    InvoiceID PRIMARY KEY,
    InvoiceDate NOT NULL,
    StoreName NOT NULL,
    CUSTOMER SCOPE IS CUSTOMERObjTable
) NESTED TABLE SportsGear STORE AS SPORTSGEAR_TABLE_TYPE;



INSERT INTO CUSTOMERObjTable VALUES
( 
--CustomerID, CustomerName, DoB, Gender, MainSport
 1, 'Bob Smith', '01-Jun-2000', 'M', 'Football'
);
INSERT INTO CUSTOMERObjTable VALUES
( 2, 'Mary Smith', '02-Oct-2001', 'F', 'GAA');


INSERT INTO INVOICESObjTable
( SELECT 2, '18-Oct-2023', 'Tallaght Store'
    REF(dt), -- The reference to the deliverer Mary Smith
    SPORTSGEAR_TABLE_TYPE
    (
    SportsGearType(1, 'Boots', 6, 50.99),
    SportsGearType(2, 'Shin Guards', 7, 60.99),
    SportsGearType(3, 'Helmet', 9, 60.00)
    )
    FROM CUSTOMERObjTable dt
    WHERE dt.CUSTOMERNAME = 'Bob Smith'
);


SELECT 
I.Customer.CUSTOMERNAME "CustomerName", I.MainSport, I.StoreName
from INVOICESObjTable I , table(I.SportsGear) M
order by 1,8;

SELECT I.InvoiceID, I.InvoiceDate, SG.SportsGear
FROM INVOICESObjtable I
(I.SportsGear) AS SG;





1. Count how many deliveries have there been at 17:30 with a kms travelled of < than 5 km
-- db.deliveries.count({
   "deliveryTime": "17:30",
   "kmsTravelled": { 5 }
})

-- 0


2. Display the car reg for car model 'Kia Sportage'
-- db.deliveries.find({ "vehicle.model": "Kia Sportage" }, { "vehicle.reg": 1, "_id": 0 })

-- Vehicle Reg: 171D14421


3. Update error for the driver that has a mobile number of 087787839089
-- db.deliveries.update({ "mobileNo": "087787839089" }, { $set: { "driverAddress.streetNo": "165A" } })


4. Count how many customers live in Tallaght that driver 'Hazel Brooks' delivered to
-- db.deliveries.count({
   "drivername": "Hazel Brooks",
   "deliveryAddress": "Tallaght"
})

-- 0


5. What are the names of the drivers that are aged between 50 and 80
-- db.deliveries.find({
   "age": { $gte: 50, $lte: 80 }
}, { "drivername": 1, "_id": 0 })

-- drivername: 'James Carr' }, { drivername: 'Hazel Brooks'}


6. Display the mobile number of only driver James Carr
-- db.deliveries.find({ "drivername": "James Carr" }, { "mobileNo": 1, "_id": 0 })

-- mobileNo == 08778783456


7. What is the age of drivers that drivers that driver either a blue car or a Hyundai Ascent
-- db.deliveries.find({
   $or: [
      { "vehicle.color": "blue" },
      { "vehicle.model": "Hyundai Ascent" }
   ]
}, { "age": 1, "_id": 0 })

-- Age : 55


9. List out all the areas Henry Shannon delivered to
-- db.deliveries.distinct("deliveryAddress.area", { "drivername": "Henry Shannon" })


10. Delete all documents that delivered to customer Anna Dempsey on August 2nd
-- db.deliveries.remove({
   	"deliveryTime": {("deliveryDate.date" : "Aug 02 2020")},
   	"deliveryAddress.customerName": "Anna Dempsey"
	})



