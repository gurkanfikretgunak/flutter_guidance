
# Flutter MVVM Guidance

## Introduction

MVVM (Model-View-ViewModel) is a design pattern that organizes the logical structure of software applications. MVVM consists of three main components: Model, View, and ViewModel. In this guidance, we will explore how to implement MVVM architecture in Flutter and provide 10 example use cases.

## 1. What is Model?

The Model represents the data structure and business logic of your application. It typically contains only the data structure and business logic, without any direct connection to the UI. For example, in a to-do app, you can create a "Task" class that holds data like task name, description, and completion status:

```dart
class Task {
  final String name;
  final String description;
  bool isCompleted;

  Task({required this.name, required this.description, this.isCompleted = false});
}
```

## 2. What is View?

The View represents the user interface. It includes visual components, interface design, and user interaction. The View is independent of the ViewModel and doesn't directly communicate with the Model. For example, you can create a Widget using ListView.builder to list tasks as follows:

```dart
class TaskListView extends StatelessWidget {
  final List<Task> tasks;

  TaskListView({required this.tasks});

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: tasks.length,
      itemBuilder: (context, index) {
        return ListTile(
          title: Text(tasks[index].name),
          subtitle: Text(tasks[index].description),
          leading: Checkbox(
            value: tasks[index].isCompleted,
            onChanged: (value) {
              // Notify the ViewModel
            },
          ),
        );
      },
    );
  }
}
```

## 3. What is ViewModel?

The ViewModel acts as an intermediary between the Model and View. It retrieves data from the Model to fulfill the View's needs and processes user interactions from the View to update the Model. The ViewModel should be completely independent of the View, making application logic easy to test and modify. For instance, in the above task list, the ViewModel can be defined as:

```dart
import 'package:flutter/material.dart';

class TaskListViewModel extends ChangeNotifier {
  List<Task> tasks = [
    Task(name: 'Go shopping', description: 'Buy bread and milk', isCompleted: false),
    Task(name: 'Do exercise', description: 'Run for 30 minutes', isCompleted: true),
    Task(name: 'Read a book', description: 'Finish the last book you read', isCompleted: false),
    // Add more tasks here
  ];

  void updateTaskCompletion(Task task, bool isCompleted) {
    task.isCompleted = isCompleted;
    notifyListeners();
  }
}
```

## 4. How to Implement MVVM?

To implement MVVM in Flutter, follow these steps:

a. **Create the Model:**
   Define classes that represent your data structure and business logic.

b. **Create the ViewModel:**
   Build a ViewModel class or functions that provide the connection between the Model and View.

c. **Create the View:**
   Design the user interface using Flutter widgets.

d. **Connect ViewModel and View:**
   Establish the connection between the View and ViewModel using data binding mechanisms.

e. **Services and Repositories:**
   Manage data sources, API calls, and connections between Model and View using services and repositories.

---

# MVVM Use Cases - Banking Application

## 1. View User Account Information

In this use case, we will use MVVM to display user account information.

**Model:**

```dart
class BankAccount {
  final String accountNumber;
  final String accountHolderName;
  double balance;

  BankAccount({required this.accountNumber, required this.accountHolderName, this.balance = 0.0});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class AccountViewModel extends ChangeNotifier {
  BankAccount _account = BankAccount(accountNumber: '123456789', accountHolderName: 'John Doe', balance: 5000.0);

  BankAccount get account => _account;
}
```

**View:**

```dart
class AccountView extends StatelessWidget {
  final AccountViewModel viewModel;

  AccountView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Account Information')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('Account Number: ${viewModel.account.accountNumber}'),
            Text('Account Holder: ${viewModel.account.accountHolderName}'),
            Text('Balance: \$${viewModel.account.balance.toStringAsFixed(2)}'),
          ],
        ),
      ),
    );
  }
}
```

## 2. Perform Money Transfer

In this use case, we will use MVVM to perform money transfers between users.

**Model:**

```dart
class Transaction {
  final String fromAccount;
  final String toAccount;
  double amount;

  Transaction({required this.fromAccount, required this.toAccount, required this.amount});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class TransactionViewModel extends ChangeNotifier {
  List<Transaction> _transactions = [];

  List<Transaction> get transactions => _transactions;

  void performTransaction(Transaction transaction) {
    _transactions.add(transaction);
    notifyListeners();
  }
}
```

**View:**

