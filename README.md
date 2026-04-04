# NAME:
# Sp26_71552_Group 4: "Last-Mile" Urban Logistics (Drone Delivery)
# Urban Logistics: Drone Delivery System

# Team Members:
Guy Mayer: https://github.com/guymayer1/drone  
David Moreno  https://github.com/dmon67854
Mariana Munoz: https://github.com/Mariana-Munoz-70445 
Nadia Nazeem  
Kevin Behlke: https://github.com/kwb95124


# Project Overview

This project models a drone-based delivery system designed to improve efficiency in last-mile urban logistics. Last-mile delivery refers to the final stage of the supply chain, where goods are transported from a local hub to the customer’s door. Although it is the shortest part of the delivery process, it is often the most complex and expensive due to factors such as traffic congestion, frequent stops, and the need for individualized deliveries in urban environments.

To address these challenges, companies are increasingly exploring solutions such as drone delivery to improve speed and reduce operational costs. Our system focuses on organizing and optimizing drone operations while ensuring safety and reliability through structured data management.


# System Focus and Objectives 

The primary focus of this system is:
- Fleet maintenance  
- Route optimization  

Our goal is to design a system that keeps drone delivery operations efficient, safe, and scalable by enforcing key constraints such as drone status validation, maintenance tracking, and charging station limitations.

# Business Rules

The system enforces the following rules:

1. Drones must have a valid status (In-Flight, Charging, Maintenance) before being assigned to a trip.  
2. Maintenance logs must be created when a drone exceeds 50 flight hours.  
3. Charging stations can only serve one drone at a time, with start and end times recorded.


# Data Model
<img width="1270" height="801" alt="Screenshot 2026-04-02 215114" src="https://github.com/user-attachments/assets/cae8f091-7f6f-4a00-97c3-cb9eb8acadf8" />

The data model illustrates how drones, trips, packages, and supporting entities interact to manage delivery operations and enforce system constraints.

At the center of the system is the Drone entity, which connects operational activity with maintenance and resource management. Each drone can complete many trips, accumulate maintenance logs over time, and participate in charging sessions. This allows the system to track both performance and upkeep, ensuring that drones remain reliable for delivery operations.

A Storage_Facility serves as the operational hub, housing both drones and technicians. One facility can support many drones and many technicians, creating a structured environment for both deployment and maintenance. Technicians generate maintenance logs for drones, allowing the system to monitor servicing and enforce maintenance requirements based on flight hours.

On the delivery side, the system connects warehouses, routes, trips, and packages to represent how goods move through the network. A warehouse can store many packages and support multiple routes. Each route can be used for many trips, and each trip is assigned to one drone. Packages are linked to trips through the Trip_Packages table, which represents a many-to-many relationship. This allows a single trip to carry multiple packages while maintaining flexibility in delivery assignments.

Shipping destinations are connected to packages to represent delivery endpoints, while routes ensure that trips follow structured and optimized paths. Together, these relationships allow the system to efficiently coordinate deliveries across an urban environment.

The model also supports key operational constraints. Charging stations and charging sessions track when and where drones are charging, ensuring that only one drone occupies a station at a given time. In addition, maintenance logs track drone usage and enforce servicing once flight-hour thresholds are exceeded. These constraints help maintain safety and prevent operational conflicts.


# Data Dictionary

