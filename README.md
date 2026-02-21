# 🛫 Airline Reservation System

A robust console-based **Airline Reservation System** built with Java, demonstrating core Object-Oriented Programming (OOP) principles and best practices.

## 📋 Table of Contents

- [Features](#features)
- [OOP Concepts](#oop-concepts)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Usage Guide](#usage-guide)
- [Sample Data](#sample-data)
- [Technical Details](#technical-details)
- [About](#about)

## ✨ Features

- ✅ **Add New Flights** - Register flights with passenger capacity
- ✅ **Book Seats** - Reserve multiple seats for passengers
- ✅ **Cancel Tickets** - Cancel bookings using Passport Number
- ✅ **View Flights** - Display all available flights with current occupancy
- ✅ **Passenger Management** - View all passengers on a specific flight
- ✅ **Custom Exception Handling** - Overbooking prevention with custom exceptions
- ✅ **Input Validation** - Secure input handling to prevent invalid data

## 🏗️ OOP Concepts Demonstrated

### 1. **Inheritance**
- `Passenger` class extends `Person` class
- Promotes code reuse and establishes hierarchical relationships

### 2. **Encapsulation**
- Private instance variables with public getter/setter methods
- Protects internal state and controls access to data

### 3. **Polymorphism**
- Method overriding: `displayDetails()` implementation varies across classes
- Allows flexible and extensible code

### 4. **Exception Handling**
- Custom `OverbookingException` for handling booking limit scenarios
- Robust error management and user feedback

## 📁 Project Structure

```
Airline-Reservation-System/
├── Code/                          # Java source files
│   ├── Person.java               # Base class for all persons
│   ├── Passenger.java            # Extends Person, represents passengers
│   ├── Flight.java               # Flight information and seat management
│   ├── AirlineReservationSystem.java  # Core system logic
│   ├── OverbookingException.java  # Custom exception class
│   └── AirlineReservationDemo.java    # Main entry point
├── README.md                      # This file
└── .gitattributes               # Git configuration
```

## 🚀 Getting Started

### Prerequisites
- Java Development Kit (JDK) 8 or higher
- Command-line terminal or IDE (IntelliJ IDEA, Eclipse, VS Code)

### Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/18ayushyadav/Airline-Reservation-System.git
   cd Airline-Reservation-System
   ```

2. **Compile the Java files:**
   ```bash
   javac Code/*.java
   ```

3. **Run the application:**
   ```bash
   java -cp Code AirlineReservationDemo
   ```

## 📖 Usage Guide

The application provides an interactive console menu with the following options:

### Main Operations

1. **Add Flight** - Register a new flight with route and seat capacity
2. **Book Seat(s)** - Reserve seats for a passenger
   - Enter passenger details (name, passport number, age)
   - Specify number of seats required
3. **Cancel Ticket** - Remove booking using passport number
4. **View All Flights** - Display all registered flights with occupancy
5. **View Passengers** - List all passengers on a selected flight
6. **Exit** - Close the application

### Example Workflow
```
1. Start the system
2. Add flights (if not preloaded)
3. Book seats for passengers
4. View flight details to confirm bookings
5. Cancel tickets as needed
```

## 📊 Sample Data

The system comes preloaded with sample flights:

| Flight Code | Route | Capacity | Status |
|---|---|---|---|
| **AI101** | Delhi → New York | 3 seats | Active |
| **BA202** | Mumbai → London | 2 seats | Active |
| **QA303** | Doha → Sydney | 4 seats | Active |

### Pricing
- **Fixed Ticket Price:** ₹5,000 per seat

## 🔧 Technical Details

### Class Architecture

**Person.java** (Base Class)
- `name` - Passenger's full name
- `passportNumber` - Unique passport identifier
- `age` - Passenger age

**Passenger.java** (Derived Class)
- Extends `Person`
- `seatNumber` - Assigned seat number
- `displayDetails()` - Overridden method

**Flight.java**
- `flightCode` - Unique flight identifier
- `source` & `destination` - Route details
- `capacity` - Total available seats
- `passengers` - List of booked passengers
- Seat booking and cancellation logic

**OverbookingException.java**
- Custom exception for booking capacity violations
- Ensures data integrity

### Key Methods

```java
// Flight management
void addFlight(String code, String source, String destination, int capacity)
void bookSeat(Flight flight, Passenger passenger, int numSeats)
void cancelTicket(Flight flight, String passportNumber)
List<Flight> viewAllFlights()
void viewPassengers(Flight flight)

// Validation
boolean validateInput(String input)
void checkOverbooking(int currentPassengers, int capacity, int newBookings)
```

## 🎯 Learning Outcomes

By studying this project, you'll understand:
- How to design class hierarchies using inheritance
- Implementing encapsulation with access modifiers
- Leveraging polymorphism through method overriding
- Creating custom exceptions for specific scenarios
- Building a complete application with user interaction
- Exception handling best practices

## 🐛 Error Handling

The system handles:
- Invalid seat availability (OverbookingException)
- Invalid user inputs
- Non-existent passenger records
- Duplicate flight codes

## 🚀 Possible Enhancements

- Add persistent data storage (Database/File I/O)
- Implement a graphical user interface (GUI)
- Add seat selection preferences (window, aisle, etc.)
- Implement dynamic pricing based on demand
- Add passenger profile management
- Integration with payment gateway
- Email confirmation system

## 👤 About

**Author:** Ayush Yadav  
**Status:** B.Tech Student Project  
**Created:** 2024-2025  
**License:** Open Source

---

## 📬 Feedback & Contributions

Found a bug or want to suggest improvements? Feel free to:
- Open an [issue](https://github.com/18ayushyadav/Airline-Reservation-System/issues)
- Submit a [pull request](https://github.com/18ayushyadav/Airline-Reservation-System/pulls)

---

**⭐ If you found this project helpful, please consider giving it a star!**