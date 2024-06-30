import hashlib
import datetime
import uuid
import json
import re
from num2words import num2words

# Helper function to hash passwords
def hash_password(password):
    return hashlib.sha256(password.encode()).hexdigest()

# Helper function to get current timestamp
def current_time():
    return datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")

# Function to send email notifications (dummy implementation)
def send_email(email, subject, message):
    print(f"Email sent to {email} with subject: '{subject}' and message: '{message}'")

# Function to validate password strength
def validate_password(password):
    if (len(password) > 10 and
        re.search(r"[A-Z]", password) and
        re.search(r"[a-z]", password) and
        re.search(r"\d", password) and
        re.search(r"[!@#$%^&*(),.?\":{}|<>]", password)):
        return True
    else:
        print("Password must be more than 10 characters long, "
              "contain at least one uppercase letter, one lowercase letter, "
              "one digit, and one special character.")
        return False

# Function to validate mobile number
def validate_mobile(mobile):
    if re.match(r"^\d{10}$", mobile):
        return True
    else:
        print("Mobile number must be exactly 10 digits.")
        return False

# Function to validate Aadhar number
def validate_aadhar(aadhar):
    if re.match(r"^\d{12}$", aadhar):
        return True
    else:
        print("Aadhar number must be exactly 12 digits.")
        return False

# Function to convert amount to words
def amount_in_words(amount):
    return num2words(amount, lang='en_IN').capitalize() + " rupees only"

class BankAccount:
    def __init__(self, first_name, last_name, password, age, aadhar, mobile, email, account_type='Savings'):
        self.first_name = first_name
        self.last_name = last_name
        self.password = hash_password(password)
        self.age = age
        self.aadhar = aadhar
        self.mobile = mobile
        self.email = email
        self.account_type = account_type
        self.balance = 0.0
        self.transactions = []
        self.account_id = uuid.uuid4().hex  # Unique account ID
    
    def deposit(self, amount):
        self.balance += amount
        self.transactions.append(f"{current_time()} - Deposit: ₹{amount}")
        send_email(self.email, "Deposit Alert", f"₹{amount} deposited successfully. New balance: ₹{self.balance}")
        print(f"₹{amount} deposited successfully. New balance: ₹{self.balance}")
        print(f"Amount in words: {amount_in_words(amount)}")
    
    def withdraw(self, amount):
        if self.balance >= amount:
            self.balance -= amount
            self.transactions.append(f"{current_time()} - Withdrawal: ₹{amount}")
            send_email(self.email, "Withdrawal Alert", f"₹{amount} withdrawn successfully. New balance: ₹{self.balance}")
            print(f"₹{amount} withdrawn successfully. New balance: ₹{self.balance}")
            print(f"Amount in words: {amount_in_words(amount)}")
        else:
            print("Insufficient funds!")
    
    def get_balance(self):
        return self.balance
    
    def get_transaction_history(self):
        return "\n".join(self.transactions) if self.transactions else "No transactions yet."
    
    def account_summary(self):
        return (f"Account Summary:\n"
                f"Name: {self.first_name} {self.last_name}\n"
                f"Account Type: {self.account_type}\n"
                f"Age: {self.age}\n"
                f"Aadhar: {self.aadhar}\n"
                f"Mobile: {self.mobile}\n"
                f"Email: {self.email}\n"
                f"Balance: ₹{self.balance}\n"
                f"Balance in words: {amount_in_words(self.balance)}")

    def apply_interest(self, rate):
        interest = self.balance * rate / 100
        self.deposit(interest)
        self.transactions.append(f"{current_time()} - Interest: ₹{interest}")
        print(f"Interest of ₹{interest} applied. New balance: ₹{self.balance}")
        print(f"Interest amount in words: {amount_in_words(interest)}")