# Data Dictionary
[Mist 4610 data dict.pdf](https://github.com/user-attachments/files/26470808/Mist.4610.data.dict.pdf)
<img width="846" height="927" alt="image" src="https://github.com/user-attachments/assets/3232a2d6-6118-489d-b2f4-c679b631c3e4" />
<img width="845" height="929" alt="image" src="https://github.com/user-attachments/assets/7f350c78-993f-4760-b760-274e2067823f" />
<img width="809" height="882" alt="image" src="https://github.com/user-attachments/assets/9625a715-4f4a-4bad-a760-15ca047c8301" />
<img width="734" height="935" alt="image" src="https://github.com/user-attachments/assets/0f927258-f264-491e-ad57-b7d7d8625ad3" />
<img width="734" height="911" alt="image" src="https://github.com/user-attachments/assets/814b5fc8-cd16-4d5d-9ab4-07ad5c40ab7a" />



# Simple Queries:

## Query for drone models in the Athens area that is AeroX Pro

### Managerial Explanation: This query helps management identify which deliveries in the Athens area are being handled by a specific drone model. This is useful for tracking how certain models are being deployed in different locations, evaluating model performance in a target service area, and supporting decisions about where particular drone types should be used most often.

#### SELECT shipping_destination.destinationAddress, Drone.model FROM Trip JOIN Drone ON Trip.Drone_droneID = Drone.droneID JOIN shipping_destination  ON shipping_destination.Trip_TripID = Trip.TripID WHERE destinationAddress LIKE '%Athens%' and model = "AeroX Pro"; 

<img width="630" height="126" alt="image" src="https://github.com/user-attachments/assets/04d96ebe-dd93-42c3-b390-49c87ac37051" />

## Drone and status 

### Managerial Explanation: This query gives managers a quick snapshot of the current condition of the fleet by showing each drone’s status, battery level, and flight hours. It helps support day-to-day operational decisions by showing which drones may be available, which may need attention soon, and which are approaching maintenance thresholds.

#### SELECT Drone.droneID, Drone.status, Drone.battery, Drone.flightHours FROM Drone;
<img width="394" height="259" alt="image" src="https://github.com/user-attachments/assets/01714170-e013-42b6-8f09-1390ac0ec179" />

## Drones needing maintenance (> 50 hours)
### Managerial Explanation: This query helps managers identify drones that have exceeded the maintenance threshold and may require servicing. It is useful for preventative maintenance planning, reducing the risk of equipment failure, and making sure the fleet remains safe and reliable during delivery operations.

#### SELECT Drone.droneID, Drone.flightHours, Drone.status FROM Drone WHERE Drone.flightHours > 50;

<img width="570" height="269" alt="image" src="https://github.com/user-attachments/assets/841a03f3-8bc1-4e49-8d5a-32c36262bd78" />

## Packages assigned to trips
### Managerial Explanation: This query helps managers understand how packages are being distributed across trips. It is useful for monitoring package loads, verifying that delivery assignments are being made correctly, and evaluating whether trips are being used efficiently to carry multiple deliveries at once.

#### SELECT Trip_Packages.Trip_TripID, Packages.packageID,  Packages.packageType, Packages.weight, Packages.packageContent FROM Trip_Packages JOIN Packages ON Trip_Packages.Packages_packageID = Packages.packageID;

<img width="656" height="475" alt="image" src="https://github.com/user-attachments/assets/5be83f28-4f71-4607-9b51-bf8fd4d75a40" />

## Total distance per drone
### Managerial Explanation: This query allows managers to measure how heavily each drone is being used by showing the total distance it has traveled. It helps with workload balancing, identifying high-use drones, planning maintenance more effectively, and evaluating whether fleet usage is being distributed efficiently.

#### SELECT Trip.Drone_droneID, SUM(Trip.tripDistance) AS total_distance_flown FROM Trip GROUP BY Trip.Drone_droneID;

<img width="413" height="230" alt="image" src="https://github.com/user-attachments/assets/e0bba2ff-25ff-47ae-9134-513ae0c88e36" />

## Packages with destination
### Managerial Explanation: This query helps managers track what packages are going to which delivery destinations. It is useful for monitoring delivery patterns, confirming routing accuracy, and understanding customer demand by location. Over time, this can support better route planning and service allocation.

#### SELECT packageID, packageType, weight, destinationAddress FROM Packages JOIN shipping_destination ON Packages.shipping_destination_destinationID = shipping_destination.destinationID;

<img width="580" height="335" alt="image" src="https://github.com/user-attachments/assets/57b459ef-9da2-475f-a3ba-7f28711d1aa6" />

# Complex Queries 

## Use this to see which drones that are in flight are low on battery (and need to find a charging station soon)
### Managerial Explanation: This query helps managers quickly evaluate which active drones are in strong condition and which may need charging soon. Instead of only showing a battery number, it translates battery levels into more practical categories, making it easier to prioritize operational decisions and respond quickly when a drone may need to leave service for charging.

#### SELECT *, CASE 	WHEN battery > 67 THEN "Good to go"       WHEN battery > 33 THEN "Monitor Battery" WHEN battery <= 32 THEN "Charge Soon" END AS BatteryLevel FROM Drone WHERE status = "In-Flight";

<img width="872" height="102" alt="image" src="https://github.com/user-attachments/assets/64bd169e-1daa-423f-b661-4983b993598a" />

## This query lists the drone ID, status, facility address, average speed in miles per minute, and current battery level for faster than average drones with a sufficient battery level. 
### Managerial Explanation: This query is to be used in emergencies, such as same-day-delivery orders or medical emergencies than might need a certain medicine delivered ASAP. It shows managers all the relevant information they might need to decide what drone to use to make those urgent deliveries, such as their battery level and where they're located at the moment. Managers can choose the fastest drone closest to the warehouse where the package is stored to make the delivery.

#### SELECT Drone_droneID, Drone.status, facilityAddress, AVG(tripDistance/tripLength) AS avg_drone_mpm, battery
FROM Drone
JOIN storage_facility ON facilityID = `Storage Facility_facilityID`
JOIN Trip ON Drone_droneID = droneID
WHERE battery > 70
GROUP BY Drone_droneID, Drone.status, facilityAddress
HAVING avg_drone_mpm < (SELECT AVG(tripDistance/tripLength) FROM Trip) ORDER BY avg_drone_mpm DESC;

<img width="803" height="94" alt="image" src="https://github.com/user-attachments/assets/2e8f0c55-e9c0-4117-9a03-25c958359c5a" />

## This query calculates how many packages each warehouse has handled and the total weight of those packages, then ranks the warehouses from highest to lowest activity, for warehouses that have handled at least one package.
### Managerial Explanation: This query helps management evaluate warehouse activity by showing how many packages each warehouse has handled and the total weight processed. It is useful for comparing warehouse workload, identifying high-volume locations, and supporting decisions related to staffing, resource allocation, and operational efficiency.

#### SELECT Warehouse.warehouseID, COUNT(Packages.packageID) AS TotalPackagesHandled, SUM(Packages.weight) AS TotalWeightHandled FROM Warehouse JOIN Packages ON Warehouse.warehouseID = Packages.Warehouse_warehouseID GROUP BY Warehouse.warehouseID HAVING COUNT(Packages.packageID) > 0 ORDER BY TotalPackagesHandled DESC;

<img width="553" height="175" alt="image" src="https://github.com/user-attachments/assets/855c610f-49e0-45a0-a2a6-58c596e3f4c4" />

## SUB QUERY: Drones with above average flight hours 
### Managerial Explanation: This query helps managers identify drones that are being used more heavily than the rest of the fleet. It is useful for spotting overutilized assets, determining which drones may need closer monitoring, and making better decisions about balancing usage across the fleet.

#### SELECT Drone.droneID, Drone.flightHours FROM Drone WHERE Drone.flightHours > ( SELECT AVG(Drone.flightHours) FROM Drone);

<img width="250" height="148" alt="image" src="https://github.com/user-attachments/assets/f4282411-8081-460a-9203-cf4e81eecc9f" />

REPLACE: ## EXISTS: Find technicians who have performed at least one maintenance job
### Managerial Explanation:This query helps managers confirm which technicians are actively contributing to maintenance operations. It can be useful for workforce evaluation, staffing reviews, and understanding which personnel are involved in maintaining fleet readiness.

#### SELECT Technicians.technicianID, Technicians.techniciansFName, Technicians.techniciansLName FROM Technicians WHERE EXISTS (    SELECT * FROM maintenance_logs  WHERE maintenance_logs.Technicians_technicianID = Technicians.technicianID );

<img width="430" height="225" alt="image" src="https://github.com/user-attachments/assets/b667a269-68d2-406b-aff1-9cbe3b62db0c" />

## Find drones that have at least one trip over 10 miles
### Managerial Explanation: This query helps managers identify drones that are being used for longer-distance deliveries. This is useful for evaluating route demands, determining which drones are handling more intensive assignments, and assessing whether long-distance trips are being assigned appropriately based on drone capability and battery constraints.

#### SELECT Drone.droneID FROM Drone WHERE EXISTS (    SELECT *     FROM Trip    WHERE Trip.Drone_droneID = Drone.droneID  AND Trip.tripDistance > 10 );

<img width="174" height="286" alt="image" src="https://github.com/user-attachments/assets/736f6748-ae2f-4028-bd26-715ad4d4d793" />

# Database
Name of the database: ns_Sp26_71552_Group4 Each query that is listed above is marked in the database using stored procedures which can be called using this format: CALL TP_Q1();










Mist 4610
Data Dictionary 

Below you’ll find a description of what should be in each column of the data dictionary. An example of a data dictionary follows.



Charging_Sessions

Column Names
Description
Data Type
Size
Format
Key
sessionID
Unique identifier for each charging session
INT




PK
Charging_stations_stationID
Station where charging occurs
INT




FK
Drone_droneID
Drone being charged
INT




FK
startTime
Start time of charging session
DATETIME


YYYY-MM-DD HH:MM:SS


endTime
End time of charging session
DATETIME


YYYY-MM-DD HH:MM:SS




Charging_Stations 

Column Names
Description
Data Type
Size
Format
Key
stationID
Unique identifier for charging station
INT




PK
stationAddress
Address of the charging station
VARCHAR
45




status
Current status (available, in use, etc.)
VARCHAR
45




Trip_TripID
Trip associated with station usage
INT




FK










Drone

Column Names
Description
Data Type
Size
Format
Key
droneID
Unique identifier for each drone
INT




PK
lastMaintenance
Last maintenance date
DATETIME


YYYY-MM-DD HH:MM:SS


model
Model of the drone
VARCHAR
45 




flightHours
Total hours flown
INT






battery
Battery level of the drone
INT






Storage_Facility_facilityID
Facility where drone is stored
INT




FK
status
Current drone status (In-Flight, Charging, Maintenance)
VARCHAR
12






Maintenance Logs

Column Names
Description
Data Type
Size
Format
Key
maintenanceLogID
Unique identifier for maintenance record
INT




PK
Drone_droneID
Drone being maintained
INT




FK
Technicians_technicianID
Technician performing maintenance
INT




FK
maintenanceType
Type of maintenance performed
VARCHAR
45 




maintenanceCost
Cost of maintenance
DECIMAL
(3,2)




maintenanceDate
Date of maintenance
DATETIME


YYYY-MM-DD HH:MM:SS












Packages 

Column Names
Description
Data Type
Size
Format
Key
packageID
Unique identifier for each package
INT




PK 
packageType
Type of package
VARCHAR
45




weight
Weight of package
DECIMAL
(3,2)




packageContent
Contents of the package
VARCHAR
45




Shipping_Destination_destinationID
Destination of package
INT




FK
Warehouse_warehouseID
Warehouse storing package
INT




FK




Route

Column Names
Description
Data Type
Size
Format
Key
routeID
Unique identifier for the flight path
INT




PK
areacode
Unique identifier for the flight path
VARCHAR
45




streetAddress
The origin point of the route
VARCHAR
45




Warehouse_warehouseID
The warehouse that manages this route
INT




FK











Shipping Destination
Column Names
Description
Data Type
Size
Format
Key
destinationID 
Unique identifier for the delivery address
INT




PK
destinationAddress 
Full street address for the drop off 
VARCHAR
45




lastDelivery
Timestamp of the most recent arrival
DATETIME


YYYY-MM-DD HH:MM:SS


nextDelivery
Scheduled timestamp for the upcoming arrival 
DATETIME


YYYY-MM-DD HH:MM:SS


routeID
 Identifier for the route assigned to this destination
VARCHAR
45




Trip_TripID
Links the destination to a specific active trip
INT




FK


Storage Facility
Column Names
Description
Data Type
Size
Format
Key
facilityID
Unique identifier for the facility
INT




PK
facilityAddress
Physical location address
VARCHAR
45




facilityName
Name of the facility
VARCHAR
45




status
Operational state
VARCHAR
12






Technicians
Column Names
Description
Data Type
Size
Format
Key
technicianID
 Unique identifier for each technician
INT




PK
techniciansFName
First name of the technician 
VARCHAR
45




techniciansLName
Last name of the technician 
VARCHAR
45




Storage Facility_facilityID
References the facility where the technician is based
INT




FK


Trip
Column Names
Description
Data Type
Size
Format
Key
tripID
Unique identifier for the trip
INT




PK
tripStart
Departure Time
DATETIME


YYYY-MM-DD HH:MM:SS


tripEnd
Arrival Time
DATETIME


YYYY-MM-DD HH:MM:SS


tripLength
Duration of the flight
INT






tripDistance
Total distance travel
DECIMAL
(6,2) 




Drone_droneID
The drone assigned to the trip
INT




FK
Route_routeID
The planned path taken for the trip
INT




FK


Trip_Packages
Column Names
Description
Data Type
Size
Format
Key
Trip_TripID    
Link to the specific trip
INT




 PK, FK
Packages_packageID
Link to the specific package


INT




PK, FK




Warehouse
Column Names
Description
Data Type
Size
Format
Key
warehouseID
Unique identifier for the warehouse 
INT




PK
warehouseAddress
Physical location of the warehouse
VARCHAR 
45




status
Current operational status 
VARCHAR
12




Operating Hours 
The timeframe the warehouse is open for business
VARCHAR
45






