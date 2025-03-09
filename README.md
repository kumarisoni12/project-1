# project-1
# Banking System
import java.util.HashMap;
import java.util.Scanner;

class User {
    String accountNumber;
    String name;
    String password;
    double balance;

    public User(String accountNumber, String name, String password) {
        this.accountNumber = accountNumber;
        this.name = name;
        this.password = password;
        this.balance = 0.0;
    }

    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("₹" + amount + " Withdrawn Successfully!");
            return true;
        }
        System.out.println("Insufficient Balance or Invalid Amount!");
        return false;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("₹" + amount + " Deposited Successfully!");
        } else {
            System.out.println("Invalid Deposit Amount!");
        }
    }

    public void transfer(User recipient, double amount) {
        if (withdraw(amount)) {
            recipient.deposit(amount);
            System.out.println("₹" + amount + " Transferred to " + recipient.name);
        }
    }
}

class Bank {
    HashMap<String, User> users = new HashMap<>();
    Scanner scanner = new Scanner(System.in);

    public void registerUser() {
        System.out.print("Name: ");
        String name = scanner.nextLine();
        System.out.print("Account Number: ");
        String accountNumber = scanner.nextLine();
        System.out.print("Password: ");
        String password = scanner.nextLine();

        if (!users.containsKey(accountNumber)) {
            users.put(accountNumber, new User(accountNumber, name, password));
            System.out.println("Account Created Successfully!");
        } else {
            System.out.println("Account Number Already Exists!");
        }
    }

    public User loginUser() {
        System.out.print("Account Number: ");
        String acc = scanner.nextLine();
        System.out.print("Password: ");
        String pass = scanner.nextLine();

        if (users.containsKey(acc) && users.get(acc).password.equals(pass)) {
            return users.get(acc);
        } else {
            System.out.println("Invalid Credentials!");
            return null;
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Bank bank = new Bank();
        Scanner scanner = new Scanner(System.in);
        User user = null;

        while (true) {
            System.out.println("\n1. Register  2. Login  3. Exit");
            System.out.print("Choose: ");
            int choice = scanner.nextInt();
            scanner.nextLine();

            if (choice == 1) {
                bank.registerUser();
            } else if (choice == 2) {
                user = bank.loginUser();
                if (user != null) {
                    while (true) {
                        System.out.println("\n1. Check Balance  2. Deposit  3. Withdraw  4. Transfer  5. Logout");
                        System.out.print("Choose: ");
                        int option = scanner.nextInt();
                        scanner.nextLine();

                        if (option == 5) break;

                        switch (option) {
                            case 1:
                                System.out.println("Balance: ₹" + user.balance);
                                break;
                            case 2:
                                System.out.print("Amount: ");
                                user.deposit(scanner.nextDouble());
                                break;
                            case 3:
                                System.out.print("Amount: ");
                                user.withdraw(scanner.nextDouble());
                                break;
                            case 4:
                                System.out.print("Recipient Account: ");
                                String acc = scanner.next();
                                if (bank.users.containsKey(acc)) {
                                    System.out.print("Amount: ");
                                    user.transfer(bank.users.get(acc), scanner.nextDouble());
                                } else {
                                    System.out.println("Account Not Found!");
                                }
                                break;
                            default:
                                System.out.println("Invalid Option!");
                        }
                    }
                }
            } else if (choice == 3) {
                System.out.println("Thank you for using our bank!");
                break;
            } else {
                System.out.println("Invalid Choice!");
            }
        }
        scanner.close();
    }
}

