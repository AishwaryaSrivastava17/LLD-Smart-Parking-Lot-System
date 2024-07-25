# LLD-Smart-Parking-Lot-System
Low-level architecture for a backend system of a smart parking lot. It handle vehicle entry and exit management, parking space allocation, and fee calculation.

Here's a detailed design addressing these requirements:

# 1. Data Model
Database Schema

We'll use a relational database for this design. The schema will include tables for parking spots, vehicles, and parking transactions.

Table Definitions
<pre>
<b>Table : ParkingSpot</b>

spot_id (integer, primary key)
vehicle_type (varchar)
vehicle_id (integer, nullable) — Foreign Key referencing Vehicle.vehicle_id
is_occupied (boolean)
</pre>
Purpose: Represents each parking spot and its occupancy status.

<pre>
<b>Table : Vehicle</b>

vehicle_id (integer, primary key)
license_plate (varchar, unique)
vehicle_type (varchar)
</pre>
Purpose: Represents each vehicle with a unique identifier, license plate, and type.

<pre>
<b>Table : ParkingTransaction</b>

transaction_id (integer, primary key)
vehicle_id (integer) — Foreign Key referencing Vehicle.vehicle_id
spot_id (integer) — Foreign Key referencing ParkingSpot.spot_id
checkin_at (timestamp)
exited_at (timestamp, nullable)
</pre>
Purpose: Represents each parking transaction, including the check-in and check-out times.

<b><br>Relationships:</b>
<pre>One-to-One: ParkingSpot to Vehicle — Each parking spot is linked to a single vehicle.
One-to-Many: Vehicle to ParkingTransaction — Each vehicle can have multiple transactions.
One-to-Many: ParkingSpot to ParkingTransaction — Each parking spot can have multiple transactions over time.</pre>

# 2. Algorithm for Spot Allocation

Spot Allocation Algorithm:
<pre>
1 - Determine Vehicle Size Requirement: Based on the vehicle_type, determine the required spot size.
2 - Query Available Spots: Query the ParkingSpot table for available spots that match the required size and are not currently occupied. Order the results by spot_number to find the closest available spot.
3 - Assign Spot: Mark the spot as occupied (is_occupied = true), update the vehicle_id in the ParkingSpot table, and create a new entry in the ParkingTransaction table with the checkin_at set to the current timestamp.
4 - Handle No Availability: If no spots are available, return a message indicating that the parking lot is full.
</pre>

# 3. Fee Calculation Logic


Fee Calculation Logic:
<pre>1 - Retrieve Check-In and Check-Out Times: Fetch check_in_at and exit_at from ParkingTransaction.
2 - Calculate Duration: Compute the difference between check_in_time and check_out_time to get the duration in hours.
3 - Calculate Fee: Apply the appropriate rate based on vehicle_type and duration.</pre>

# 4. Real-Time Availability Update

Real-Time Update Mechanism:
<pre>
1 - Spot Status Change: Whenever a vehicle enters or exits, update the is_occupied status and the vehicle_id in the ParkingSpot table.
2 - Notification System: Implement a notification system to push real-time updates to any front-end systems or apps.
</pre>


# Summary
The low-level architecture for the smart parking lot backend system includes:
<pre>
1 - A relational database schema for managing parking spots, vehicles, and transactions.
2 - An efficient spot allocation algorithm based on vehicle size and spot availability.
3 - Fee calculation logic based on vehicle type and parking duration.
4 - Real-time updates and concurrency handling mechanisms to ensure system reliability and responsiveness.
5 - This design aims to provide a robust and efficient solution for managing a smart parking lot in an urban environment. </pre>







