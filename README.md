# Banking-system.
class Account:
    def __init__(self, username, password, account_no, address, aadhar, bcode, mobile, bal):
        self.username = username
        self.password = password
        self.account_no = account_no
        self.address = address
        self.aadhar = aadhar
        self.bcode = bcode
        self.mobile = mobile
        self.bal = bal

class Bank:
    def __init__(self):
        self.users = {}
        self.accounts = {}

    def login(self):
        entered_username = input("Enter username: ")
        entered_password = input("Enter password: ")
        if entered_username in self.users and self.users[entered_username]["password"] == entered_password:
            print("Login successful!")
            return entered_username
        else:
            print("Invalid username or password!")
            return None

    def create_account(self):
        username = input("Enter username: ")
        if username in self.users:
            print("Username already exists!")
            return
        password = input("Enter password: ")
        account_no = int(input("Enter account Number: "))
        address = input("Enter address: ")
        aadhar = input("enter the aadhar no: ")
        bcode = input("Enter Branch Code: ")
        mobile = int(input("Enter Mobile Number: "))
        bal = int(input("Enter current Balance: "))
        self.users[username] = {"password": password, "account_no": account_no}
        self.accounts[account_no] = Account(username, password, account_no, address, aadhar, bcode, mobile, bal)
        print("Account created successfully!")

    def show_account_details(self, username):
        account_no = self.users[username]["account_no"]
        account = self.accounts.get(account_no)
        if account:
            print("Account Number:", account.account_no)
            print("Customer Name:", account.username)  # Fixed accessing customer name
            print("Address:", account.address)  # Fixed accessing address
            print("Branch Code:", account.bcode)
            print("Mobile:", account.mobile)
        else:
            print("Account not found.")

    def deposit(self, username, amount):
        account_no = self.users[username]["account_no"]
        self.accounts[account_no].bal += amount
        print("Deposit successful. New balance:", self.accounts[account_no].bal)

    def withdraw(self, username, amount):
        account_no = self.users[username]["account_no"]
        if self.accounts[account_no].bal >= amount:
            self.accounts[account_no].bal -= amount
            print("Withdrawal successful. New balance:", self.accounts[account_no].bal)
        else:
            print("Insufficient balance!")

    def check_balance(self, username):
        account_no = self.users[username]["account_no"]
        print("Current balance:", self.accounts[account_no].bal)

# Main
bank = Bank()
username = None

while True:
    print("1. Create account\n2. Login\n3. Withdraw\n4. Deposit\n5. Check balance\n6. Exit")
    choice = int(input("Select any operation: "))

    if choice == 1:
        bank.create_account()
    elif choice == 2:
        username = bank.login()
        if username:
            print("Login successful!")
    elif choice == 3:
        if username:
            amount = int(input("Enter amount to withdraw: "))
            bank.withdraw(username, amount)
        else:
            print("Please login first.")
    elif choice == 4:
        if username:
            amount = int(input("Enter amount to deposit: "))
            bank.deposit(username, amount)
        else:
            print("Please login first.")
    elif choice == 5:
        if username:
            bank.check_balance(username)
        else:
            print("Please login first.")
    elif choice == 6:
        print("Exiting...")
        break
    else:
        print("Please select one of the available options.")
