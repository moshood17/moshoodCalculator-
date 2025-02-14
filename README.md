import java.util.Scanner;

class MoshoodCalculator {
    public static void main(String[] args) {
        System.out.print("""
                Welcome to moshoodCalculator
                Press Enter to start the calculator: """);
        Scanner input = new Scanner(System.in);
        input.nextLine(); // The user is expected to press Enter to continue
        System.out.println("""
                             COMMAND LIST:
                    Use '+' for addition
                    Use '*' for multiplication
                    Use '-' for subtraction
                    Use '/' for division
                    Use '%' for remainder
                    Use '^' for raise to power (to calculate a raise to power of b; you can use a^(b^-1) to calculate the bth root of A)
                    Use 'sqrt' for square root
                    Press Enter to carry out the calculation
                    Input your calculation, press enter after every variable or operand you input""");

        double previousResult = 0;
        boolean usingPreviousResult = false;
        while (true) {
            double a;
            if (usingPreviousResult) {
                System.out.println("Using previous result: " + previousResult);
                a = previousResult;
            } else {
                a = getValidUserInput(input, "Enter first variable: ");
            }

            String operand;
            while (true) {
                System.out.print("Operand or type Quit to Exit: ");
                operand = input.next();
                if (operand.equalsIgnoreCase("quit") || operand.equalsIgnoreCase("sqrt") ||
                    operand.equals("+") || operand.equals("-") || operand.equals("*") ||
                    operand.equals("/") || operand.equals("%") || operand.equals("^")) {
                    break;
                } else {
                    System.out.println("Invalid operator. Please enter a valid operator.");
                }
            }

            if (operand.equalsIgnoreCase("sqrt")) {
                if (a >= 0) {
                    double result = Math.sqrt(a);
                    System.out.println("Result= " + result);
                    previousResult = result;
                } else {
                    System.out.println("Error: Cannot calculate the square root of a negative number.");
                }
            } else if (operand.equalsIgnoreCase("quit")) {
                break;
            } else {
                double b = getValidUserInput(input, "Second Variable: ");
                String result = switch (operand) {
                    case "+" -> String.valueOf(a + b);
                    case "-" -> String.valueOf(a - b);
                    case "*" -> String.valueOf(a * b);
                    case "/" -> {
                        while (b == 0) {
                            System.out.println("Error: Division by zero. Please enter a non-zero value for the second variable.");
                            b = getValidUserInput(input, "Second Variable: ");
                        }
                        yield String.valueOf(a / b);
                    }
                    case "%" -> {
                        if (b != 0) {
                            yield String.valueOf(a % b);
                        } else {
                            yield "Error: Modulus by zero";
                        }
                    }
                    case "^" -> String.valueOf(Math.pow(a, b));
                    default -> "Error: Invalid operator";
                };

                System.out.println("Result: " + result);
                previousResult = Double.parseDouble(result);
            }

            System.out.print("Press 'p' if you want to use the result of this calculation for the next: ");
            String pResult = input.next().toLowerCase();
            usingPreviousResult = pResult.equals("p");

            if (!usingPreviousResult) {
                System.out.print("Type 'Quit' to exit the calculator or Enter any other key to continue: ");
                input.nextLine();
                String command = input.nextLine();
                if (command.equalsIgnoreCase("Quit")) {
                    break;
                }
            }
        }
        System.out.print("Thank you for using moshoodCalculator");
    }

    private static double getValidUserInput(Scanner input, String promptMessage) {
        double userInput = 0;
        while (true) {
            System.out.print(promptMessage);
            if (input.hasNextDouble()) {
                userInput = input.nextDouble();
                break;
            } else {
                System.out.println("Invalid input. Please enter a valid decimal value.");
                input.next();
            }
        }
        return userInput;
    }
}# moshoodCalculator-