```dart
class TransactionView extends StatelessWidget {
  final TransactionViewModel viewModel;

  TransactionView({required this.viewModel});

  final TextEditingController _fromAccountController = TextEditingController();
  final TextEditingController _toAccountController = TextEditingController();
  final TextEditingController _amountController = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Perform Transaction')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            TextField(controller: _fromAccountController, decoration: InputDecoration(labelText: 'From Account')),
            TextField(controller: _toAccountController, decoration: InputDecoration(labelText: 'To Account')),
            TextField(controller: _amountController, keyboardType: TextInputType.number, decoration: InputDecoration(labelText: 'Amount')),
            ElevatedButton(
              onPressed: () {
                double amount = double.parse(_amountController.text);
                Transaction transaction = Transaction(
                  fromAccount: _fromAccountController.text,
                  toAccount: _toAccountController.text,
                  amount: amount,
                );
                viewModel.performTransaction(transaction);
                _fromAccountController.clear();
                _toAccountController.clear();
                _amountController.clear();
              },
              child: Text('Transfer'),
            ),
          ],
        ),
      ),
    );
  }
}
```

## 3. View Account Transaction History

In this use case, we will use MVVM to view the account transaction history.

**Model:**

```dart
class AccountHistory {
  final String description;
  double amount;
  DateTime date;

  AccountHistory({required this.description, required this.amount, required this.date});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class AccountHistoryViewModel extends ChangeNotifier {
  List<AccountHistory> _history = [
    AccountHistory(description: 'Deposit', amount: 1000.0, date: DateTime.now().subtract(Duration(days: 5))),
    AccountHistory(description: 'Withdrawal', amount: -500.0, date: DateTime.now().subtract(Duration(days: 3))),
    AccountHistory(description: 'Transfer', amount: -200.0, date: DateTime.now().subtract(Duration(days: 2))),
  ];

  List<AccountHistory> get history => _history;
}
```

**View:**

```dart
class AccountHistoryView extends StatelessWidget {
  final AccountHistoryViewModel viewModel;

  AccountHistoryView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Account History')),
      body: ListView.builder(
        itemCount: viewModel.history.length,
        itemBuilder: (context, index) {
          final historyItem = viewModel.history[index];
          return ListTile(
            leading: historyItem.amount > 0 ? Icon(Icons.arrow_downward, color: Colors.green) : Icon(Icons.arrow_upward, color: Colors.red),
            title: Text(historyItem.description),
            subtitle: Text('\$${historyItem.amount.toStringAsFixed(2)}'),
            trailing: Text('${historyItem.date.day}/${historyItem.date.month}/${historyItem.date.year}'),
          );
        },
      ),
    );
  }
}
```

## 4. Deposit and Withdraw Money

In this use case, we will use MVVM to deposit and withdraw money from the account.

**Model:**

```dart
class DepositWithdrawal {
  double amount;

  DepositWithdrawal({required this.amount});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class DepositWithdrawalViewModel extends ChangeNotifier {
  BankAccount _account = BankAccount(accountNumber: '123456789', accountHolderName: 'John Doe', balance: 5000.0);

  BankAccount get account => _account;

  void performDeposit(DepositWithdrawal deposit) {
    _account.balance += deposit.amount;
    notifyListeners();
  }

  void performWithdrawal(DepositWithdrawal withdrawal) {
    if (_account.balance >= withdrawal.amount) {
      _account.balance -= withdrawal.amount;
      notifyListeners();
    } else {
      // Not enough balance, show error message or take appropriate action
    }
  }
}
```

**View:**

```dart
class DepositWithdrawalView extends StatelessWidget {
  final DepositWithdrawalViewModel viewModel;

  DepositWithdrawalView({required this.viewModel});

  final TextEditingController _amountController = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Deposit or Withdraw')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            TextField(controller: _amountController, keyboardType: TextInputType.number, decoration: InputDecoration(labelText: 'Amount')),
            ElevatedButton(
              onPressed: () {
                double amount = double.parse(_amountController.text);
                DepositWithdrawal deposit = DepositWithdrawal(amount: amount);
                viewModel.performDeposit(deposit);
                _amountController.clear();
              },
              child: Text('Deposit'),
            ),
            ElevatedButton(
              onPressed: () {
                double amount = double.parse(_amountController.text);
                DepositWithdrawal withdrawal = DepositWithdrawal(amount: amount);
                viewModel.performWithdrawal(withdrawal);


                _amountController.clear();
              },
              child: Text('Withdraw'),
            ),
          ],
        ),
      ),
    );
  }
}
```

