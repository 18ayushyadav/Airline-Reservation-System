package ARC;
import java.util.Scanner;

// Base class: Person
class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public int getAge() { return age; }

    public void displayDetails() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

// Subclass: Passanger (as requested)
class Passanger extends Person {
    private String passportNumber;

    public Passanger(String name, int age, String passportNumber) {
        super(name, age);
        this.passportNumber = passportNumber;
    }

    public String getPassportNumber() {
        return passportNumber;
    }

    @Override
    public void displayDetails() {  // Polymorphism
        super.displayDetails();
        System.out.println("Passport Number: " + passportNumber);
    }
}

// Custom Exception for Overbooking
class OverbookingException extends Exception {
    public OverbookingException(String message) {
        super(message);
    }
}

// Flight class
class Flight {
    private String flightNumber;
    private String origin;
    private String destination;
    private Passanger[] passangers;
    private int capacity;
    private int count;
    private final int ticketPrice = 5000; // fixed price per seat

    public Flight(String flightNumber, String origin, String destination, int capacity) {
        this.flightNumber = flightNumber;
        this.origin = origin;
        this.destination = destination;
        this.capacity = capacity;
        this.passangers = new Passanger[capacity];
        this.count = 0;
    }

    public String getFlightNumber() { return flightNumber; }
    public String getOrigin() { return origin; }
    public String getDestination() { return destination; }
    public int getTicketPrice() { return ticketPrice; }
    public int getAvailableSeats() { return capacity - count; }

    public void bookTicket(Passanger p) throws OverbookingException {
        if (count >= capacity) {
            throw new OverbookingException("Flight " + flightNumber + " is fully booked.");
        }
        passangers[count++] = p;
        System.out.println("Booked: " + p.getName() + " on flight " + flightNumber);
    }

    public void cancelTicket(String passportNumber) {
        boolean found = false;
        for (int i = 0; i < count; i++) {
            if (passangers[i].getPassportNumber().equalsIgnoreCase(passportNumber)) {
                System.out.println("Ticket cancelled for " + passangers[i].getName() + " on flight " + flightNumber);
                passangers[i] = passangers[count - 1]; // replace with last passanger
                passangers[count - 1] = null;
                count--;
                found = true;
                break;
            }
        }
        if (!found) {
            System.out.println("Passanger with Passport " + passportNumber + " not found on flight " + flightNumber);
        }
    }

    public void showPassengers() {
        System.out.println("\n--- Flight " + flightNumber + " (" + origin + " -> " + destination + ") ---");
        if (count == 0) {
            System.out.println("No passangers booked.");
            return;
        }
        for (int i = 0; i < count; i++) {
            passangers[i].displayDetails();
            System.out.println("----------------");
        }
    }
}

// Airline Reservation System with Multiple Flights
class AirlineReservationSystem {
    private Flight[] flights;
    private int flightCount;

    public AirlineReservationSystem(int maxFlights) {
        flights = new Flight[maxFlights];
        flightCount = 0;
    }

    public void addFlight(String flightNumber, String origin, String destination, int capacity) {
        if (flightCount < flights.length) {
            flights[flightCount++] = new Flight(flightNumber, origin, destination, capacity);
            // intentionally no "Flight added" print as requested
        }
    }

    public Flight getFlight(String flightNumber) {
        for (int i = 0; i < flightCount; i++) {
            if (flights[i].getFlightNumber().equalsIgnoreCase(flightNumber)) {
                return flights[i];
            }
        }
        return null;
    }

    public void showAllFlights() {
        System.out.println("\n--- Available Flights ---");
        for (int i = 0; i < flightCount; i++) {
            Flight f = flights[i];
            System.out.println((i + 1) + ". Flight " + f.getFlightNumber() +
                " (" + f.getOrigin() + " -> " + f.getDestination() + ") - Available Seats: " + f.getAvailableSeats());
        }
    }
}

// Main class
public class AirlineReservationDemo {

