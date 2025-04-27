import java.util.HashMap;
import java.util.InputMismatchException;
import java.util.Map;
import java.util.Scanner;

// Custom exception classes
class InvalidAccountException extends Exception {
    public InvalidAccountException(String message) {
        super(message);
    }
}

class NegativeAmountException extends Exception {
    public NegativeAmountException(String message) {
        super(message);
    }
}

class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
}

class Account {
    private String accountNumber;
    private String accountHolder;
    private double balance;

    public Account(String accountNumber, String accountHolder, double initialBalance) throws NegativeAmountException {
        if (initialBalance < 0) {
            throw new NegativeAmountException("Initial balance cannot be negative");
        }
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.balance = initialBalance;
    }

    public void deposit(double amount) throws NegativeAmountException {
        if (amount <= 0) {
            throw new NegativeAmountException("Deposit amount must be positive");
        }
        balance += amount;
    }

    public void withdraw(double amount) throws NegativeAmountException, InsufficientFundsException {
        if (amount <= 0) {
            throw new NegativeAmountException("Withdrawal amount must be positive");
        }
        if (amount > balance) {
            throw new InsufficientFundsException("Insufficient funds for withdrawal");
        }
        balance -= amount;
    }

    public double getBalance() {
        return balance;
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    public String getAccountHolder() {
        return accountHolder;
    }

    @Override
    public String toString() {
        return String.format("Account[%s, Holder: %s, Balance: %.2f]",
                accountNumber, accountHolder, balance);
    }
}

public class BankAccountManagementSystem {
    private static Map<String, Account> accounts = new HashMap<>();
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        boolean running = true;

        while (running) {
            System.out.println("\nOnline Bank Account Management System");
            System.out.println("1. Create Account");
            System.out.println("2. Deposit Money");
            System.out.println("3. Withdraw Money");
            System.out.println("4. View Balance");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");

            try {
                int choice = scanner.nextInt();
                scanner.nextLine(); // consume newline

                switch (choice) {
                    case 1:
                        createAccount();
                        break;
                    case 2:
                        depositMoney();
                        break;
                    case 3:
                        withdrawMoney();
                        break;
                    case 4:
                        viewBalance();
                        break;
                    case 5:
                        running = false;
                        System.out.println("Thank you for using our banking system!");
                        break;
                    default:
                        System.out.println("Invalid choice. Please enter a number between 1 and 5.");
                }
            } catch (InputMismatchException e) {
                System.out.println("Invalid input. Please enter a number.");
                scanner.nextLine(); // clear the invalid input
            } catch (Exception e) {
                System.out.println("An unexpected error occurred: " + e.getMessage());
            }
        }

        scanner.close();
    }

    private static void createAccount() {
        System.out.println("\n--- Create New Account ---");

        try {
            System.out.print("Enter account number: ");
            String accountNumber = scanner.nextLine();

            if (accounts.containsKey(accountNumber)) {
                throw new IllegalArgumentException("Account number already exists");
            }

            System.out.print("Enter account holder name: ");
            String accountHolder = scanner.nextLine();

            System.out.print("Enter initial balance: ");
            double initialBalance = scanner.nextDouble();
            scanner.nextLine(); // consume newline

            Account account = new Account(accountNumber, accountHolder, initialBalance);
            accounts.put(accountNumber, account);

            System.out.println("Account created successfully!");
            System.out.println(account);
        } catch (InputMismatchException e) {
            System.out.println("Invalid input. Please enter a valid number for balance.");
            scanner.nextLine(); // clear the invalid input
        } catch (NegativeAmountException e) {
            System.out.println("Error: " + e.getMessage());
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        } finally {
            System.out.println("Account creation process completed.");
        }
    }

    private static void depositMoney() {
        System.out.println("\n--- Deposit Money ---");

        try {
            System.out.print("Enter account number: ");
            String accountNumber = scanner.nextLine();

            Account account = accounts.get(accountNumber);
            if (account == null) {
                throw new InvalidAccountException("Account not found");
            }

            System.out.print("Enter deposit amount: ");
            double amount = scanner.nextDouble();
            scanner.nextLine(); // consume newline

            account.deposit(amount);

            System.out.printf("Deposit successful. New balance: %.2f%n", account.getBalance());
        } catch (InputMismatchException e) {
            System.out.println("Invalid input. Please enter a valid number for amount.");
            scanner.nextLine(); // clear the invalid input
        } catch (NegativeAmountException | InvalidAccountException e) {
            System.out.println("Error: " + e.getMessage());
        } finally {
            System.out.println("Deposit process completed.");
        }
    }

    private static void withdrawMoney() {
        System.out.println("\n--- Withdraw Money ---");

        try {
            System.out.print("Enter account number: ");
            String accountNumber = scanner.nextLine();

            Account account = accounts.get(accountNumber);
            if (account == null) {
                throw new InvalidAccountException("Account not found");
            }

            System.out.print("Enter withdrawal amount: ");
            double amount = scanner.nextDouble();
            scanner.nextLine(); // consume newline

            account.withdraw(amount);

            System.out.printf("Withdrawal successful. New balance: %.2f%n", account.getBalance());
        } catch (InputMismatchException e) {
            System.out.println("Invalid input. Please enter a valid number for amount.");
            scanner.nextLine(); // clear the invalid input
        } catch (NegativeAmountException | InsufficientFundsException | InvalidAccountException e) {
            System.out.println("Error: " + e.getMessage());
        } finally {
            System.out.println("Withdrawal process completed.");
        }
    }

    private static void viewBalance() {
        System.out.println("\n--- View Account Balance ---");

        try {
            System.out.print("Enter account number: ");
            String accountNumber = scanner.nextLine();

            Account account = accounts.get(accountNumber);
            if (account == null) {
                throw new InvalidAccountException("Account not found");
            }

            System.out.printf("Account Holder: %s%n", account.getAccountHolder());
            System.out.printf("Current Balance: %.2f%n", account.getBalance());
        } catch (InvalidAccountException e) {
            System.out.println("Error: " + e.getMessage());
        } finally {
            System.out.println("Balance inquiry completed.");
        }
    }
}





