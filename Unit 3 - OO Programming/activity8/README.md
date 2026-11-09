# Activity 8: Polymorphism & Dynamic Method Dispatch

## Objective
Override methods in account subclasses to implement specialized withdrawal business rules using runtime polymorphism.

---

## Target File to Complete
- `src/com/gdb/tests/TestAccountSubclasses.java`

The domain classes in `src/com/gdb/domain` are provided complete. Steps 1-3 explain the overridden `withdraw()` methods they contain; Step 4 is the test code you write.

---

## Plain English Step-by-Step Instructions

### Step 1: Override Withdrawal in `SavingsAccount`
1. Override the `withdraw` method.
2. Validate PIN, account status, and positive amount (the same checks as `Account.withdraw`).
3. Check if the withdrawal would leave the balance below `minBalance`.
4. If it violates minimum balance, throw `MinimumBalanceViolationException`; otherwise deduct the amount.

### Step 2: Override Withdrawal in `CurrentAccount`
1. Override the `withdraw` method.
2. Validate PIN, account status, and positive amount.
3. Check if the requested amount exceeds the balance plus the allowed `overdraftLimit`.
4. If exceeded, throw `InsufficientBalanceException`.
5. If allowed, deduct the amount (allowing balance to become negative down to `-overdraftLimit`).

### Step 3: Override Withdrawal in `FixedDepositAccount`
1. Override the `withdraw` method.
2. Throw an `AccountException` indicating that premature withdrawals are not permitted on Fixed Deposit accounts before maturity.

### Step 4: Write the Polymorphism Tests
In `TestAccountSubclasses.java`, declare every account with the parent type `Account` so the overridden `withdraw()` is chosen at runtime:
1. `SavingsAccount` (balance 10000.0, minBalance 1000.0): withdraw 9500.0, catch `MinimumBalanceViolationException`, print `[PASS]`.
2. `CurrentAccount` (balance 5000.0, overdraftLimit 25000.0): withdraw 10000.0, verify the balance becomes -5000.0, print `[PASS]`.
3. Same account: withdraw 30000.0, catch `InsufficientBalanceException`, print `[PASS]`.
4. `FixedDepositAccount`: attempt any withdrawal, catch `AccountException`, print `[PASS]`.

---

## How to Compile & Run (Multi-OS Guide)

### Windows (PowerShell)
```powershell
# Create bin folder if not exists
if (!(Test-Path bin)) { New-Item -ItemType Directory -Path bin }

# Compile all source files
javac -d bin (Get-ChildItem -Recurse -Filter *.java src | ForEach-Object { $_.FullName })

# Run the test program
java -cp bin com.gdb.tests.TestAccountSubclasses
```

### Windows (Command Prompt - CMD)
```cmd
if not exist bin mkdir bin
javac -d bin src\com\gdb\domain\*.java src\com\gdb\tests\*.java src\com\gdb\exceptions\*.java
java -cp bin com.gdb.tests.TestAccountSubclasses
```

### Linux & macOS (Terminal / Bash / Zsh)
```bash
# Create bin directory
mkdir -p bin

# Compile all Java files
find src -name "*.java" -print0 | xargs -0 javac -d bin

# Run the test program
java -cp bin com.gdb.tests.TestAccountSubclasses
```

---

## Expected Output
```
=== Activity 8: Polymorphism Test ===
[Savings] Withdraw 9500 (breaches min balance 1000): Caught MinimumBalanceViolationException [PASS]
[Current] Withdraw with Overdraft (Balance goes to -5000): SUCCESS [PASS]
[Current] Withdraw exceeding Overdraft (exceeds -25000): Caught InsufficientBalanceException [PASS]
[FixedDeposit] Withdraw attempt: Caught AccountException [PASS]
All polymorphic behaviors verified!
```