    // helper: read integer robustly from scanner
    private static int readInt(Scanner sc, String prompt) {
        while (true) {
            System.out.print(prompt);
            String line = sc.nextLine().trim();
            try {
                return Integer.parseInt(line);
            } catch (NumberFormatException e) {
                System.out.println("Please enter a valid integer.");
            }
        }
    }

    // helper: read non-empty string
    private static String readNonEmptyString(Scanner sc, String prompt) {
        while (true) {
            System.out.print(prompt);
            String s = sc.nextLine().trim();
            if (!s.isEmpty()) return s;
            System.out.println("Input cannot be empty.");
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        AirlineReservationSystem ars = new AirlineReservationSystem(10); // allow up to 10 flights

        // Preload some flights
        ars.addFlight("AI101", "Delhi", "New York", 3);
        ars.addFlight("BA202", "Mumbai", "London", 2);
        ars.addFlight("QA303", "Doha", "Sydney", 4);

        while (true) {
            System.out.println("\n--- Airline Reservation System ---");
            System.out.println("1. Book Ticket(s)");
            System.out.println("2. Cancel Ticket");
            System.out.println("3. Show Passangers of a Flight");
            System.out.println("4. Show All Flights");
            System.out.println("5. Exit");

            int choice = readInt(sc, "Enter choice: ");

            switch (choice) {
                case 1: // Booking one or more passangers
                    ars.showAllFlights();
                    String flightNo = readNonEmptyString(sc, "Enter Flight Number: ");
                    Flight flight = ars.getFlight(flightNo);

                    if (flight == null) {
                        System.out.println("Invalid flight number.");
                        break;
                    }

                    int available = flight.getAvailableSeats();
                    if (available == 0) {
                        System.out.println("Sorry, no seats available on this flight.");
                        break;
                    }

                    int numSeats = readInt(sc, "How many seats do you want to book? (Available: " + available + "): ");
                    if (numSeats <= 0) {
                        System.out.println("No seats booked.");
                        break;
                    }
                    if (numSeats > available) {
                        System.out.println("Only " + available + " seat(s) available. Booking " + available + " seat(s).");
                        numSeats = available;
                    }

                    int bookedCount = 0;
                    for (int i = 0; i < numSeats; i++) {
                        System.out.println("\nEnter details for Passanger " + (i + 1));
                        String name = readNonEmptyString(sc, "Enter Name: ");
                        int age = readInt(sc, "Enter Age: ");
                        String passport = readNonEmptyString(sc, "Enter Passport Number: ");

                        Passanger p = new Passanger(name, age, passport);
                        try {
                            flight.bookTicket(p);
                            bookedCount++;
                        } catch (OverbookingException e) {
                            // shouldn't happen because we checked availability, but handle just in case
                            System.out.println("Error: " + e.getMessage());
                            break;
                        }
                    }

                    if (bookedCount > 0) {
                        int totalPrice = bookedCount * flight.getTicketPrice();
                        System.out.println("\n✅ " + bookedCount + " seat(s) booked. Total Price: Rs " + totalPrice);
                    }
                    break;

                case 2: // Cancellation
                    ars.showAllFlights();
                    String fNo = readNonEmptyString(sc, "Enter Flight Number: ");
                    Flight cancelFlight = ars.getFlight(fNo);

                    if (cancelFlight == null) {
                        System.out.println("Invalid flight number.");
                        break;
                    }

                    String cancelPassport = readNonEmptyString(sc, "Enter Passport Number to cancel: ");
                    cancelFlight.cancelTicket(cancelPassport);
                    break;

                case 3: // Show passangers
                    ars.showAllFlights();
                    String fShow = readNonEmptyString(sc, "Enter Flight Number: ");
                    Flight showFlight = ars.getFlight(fShow);

                    if (showFlight == null) {
                        System.out.println("Invalid flight number.");
                    } else {
                        showFlight.showPassengers();
                    }
                    break;

                case 4: // Show flights
                    ars.showAllFlights();
                    break;

                case 5: // Exit
                    System.out.println("Exiting...");
                    sc.close();
                    System.exit(0);
                    break;

                default:
                    System.out.println("Invalid choice. Try again.");
            }
        }
    }
}