class BankingSystem:
    def __init__(self, storage_file='bank_accounts.json'):
        self.accounts = {}
        self.storage_file = storage_file
        self.load_accounts()
    
    def save_accounts(self):
        with open(self.storage_file, 'w') as file:
            json.dump({aadhar: account.__dict__ for aadhar, account in self.accounts.items()}, file)
    
    def load_accounts(self):
        try:
            with open(self.storage_file, 'r') as file:
                accounts_data = json.load(file)
                for aadhar, data in accounts_data.items():
                    if 'balance' in data and 'transactions' in data:
                        account = BankAccount(
                            first_name=data['first_name'],
                            last_name=data['last_name'],
                            password=data['password'],
                            age=data['age'],
                            aadhar=aadhar,
                            mobile=data['mobile'],
                            email=data['email'],
                            account_type=data.get('account_type', 'Savings')
                        )
                        account.balance = data['balance']
                        account.transactions = data['transactions']
                        self.accounts[aadhar] = account
                    else:
                        print(f"Invalid data format for account with Aadhar: {aadhar}. Skipping...")
        except FileNotFoundError:
            pass
    
    def create_account(self, first_name, last_name, password, age, aadhar, mobile, email, account_type='Savings'):
        if not validate_password(password):
            return
        if not validate_mobile(mobile):
            return
        if not validate_aadhar(aadhar):
            return
        if aadhar in self.accounts:
            print("Account with this Aadhar already exists!")
            return
        account = BankAccount(first_name, last_name, password, age, aadhar, mobile, email, account_type)
        self.accounts[aadhar] = account
        self.save_accounts()
        print("Account created successfully.")
        print(f"Welcome to Nakuul Bank, {first_name}!")
    
    def login(self, aadhar, password):
        account = self.accounts.get(aadhar)
        if account and account.password == hash_password(password):
            print("Login successful.")
            return account
        else:
            print("Invalid Aadhar or Password.")
            return None
    
    def reset_password(self, aadhar, new_password):
        if not validate_password(new_password):
            return
        account = self.accounts.get(aadhar)
        if account:
            account.password = hash_password(new_password)
            self.save_accounts()
            print("Password reset successfully.")
        else:
            print("Account not found.")

def main():
    bank = BankingSystem()
    
    while True:
        print("\nWelcome to Nakuul Bank")
        print("1. Create Account")
        print("2. Login")
        print("3. Reset Password")
        print("4. Exit")
        choice = input("Enter choice: ")

        if choice == '1':
            first_name = input("Enter First Name: ")
            last_name = input("Enter Last Name: ")
            while True:
                password = input("Enter Password: ")
                if validate_password(password):
                    break
            age = input("Enter Age: ")
            while True:
                aadhar = input("Enter Aadhar (12 digits): ")
                if validate_aadhar(aadhar):
                    break
            while True:
                mobile = input("Enter Mobile (10 digits): ")
                if validate_mobile(mobile):
                    break
            email = input("Enter Email: ")
            account_type = input("Enter Account Type (Savings/Checking): ")
            bank.create_account(first_name, last_name, password, age, aadhar, mobile, email, account_type)
        
        elif choice == '2':
            aadhar = input("Enter Aadhar: ")
            password = input("Enter Password: ")
            account = bank.login(aadhar, password)
            
            if account:
                while True:
                    print("\n1. Check Balance")
                    print("2. Deposit Money")
                    print("3. Withdraw Money")
                    print("4. Account Summary")
                    print("5. Transaction History")
                    print("6. Apply Interest")
                    print("7. Logout")
                    sub_choice = input("Enter choice: ")
                    
                    if sub_choice == '1':
                        print(f"Current Balance: ₹{account.get_balance()}")
                        print(f"Balance in words: {amount_in_words(account.get_balance())}")
                    
                    elif sub_choice == '2':
                        amount = float(input("Enter amount to deposit: "))
                        account.deposit(amount)
                        bank.save_accounts()
                    
                    elif sub_choice == '3':
                        amount = float(input("Enter amount to withdraw: "))
                        account.withdraw(amount)
                        bank.save_accounts()
                    
                    elif sub_choice == '4':
                        print(account.account_summary())
                    
                    elif sub_choice == '5':
                        print(account.get_transaction_history())
                    
                    elif sub_choice == '6':
                        rate = float(input("Enter interest rate (in %): "))
                        account.apply_interest(rate)
                        bank.save_accounts()
                    
                    elif sub_choice == '7':
                        break
                    else:
                        print("Invalid choice!")
        
        elif choice == '3':
            aadhar = input("Enter Aadhar: ")
            while True:
                new_password = input("Enter New Password: ")
                if validate_password(new_password):
                    break
            bank.reset_password(aadhar, new_password)
        
        elif choice == '4':
            print("Thank you for using Nakuul Bank.")
            break
        else:
            print("Invalid choice!")

if __name__ == "__main__":
    main()