## 5. Apply for a Loan

In this use case, we will use MVVM to apply for a loan.

**Model:**

```dart
class LoanApplication {
  final String applicantName;
  double amount;
  bool approved;

  LoanApplication({required this.applicantName, required this.amount, this.approved = false});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class LoanApplicationViewModel extends ChangeNotifier {
  List<LoanApplication> _applications = [];

  List<LoanApplication> get applications => _applications;

  void applyForLoan(LoanApplication application) {
    _applications.add(application);
    notifyListeners();
  }
}
```

**View:**

```dart
class LoanApplicationView extends StatelessWidget {
  final LoanApplicationViewModel viewModel;

  LoanApplicationView({required this.viewModel});

  final TextEditingController _nameController = TextEditingController();
  final TextEditingController _amountController = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Apply for Loan')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            TextField(controller: _nameController, decoration: InputDecoration(labelText: 'Applicant Name')),
            TextField(controller: _amountController, keyboardType: TextInputType.number, decoration: InputDecoration(labelText: 'Loan Amount')),
            ElevatedButton(
              onPressed: () {
                String name = _nameController.text;
                double amount = double.parse(_amountController.text);
                LoanApplication application = LoanApplication(applicantName: name, amount: amount);
                viewModel.applyForLoan(application);
                _nameController.clear();
                _amountController.clear();
              },
              child: Text('Apply'),
            ),
          ],
        ),
      ),
    );
  }
}
```

## 6. Approve and Reject Loan Applications

In this use case, we will use MVVM to approve and reject loan applications.

**ViewModel:**

```dart
import 'package:flutter/material.dart';

enum LoanApplicationStatus {
  Pending,
  Approved,
  Rejected,
}

class LoanApprovalViewModel extends ChangeNotifier {
  List<LoanApplication> _applications = [
    LoanApplication(applicantName: 'Jane Doe', amount: 5000.0),
    LoanApplication(applicantName: 'Michael Smith', amount: 8000.0),
  ];

  List<LoanApplication> get applications => _applications;

  void approveApplication(LoanApplication application) {
    application.approved = true;
    notifyListeners();
  }

  void rejectApplication(LoanApplication application) {
    application.approved = false;
    notifyListeners();
  }

  LoanApplicationStatus getApplicationStatus(LoanApplication application) {
    return application.approved ? LoanApplicationStatus.Approved : LoanApplicationStatus.Rejected;
  }
}
```

**View:**

```dart
class LoanApprovalView extends StatelessWidget {
  final LoanApprovalViewModel viewModel;

  LoanApprovalView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Loan Applications')),
      body: ListView.builder(
        itemCount: viewModel.applications.length,
        itemBuilder: (context, index) {
          final application = viewModel.applications[index];
          return ListTile(
            title: Text(application.applicantName),
            subtitle: Text('\$${application.amount.toStringAsFixed(2)}'),
            trailing: application.approved
                ? Icon(Icons.check_circle, color: Colors.green)
                : Icon(Icons.cancel, color: Colors.red),
            onTap: () {
              showDialog(
                context: context,
                builder: (context) => AlertDialog(
                  title: Text('Loan Application Status'),
                  content: Text(viewModel.getApplicationStatus(application).toString().split('.').last),
                  actions: [
                    TextButton(
                      onPressed: () {
                        Navigator.pop(context);
                      },
                      child: Text('Close'),
                    ),
                  ],
                ),
              );
            },
          );
        },
      ),
    );
  }
}
```

## 7. Loan Calculator

In this use case, we will use MVVM to calculate loan EMIs.

**Model:**

```dart
class LoanCalculator {
  double calculateEMI(double principal, double interestRate, int loanTerm) {
    double monthlyInterestRate = interestRate / 12 / 100;
    double monthlyPayment = principal * monthlyInterestRate / (1 - (pow(1 + monthlyInterestRate, -loanTerm)));
    return monthlyPayment;
  }
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class LoanCalculatorViewModel extends ChangeNotifier {
  LoanCalculator _calculator = LoanCalculator();
  double _principal = 0.0;
  double _interestRate = 0.0;
  int _loanTerm = 1;
  double _monthlyPayment = 0.0;

  double get principal => _principal;
  set principal(double value) {
    _principal = value;
    calculateEMI();
  }

  double get interestRate => _interestRate;
  set interestRate(double value) {
    _interestRate = value;
    calculateEMI();
  }

  int get loanTerm => _loanTerm;
  set loanTerm(int value) {
    _loanTerm = value;
    calculateEMI();
  }

  double get monthlyPayment => _monthlyPayment;

  void calculateEMI() {
    _monthlyPayment = _calculator.calculateEMI(_principal, _interestRate, _loanTerm);
    notifyListeners();
  }
}
```

