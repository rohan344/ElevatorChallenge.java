import java.util.*; 
import java.util.Scanner;

public class ElevatorChallenge {

    public enum Direction { 
        UP, 
        DOWN, 
        IDLE 
    } 

    public static class Elevator {
        private static final int MIN_FLOOR = 0; 
        private static final int MAX_FLOOR = 10; 
        private static int processingTime = 500; // ms 

        private int currentFloor; 
        private Direction currentDirection; 
        private Map<Integer, List<Integer>> requestedPathsMap; 
        private Boolean[] currentFloorDestinations; 

        public Elevator() { 
            this.currentFloor = 0; // Start from ground floor
            this.currentDirection = Direction.UP; // Start by going up
            this.requestedPathsMap = new HashMap<>(); 
            this.currentFloorDestinations = new Boolean[MAX_FLOOR + 1]; 
            Arrays.fill(this.currentFloorDestinations, Boolean.FALSE); 
        } 

        public void setProcessingTime(int processingTime) { 
            Elevator.processingTime = processingTime; 
        } 

        public int getCurrentFloor() { 
            return this.currentFloor; 
        } 

        public Map<Integer, List<Integer>> getRequestedPathsMap() { 
            return this.requestedPathsMap; 
        } 

        public Boolean[] getCurrentFloorDestinations() { 
            return this.currentFloorDestinations; 
        }

        public void start() throws InterruptedException { 
            currentDirection = Direction.UP; // Assume the elevator starts on the ground floor
            do { 
                System.out.println("--------"); 
                processFloor(currentFloor); 
                System.out.println("--------"); 
            } while (currentDirection != Direction.IDLE); 
            System.out.println("No one is waiting and no one is looking to go anywhere"); 
            System.out.println("Chilling for now"); 
        }

        public void lunchtimeElevatorRush() { 
            Random random = new Random(); 
            for (int i = 0; i < 30; i++) { 
                int start, destination;
                do {
                    start = random.nextInt(11);
                    destination = random.nextInt(11);
                } while (start == destination); // Ensure start and destination are not the same
                callElevator(start, destination); 
            } 
        }

        public void callElevator(int start, int destination) { 
            if (isInvalidFloor(start) || isInvalidFloor(destination) || start == destination) { 
                System.out.println("INVALID FLOORS. Try again"); 
                return; 
            } 
            if (requestedPathsMap.containsKey(start)) // If START is already in the map, add the destination
                requestedPathsMap.get(start).add(destination); 
            else { // Add a new key for START with the list containing the DESTINATION
                requestedPathsMap.put(start, new ArrayList<>() {{ 
                    add(destination); 
                }}); 
            } 
        }

        private void processFloor(int floor) throws InterruptedException { 
            if (currentFloorDestinations[floor]) // Deboarding if any people who reached this destination floor
                System.out.println("UN-BOARDING at Floor : " + floor); 
            if (requestedPathsMap.containsKey(floor)) { // Onboarding people and their destination
                System.out.println("BOARDING at Floor : " + floor); 
                requestedPathsMap.get(floor).forEach(destinationFloor -> 
                    currentFloorDestinations[destinationFloor] = true); // Mark destination for next traversal
                requestedPathsMap.remove(floor); // Remove the entry from map as we have marked all the destinations
            } 
            currentFloorDestinations[floor] = false; // Mark as false since we have just arrived at this floor
            moveElevator(); 
        }

        private void moveElevator() throws InterruptedException { 
            if (!Arrays.asList(currentFloorDestinations).contains(true) && requestedPathsMap.isEmpty()) { 
                currentDirection = Direction.IDLE; // Stop elevator if no more requests
                return; 
            } else if (isInvalidFloor(currentFloor + 1)) { 
                currentDirection = Direction.DOWN; // Switch to down when reaching the top floor
            } else if (isInvalidFloor(currentFloor - 1)) { 
                currentDirection = Direction.UP; // Switch to up when reaching the bottom floor
            }

            switch (currentDirection) { 
                case UP: { 
                    moveUp(); 
                    break; 
                } 
                case DOWN: { 
                    moveDown(); 
                    break; 
                } 
                default: { 
                    System.out.println("Elevator Malfunctioned"); 
                } 
            } 
        }

        private void moveUp() throws InterruptedException { 
            currentFloor++; 
            System.out.println("GOING UP TO " + currentFloor); 
            Thread.sleep(processingTime); 
        } 

        private void moveDown() throws InterruptedException { 
            currentFloor--; 
            System.out.println("GOING DOWN TO " + currentFloor); 
            Thread.sleep(processingTime); 
        } 

        private boolean isInvalidFloor(int floor) { 
            return floor < MIN_FLOOR || floor > MAX_FLOOR; 
        } 
    }

    static void automaticElevator() throws InterruptedException { 
        Elevator elevator = new Elevator(); 
        elevator.lunchtimeElevatorRush(); 
        elevator.start(); 
    } 

    static void manualElevator() throws InterruptedException { 
        Elevator elevator = new Elevator(); 

        Scanner sc = new Scanner(System.in); 
        System.out.println("Enter a starting floor 0 - 10"); 
        int start = sc.nextInt(); 
        System.out.println("Enter a destination floor 0 - 10"); 
        int end = sc.nextInt(); 

        elevator.callElevator(start, end); // Calling the elevator to pick us up 
        elevator.start(); 
    } 

    public static void main(String[] args) throws InterruptedException { 
        Scanner sc = new Scanner(System.in);
        System.out.println("Choose mode: 1 for Manual, 2 for Automatic");
        int choice = sc.nextInt();
        if (choice == 1) {
            manualElevator(); 
        } else if (choice == 2) {
            automaticElevator(); 
        } else {
            System.out.println("Invalid choice");
        }
    } 
}
