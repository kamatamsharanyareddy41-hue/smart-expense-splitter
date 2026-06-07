
# 💸 Smart Expense Splitter

An intelligent, full-stack web application designed to simplify group expenses. It tracks shared costs, manages debts, and automatically calculates the most efficient way to settle balances among friends or roommates.

---

## 📝 Introduction

Managing shared expenses within a group—whether for a weekend getaway, shared apartment bills, or a dinner out—often leads to chaotic math, awkward conversations, and a web of complex, redundant bank transfers. 

The **Smart Expense Splitter** solves this problem by acting as a centralized ledger where group members can log expenses in real time. Instead of everyone paying each other back individually, the application processes the group's entire financial footprint and optimizes the payouts. By implementing a transaction-minimization engine, it reduces the total number of peer-to-peer payments, making the entire "settle up" process painless and transparent.

---

## 🎯 Objectives

The primary goals of this project are:
* **Simplify Debt Tracking:** Provide a clear, real-time dashboard showing who paid for what and how much each person owes.
* **Minimize Transactions:** Implement an algorithmic approach to reduce unnecessary intermediate payments (e.g., if A owes B $\$10$ and B owes C $\$10$, A should just pay C $\$10$).
* **Flexible Bill Splitting:** Support multiple splitting methods, including equal splits, exact amounts, percentages, and shares.
* **Promote Accountability:** Maintain a detailed, immutable history of all transactions and settlements to avoid financial disputes.

---

## ⚙️ Procedure & Algorithm

The core logic of the application operates on a structured flow network to balance the books efficiently:

1. **Input Collection:** Users input an expense, specifying the payer, the total amount, and the participants involved (along with their split ratios).
2. **Net Balance Calculation:** The system computes the net balance ($B$) for each user ($i$) using the formula:
   $$B_i = \text{Amount Paid}_i - \text{Share Owed}_i$$
   * A **positive balance** means the user is a **Creditor** (owed money).
   * A **negative balance** means the user is a **Debtor** (owes money).
3. **Categorization:** Users are split into two pools: `Debtors` and `Creditors`, then sorted by the magnitude of their balance.
4. **Greedy Optimization:** The algorithm matches the largest debtor with the largest creditor:
   * A transaction is scheduled between them for the minimum of the two absolute values.
   * The balances are updated, and the process repeats recursively until all balances are zero ($0$).
   * ## ⚙️ How It Works

The **Smart Expense Splitter** operates on a simple, automated workflow that takes the confusion out of group finances. It bridges user inputs with an optimized backend ledger in four simple phases:



## execution 

``` bash
## 💻 Code (Core Optimization Engine)

Below is the core JavaScript algorithm used by the backend to calculate and minimize the group settlements:

```javascript
function minimizeTransactions(participants) {
    // Step 1: Calculate net balances
    let balances = {};
    
    participants.forEach(p => {
        balances[p.name] = p.paid - p.owed;
    });

    // Step 2: Separate into debtors and creditors
    let debtors = [];
    let creditors = [];

    for (let name in balances) {
        if (balances[name] < 0) {
            debtors.push({ name: name, amount: Math.abs(balances[name]) });
        } else if (balances[name] > 0) {
            creditors.push({ name: name, amount: balances[name] });
        }
    }

    // Sort descending to always match largest amounts first
    debtors.sort((a, b) => b.amount - a.amount);
    creditors.sort((a, b) => b.amount - a.amount);

    let settlements = [];

    // Step 4: Greedy matching
    let i = 0, j = 0;
    while (i < debtors.length && j < creditors.length) {
        let debtor = debtors[i];
        let creditor = creditors[j];

        let minAmount = Math.min(debtor.amount, creditor.amount);

        settlements.push({
            from: debtor.name,
            to: creditor.name,
            amount: Number(minAmount.toFixed(2))
        });

        debtor.amount -= minAmount;
        creditor.amount -= minAmount;

        if (debtor.amount === 0) i++;
        if (creditor.amount === 0) j++;
    }

    return settlements;
}

// --- Example Test Case ---
const groupExpenses = [
    { name: "Alice", paid: 100, owed: 40 }, // Balance: +60
    { name: "Bob",   paid: 20,  owed: 40 }, // Balance: -20
    { name: "Charlie", paid: 0, owed: 40 }  // Balance: -40
];

console.log("Optimized Settlements:", minimizeTransactions(groupExpenses));
``
``
output
Alice paid \$100 but her share was only \$40 (Group balance: +60)
Optimized Settlements: [
  { "from": "Charlie", "to": "Alice", "amount": 40 },
  { "from": "Bob", "to": "Alice", "amount": 20 }
]

## author
Sharanya Reddy 