**View:**

```dart
class LoanCalculatorView extends StatelessWidget {
  final LoanCalculatorViewModel viewModel;

  LoanCalculatorView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Loan Calculator')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            TextField(
              keyboardType: TextInputType.number,
              decoration: InputDecoration(labelText: 'Loan Principal'),
              onChanged: (value) => viewModel.principal = double.parse(value),
            ),
            TextField(
              keyboardType: TextInputType.number,
              decoration: InputDecoration(labelText: 'Interest Rate (%)'),
              onChanged: (value) => viewModel.interestRate = double.parse(value),
            ),
            DropdownButton<int>(
              value: viewModel.loanTerm,
              items: [1, 2, 3, 4

, 5].map((term) {
                return DropdownMenuItem<int>(
                  value: term,
                  child: Text('$term years'),
                );
              }).toList(),
              onChanged: (value) => viewModel.loanTerm = value!,
            ),
            SizedBox(height: 16),
            Text('Monthly Payment: \$${viewModel.monthlyPayment.toStringAsFixed(2)}'),
          ],
        ),
      ),
    );
  }
}
```

## 8. Wallet Application

In this use case, we will use MVVM to create a wallet application.

**Model:**

```dart
class WalletTransaction {
  final String transactionType;
  double amount;
  DateTime date;

  WalletTransaction({required this.transactionType, required this.amount, required this.date});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class WalletViewModel extends ChangeNotifier {
  List<WalletTransaction> _transactions = [];

  List<WalletTransaction> get transactions => _transactions;

  void addTransaction(WalletTransaction transaction) {
    _transactions.add(transaction);
    notifyListeners();
  }
}
```

**View:**

```dart
class WalletView extends StatelessWidget {
  final WalletViewModel viewModel;

  WalletView({required this.viewModel});

  final TextEditingController _amountController = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Wallet')),
      body: Column(
        children: [
          Expanded(
            child: ListView.builder(
              itemCount: viewModel.transactions.length,
              itemBuilder: (context, index) {
                final transaction = viewModel.transactions[index];
                return ListTile(
                  title: Text(transaction.transactionType),
                  subtitle: Text('\$${transaction.amount.toStringAsFixed(2)}'),
                  trailing: Text('${transaction.date.day}/${transaction.date.month}/${transaction.date.year}'),
                );
              },
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Row(
              children: [
                Expanded(
                  child: TextField(controller: _amountController, keyboardType: TextInputType.number, decoration: InputDecoration(labelText: 'Amount')),
                ),
                ElevatedButton(
                  onPressed: () {
                    double amount = double.parse(_amountController.text);
                    WalletTransaction transaction = WalletTransaction(
                      transactionType: 'Deposit',
                      amount: amount,
                      date: DateTime.now(),
                    );
                    viewModel.addTransaction(transaction);
                    _amountController.clear();
                  },
                  child: Text('Deposit'),
                ),
                ElevatedButton(
                  onPressed: () {
                    double amount = double.parse(_amountController.text);
                    WalletTransaction transaction = WalletTransaction(
                      transactionType: 'Withdrawal',
                      amount: -amount,
                      date: DateTime.now(),
                    );
                    viewModel.addTransaction(transaction);
                    _amountController.clear();
                  },
                  child: Text('Withdraw'),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}
```

## 9. Filter Account History

In this use case, we will use MVVM to filter account history by date range.

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class AccountHistoryViewModel extends ChangeNotifier {
  List<AccountHistory> _history = [
    AccountHistory(description: 'Deposit', amount: 1000.0, date: DateTime.now().subtract(Duration(days: 5))),
    AccountHistory(description: 'Withdrawal', amount: -500.0, date: DateTime.now().subtract(Duration(days: 3))),
    AccountHistory(description: 'Transfer', amount: -200.0, date: DateTime.now().subtract(Duration(days: 2))),
  ];

  List<AccountHistory> get history => _history;

  List<AccountHistory> getFilteredHistory(DateTime startDate, DateTime endDate) {
    return _history.where((transaction) => transaction.date.isAfter(startDate) && transaction.date.isBefore(endDate)).toList();
  }
}
```

**View:**

```dart
class FilteredAccountHistoryView extends StatefulWidget {
  final AccountHistoryViewModel viewModel;

