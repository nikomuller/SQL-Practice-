# SQL-Practice-
This is just a repository containing small practice SQL questions.


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



