# Atm-interface

import java.util.ArrayList;
import java.util.Scanner;

class User {
    private String userId;
    private String pin;
    private ArrayList<Account> accounts;

    public User(String userId, String pin) {
        this.userId = userId;
        this.pin = pin;
        this.accounts = new ArrayList<>();
    }

    public String getUserId() {
        return userId;
    }

    public String getPin() {
        return pin;
    }

    public ArrayList<Account> getAccounts() {
        return accounts;
    }

    public void addAccount(Account account) {
        accounts.add(account);
    }
}

class Account {
    private String accountNumber;
    private double balance;
    private ArrayList<String> transactionHistory;

    public Account(String accountNumber) {
        this.accountNumber = accountNumber;
        this.balance = 0.0;
        this.transactionHistory = new ArrayList<>();
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        balance += amount;
        transactionHistory.add("Deposit: +" + amount);
    }

    public void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
            transactionHistory.add("Withdraw: -" + amount);
        } else {
            System.out.println("Insufficient funds.");
        }
    }

    public void transfer(Account recipient, double amount) {
        if (balance >= amount) {
            balance -= amount;
            recipient.deposit(amount);
            transactionHistory.add("Transfer: -" + amount + " to " + recipient.getAccountNumber());
        } else {
            System.out.println("Insufficient funds.");
        }
    }

    public ArrayList<String> getTransactionHistory() {
        return transactionHistory;
    }
}

public class ATMSystem {
    public static void main(String[] args) {
        // Create user
        User user = new User("123456", "1234");

        // Create accounts
        Account account1 = new Account("001");
        Account account2 = new Account("002");

        // Add accounts to the user
        user.addAccount(account1);
        user.addAccount(account2);

        // Simulate user authentication
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter User ID: ");
        String userIdInput = scanner.nextLine();
        System.out.print("Enter PIN: ");
        String pinInput = scanner.nextLine();

        if (userIdInput.equals(user.getUserId()) && pinInput.equals(user.getPin())) {
            System.out.println("Authentication successful.");
            
            // Main menu
            while (true) {
                System.out.println("\nMain Menu:");
                System.out.println("1. Withdraw");
                System.out.println("2. Deposit");
                System.out.println("3. Transfer");
                System.out.println("4. Transaction History");
                System.out.println("5. Quit");

                System.out.print("Enter your choice: ");
                int choice = scanner.nextInt();
                
                switch (choice) {
                    case 1:
                        System.out.print("Enter withdrawal amount: ");
                        double withdrawAmount = scanner.nextDouble();
                        account1.withdraw(withdrawAmount);
                        break;
                    case 2:
                        System.out.print("Enter deposit amount: ");
                        double depositAmount = scanner.nextDouble();
                        account1.deposit(depositAmount);
                        break;
                    case 3:
                        System.out.print("Enter recipient's account number: ");
                        String recipientAccountNumber = scanner.next();
                        System.out.print("Enter transfer amount: ");
                        double transferAmount = scanner.nextDouble();
                        
                        // Find the recipient account by account number (simplified for this example)
                        Account recipient = null;
                        for (Account account : user.getAccounts()) {
                            if (account.getAccountNumber().equals(recipientAccountNumber)) {
                                recipient = account;
                                break;
                            }
                        }
                        
                        if (recipient != null) {
                            account1.transfer(recipient, transferAmount);
                        } else {
                            System.out.println("Recipient account not found.");
                        }
                        break;
                    case 4:
                        System.out.println("Transaction History:");
                        for (String transaction : account1.getTransactionHistory()) {
                            System.out.println(transaction);
                        }
                        break;
                    case 5:
                        System.out.println("Exiting...");
                        System.exit(0);
                    default:
                        System.out.println("Invalid choice. Please try again.");
                }
            }
        } else {
            System.out.println("Authentication failed. Please try again.");
        }
    }
}
