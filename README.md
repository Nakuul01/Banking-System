# Banking-System
A Python-based banking system enabling users to create accounts with detailed personal information, securely log in, deposit and withdraw funds, check balances, view account summaries and transaction histories. Features include password hashing, unique account IDs, and basic password reset functionality, all via a user-friendly CLI.
Basic Features
Account Creation:

Collects personal information (first name, last name, password, age, Aadhar, mobile number, email).
Supports multiple account types (Savings, Checking).
Assigns a unique Account ID.
User Authentication:

Login: Users log in using Aadhar number and password.
Password Hashing: Secures passwords using SHA-256 hashing.
Basic Banking Operations:

Deposit: Add money to the account.
Withdraw: Deduct money if sufficient balance is available.
Check Balance: View current account balance.
Account Management:

Account Summary: View account details and balance.
Transaction History: View logged transactions with timestamps.
Password Management:

Password Reset: Reset password using Aadhar number.
Interactive Command Line Interface:

Menu-Driven: Text-based menu for user interaction.
Sub-menu: Additional options available for logged-in users.
Transaction Logging
Time-stamped Transactions: Logs deposits and withdrawals with timestamps.
