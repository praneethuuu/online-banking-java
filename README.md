# online-banking-java
@java code for online banking 
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class Bank {
    private static List<Account> accounts = new ArrayList<>();

    public static synchronized void createAccount(Account account) {
        accounts.add(account);
        System.out.println("Account created successfully!");
    }

    public static synchronized void displayAllAccounts() {
        if (accounts.isEmpty()) {
            System.out.println("No accounts found.");
        } else {
            System.out.println("All Accounts:");
            for (Account account : accounts) {
                System.out.println(account);
            }
        }
    }

    public static synchronized void searchByAccountNumber(int accountNumber) {
        boolean found = false;
        for (Account account : accounts) {
            if (account.getAccountNumber() == accountNumber) {
                System.out.println(account);
                found = true;
                break;
            }
        }
        if (!found) {
            System.out.println("Account not found.");
        }
    }

    public static synchronized void depositAmount(int accountNumber, double amount) {
        for (Account account : accounts) {
            if (account.getAccountNumber() == accountNumber) {
                account.deposit(amount);
                System.out.println("Amount deposited successfully!");
                return;
            }
        }
        System.out.println("Account not found.");
    }

    public static synchronized void withdrawAmount(int accountNumber, double amount) {
        for (Account account : accounts) {
            if (account.getAccountNumber() == accountNumber) {
                if (account.getBalance() >= amount) {
                    account.withdraw(amount);
                    System.out.println("Amount withdrawn successfully!");
                } else {
                    System.out.println("Insufficient balance.");
                }
                return;
            }
        }
        System.out.println("Account not found.");
    }
}

class Account {
    private int accountNumber;
    private String accountHolder;
    private double balance;

    public Account(int accountNumber, String accountHolder, double balance) {
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.balance = balance;
    }

    public int getAccountNumber() {
        return accountNumber;
    }

    public String getAccountHolder() {
        return accountHolder;
    }

    public double getBalance() {
        return balance;
    }

    public synchronized void deposit(double amount) {
        balance += amount;
    }

    public synchronized void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
        } else {
            System.out.println("Insufficient balance.");
        }
    }

    @Override
    public String toString() {
        return "Account Number: " + accountNumber +
                "\nAccount Holder: " + accountHolder +
                "\nBalance: " + balance;
    }
}

public class OnlineBankingSystem {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int choice, accountNumber;
        double amount;
        String accountHolder;

        do {
            System.out.println("\n**** Online Banking System ****");
            System.out.println("1. Create Account");
            System.out.println("2. Display All Accounts");
            System.out.println("3. Search by Account Number");
            System.out.println("4. Deposit Amount");
            System.out.println("5. Withdraw Amount");
            System.out.println("6. Exit");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("Enter Account Number: ");
                    accountNumber = scanner.nextInt();
                    scanner.nextLine(); // Consume newline character
                    System.out.print("Enter Account Holder Name: ");
                    accountHolder = scanner.nextLine();
                    System.out.print("Enter Initial Balance: ");
                    double initialBalance = scanner.nextDouble();
                    Account account = new Account(accountNumber, accountHolder, initialBalance);
                    Bank.createAccount(account);
                    break;
                case 2:
                    Bank.displayAllAccounts();
                    break;
                case 3:
                    System.out.print("Enter Account Number: ");
                    accountNumber = scanner.nextInt();
                    Bank.searchByAccountNumber(accountNumber);
                    break;
                case 4:
                    System.out.print("Enter Account Number: ");
                    accountNumber = scanner.nextInt();
                    System.out.print("Enter Amount to Deposit: ");
                    amount = scanner.nextDouble();
                    Bank.depositAmount(accountNumber, amount);
                    break;
                case 5:
                    System.out.print("Enter Account Number: ");
                    accountNumber = scanner.nextInt();
                    System.out.print("Enter Amount to Withdraw: ");
                    amount = scanner.nextDouble();
                    Bank.withdrawAmount(accountNumber, amount);
                    break;
                case 6:
                    System.out.println("Exiting the program. Goodbye!");
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        } while (choice != 6);

        scanner.close();
    }
}