  FilteredAccountHistoryView({required this.viewModel});

  @override
  _FilteredAccountHistoryViewState createState() => _FilteredAccountHistoryViewState();
}

class _FilteredAccountHistoryViewState extends State<FilteredAccountHistoryView> {
  DateTime _startDate = DateTime.now().subtract(Duration(days: 7));
  DateTime _endDate = DateTime.now();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Filtered Account History')),
      body: Column(
        children: [
          Container(
            color: Colors.grey[200],
            child: Column(
              children: [
                Text('Start Date: ${_startDate.day}/${_startDate.month}/${_startDate.year}'),
                ElevatedButton(
                  onPressed: () {
                    showDatePicker(
                      context: context,
                      initialDate: _startDate,
                      firstDate: DateTime(2000),
                      lastDate: DateTime.now(),
                    ).then((selectedDate) {
                      if (selectedDate != null) {
                        setState(() {
                          _startDate = selectedDate;
                        });
                      }
                    });
                  },
                  child: Text('Select Start Date'),
                ),
                Text('End Date: ${_endDate.day}/${_endDate.month}/${_endDate.year}'),
                ElevatedButton(
                  onPressed: () {
                    showDatePicker(
                      context: context,
                      initialDate: _endDate,
                      firstDate: DateTime(2000),
                      lastDate: DateTime.now(),
                    ).then((selectedDate) {
                      if (selectedDate != null) {
                        setState(() {
                          _endDate = selectedDate;
                        });
                      }
                    });
                  },
                  child: Text('Select End Date'),
                ),
              ],
            ),
          ),
          Expanded(
            child: ListView.builder(
              itemCount: widget.viewModel.getFilteredHistory(_startDate, _endDate).length,
              itemBuilder: (context, index) {
                final historyItem = widget.viewModel.getFilteredHistory(_startDate, _endDate)[index];
                return ListTile(
                  leading: historyItem.amount > 0 ? Icon(Icons.arrow_downward, color: Colors.green) : Icon(Icons.arrow_upward, color: Colors.red),
                  title: Text(historyItem.description),
                  subtitle: Text('\$${historyItem.amount.toStringAsFixed(2)}'),
                  trailing: Text('${historyItem.date.day}/${historyItem.date.month}/${historyItem.date.year}'),
                );
              },
            ),
          ),
        ],
      ),
    );
  }
}
```

## 10. Sort Account History

In this use case, we will use MVVM to sort account history by amount.

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class AccountHistoryViewModel extends ChangeNotifier {
  List<AccountHistory> _history = [
    AccountHistory(description: 'Deposit', amount: 1000.0, date: DateTime.now().subtract(Duration(days: 5))),
    AccountHistory(description: 'Withdrawal', amount: -500.0, date: DateTime.now().subtract(Duration(days: 3))),
    AccountHistory(description: 'Transfer', amount: -200.0, date: DateTime.now().subtract(Duration(days: 2))),
  ];

  List<AccountHistory> get history => _history;

  List<AccountHistory> getSortedHistoryByAmount() {
    return List.from(_history)..sort((a, b) => b.amount.compareTo(a.amount));
  }
}
```

**View:**

```dart
class SortedAccountHistoryView extends StatelessWidget {
  final AccountHistoryViewModel viewModel;

 

 SortedAccountHistoryView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Sorted Account History')),
      body: ListView.builder(
        itemCount: viewModel.getSortedHistoryByAmount().length,
        itemBuilder: (context, index) {
          final historyItem = viewModel.getSortedHistoryByAmount()[index];
          return ListTile(
            leading: historyItem.amount > 0 ? Icon(Icons.arrow_downward, color: Colors.green) : Icon(Icons.arrow_upward, color: Colors.red),
            title: Text(historyItem.description),
            subtitle: Text('\$${historyItem.amount.toStringAsFixed(2)}'),
            trailing: Text('${historyItem.date.day}/${historyItem.date.month}/${historyItem.date.year}'),
          );
        },
      ),
    );
  }
}
```

---
