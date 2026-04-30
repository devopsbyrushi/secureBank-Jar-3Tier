# 🏦 SecureBank — JAR Edition

> **Trainer:** Rushi | **Course:** DevOps Multi-Cloud Training
> **Build:** Standalone JAR with embedded Tomcat
> **Package:** `com.rushi.securebank`

---

## 🎯 What This Is

A complete **3-tier banking application** built with Spring Boot + MySQL + Thymeleaf,
packaged as a standalone JAR with an embedded Tomcat server.

The UI is bank-grade (ICICI-inspired design), with:
- 🏛️ Bank landing page with login form
- 📝 Customer registration (auto-generates account number like `SB202600042`)
- 💵 Deposit / Withdraw / Transfer operations
- 📊 Transaction history with filters

---

## 📊 Database Schema

| Database | Table | Description |
|---|---|---|
| **`securebank_db`** | `customers` | Customer accounts (was `account` in trainer's example) |
| **`securebank_db`** | `transaction_history` | All banking transactions (was `transaction`) |

> 💡 **Why `transaction_history` instead of `transaction`?**
> `TRANSACTION` is a reserved SQL keyword — using a clearer name avoids issues
> with raw SQL queries and makes the intent obvious.

---

## ⚡ Quick Start

### Prerequisites
- ✅ Java 17 (JDK)
- ✅ Maven 3.6+
- ✅ MySQL 8 running on localhost:3306

### Step 1: Set Up MySQL Database

Run the schema setup script:

```bash
sudo mysql -u root -p < src/main/resources/db/schema.sql
```

Or run manually inside MySQL:
```sql
CREATE DATABASE securebank_db;
CREATE USER 'securebank_user'@'localhost' IDENTIFIED BY 'SecurePass@2026';
GRANT ALL PRIVILEGES ON securebank_db.* TO 'securebank_user'@'localhost';
FLUSH PRIVILEGES;
```

> 💡 If you want to use different credentials, edit
> `src/main/resources/application.properties`.

### Step 2: Build the JAR

```bash
cd SecureBank-JAR
mvn clean package
```

Output: `target/securebank.jar`

> ⚠️ If tests fail: `mvn clean package -DskipTests=true`

### Step 3: Run the JAR

#### Option A: Foreground (Ctrl+C to stop)
```bash
java -jar target/securebank.jar
```

#### Option B: Background (recommended for demo)
```bash
nohup java -jar target/securebank.jar > app.log 2>&1 &
echo $! > app.pid
echo "✅ Started (PID $(cat app.pid))"
```

### Step 4: Open in Browser

```
http://<your-server-ip>:8080
```

---

## 🛑 Stop the App

Foreground: `Ctrl + C`

Background:
```bash
kill $(cat app.pid)
rm app.pid
# Or force-kill anything on port 8080
sudo kill -9 $(sudo lsof -t -i:8080)
```

---

## 📂 Project Structure

```
SecureBank-JAR/
├── pom.xml                                  ← <packaging>jar</packaging>
├── README.md
└── src/
    ├── main/
    │   ├── java/com/rushi/securebank/
    │   │   ├── SecureBankApplication.java          (main class)
    │   │   ├── config/SecurityConfig.java
    │   │   ├── controller/SecureBankController.java
    │   │   ├── model/
    │   │   │   ├── Customer.java                   (was Account)
    │   │   │   └── TransactionHistory.java         (was Transaction)
    │   │   ├── repository/
    │   │   │   ├── CustomerRepository.java
    │   │   │   └── TransactionHistoryRepository.java
    │   │   └── service/CustomerService.java
    │   └── resources/
    │       ├── application.properties              (MySQL config)
    │       ├── db/schema.sql                       (database setup)
    │       └── templates/
    │           ├── login.html                      (bank landing + login)
    │           ├── register.html
    │           ├── dashboard.html
    │           └── transactions.html
    └── test/
        └── java/com/rushi/securebank/
            └── SecureBankApplicationTests.java
```

---

## 🎓 Demo Flow for Class

1. **Show the source:** Walk through `Customer.java`, `TransactionHistory.java`, point to schema
2. **Build:** `mvn clean package` → point out `target/securebank.jar`
3. **Run:** `java -jar target/securebank.jar` → no Tomcat install needed!
4. **Browser:** Bank landing page loads at `http://server:8080`
5. **Register:** Create user → Hibernate auto-creates `customers` table
6. **Login → Dashboard:** Real account number shown (e.g. SB202600042), balance ₹0.00
7. **Deposit/Withdraw/Transfer:** Demo all operations
8. **Transactions:** Show filtered history with descriptions

---

## 🔧 Common Issues

| Problem | Fix |
|---|---|
| `Port 8080 already in use` | `sudo kill -9 $(sudo lsof -t -i:8080)` |
| `Communications link failure` | MySQL not running — `sudo systemctl start mysql` |
| `Access denied for user 'securebank_user'` | Re-run schema.sql or check credentials |
| `Unknown database 'securebank_db'` | Run schema.sql first |
| `mvn: command not found` | `sudo apt install maven` |
| Build fails on tests | `mvn clean package -DskipTests=true` |

---

🛠️ **Banking Application by Rushi** · DevOps Multi-Cloud Training · For Educational Use Only
