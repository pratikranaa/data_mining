 
# Installing and Running Postgres

- Install Postgres in Ubuntu

```bash	
sudo apt update
sudo apt install postgresql
```

- Start the postgres server

```bash	
sudo service postgresql start
```

- Connect to the Postgres server 

```bash
sudo -u postgres psql
```
    
# Hands-on: Transactions in Postgres

 Plaksha's TechCash system, which handles payments using Plaksha ID cards,  constructs a table that stores information about accounts, such as the account holder's Plaksha username, their full name, and the total amount of money in their account. You can visualize the table as containing the following information, with 4 rows and 3 columns:

| username  | fullname          | balance |
|-----------|-------------------|---------|
| priya     | Priya Sharma      | 82      |
| ravi      | Ravi Das        | 65      |
| anand     | Anand Patel       | 73      |
| meena     | Meena Srinivas       | 79      |

In this hands-on, you will use the SQL language to issue queries to the database to perform operations on tables. Postgres has a good tutorial on SQL, as well as a more detailed SQL language manual that you can refer to.

## Using SQL

**Exercise 1**. As a first step, use the SQL CREATE command to create the table shown above, and then execute several INSERT commands to insert each of the rows into the resulting table, as follows:

    username=> CREATE TABLE "accounts" ("username" varchar(8), "fullname" varchar(128), "balance" int);
    CREATE TABLE
    username=> INSERT INTO "accounts" VALUES ('priya', 'Priya Sharma', 82);
    INSERT 0 1
    ...

If you make a mistake, you can delete the accounts table by issuing a DROP TABLE accounts; command, and then starting over.

**Exercise 2**. Now, examine the table using the special \d command:

    username=> \d "accounts
                   "Table "public.accounts"
      Column  |          Type          | Modifiers 
    ----------+------------------------+-----------
     username | character varying(8)   | 
     fullname | character varying(128) | 
     balance  | integer                | 

    username=> 

We can now read data from the table using the SELECT command. The SELECT command allows you to choose which rows to select (by specifying a predicate in a WHERE clause), and what data to return from each row (e.g., individual columns or aggregates such as SUM()):

    username=> SELECT "username", "fullname", "balance" from "accounts";
    ...
    username=> SELECT "fullname" from "accounts" where "balance" > 75;
    ...
    username=> SELECT SUM("balance") from "accounts";
    ...
    username=> 

**Exercise 3**. Run the above three commands. Also construct and run a command to display the full name of the person with username `ravi`. Also construct and run a command to display the average account balance of people that have at least $70 in their account. You may want to refer to the Postgres SQL manual to find a suitable function for computing the average.

**Exercise 4**. Transfer $10 from `priya` to `anand`. You will find the UPDATE command useful; consult the Postgres SQL manual for more details. For this exercise, it will suffice to perform two updates: one to deduct $10 from Priya's balance, and another to add $10 to Anand's balance.

## Transactions

To deal with concurrent operations, many SQL databases, including Postgres, support transactions, which allow several SQL statements to execute atomically (often meaning both before-or-after atomicity and all-or-nothing atomicity). To execute statements as a single transaction, SQL clients first issue a `BEGIN` command, then execute some SQL commands, and finally issue either a `COMMIT` command, which makes the transaction's changes permanent, or a `ROLLBACK` command, which reverts the changes from all of the commands in the transaction.

In this assignment, you will simulate concurrent database queries by issuing SQL statements over two different connections to the database. Open up two terminals, and run `sudo -u postgres psql` in each of them, to create two connections to the database. We will use two colors (<span style="color:blue">blue</span> and <span style="color:red">red</span>) to indicate the two database sessions.

Start a transaction in the first (<span style="color:blue">blue</span>) terminal, and display a list of all accounts:

<pre style="background-color: lightblue;">
username=> BEGIN;
BEGIN
username=> SELECT * FROM "accounts";
| username |     fullname     | balance |
|----------|------------------|---------|
|   ravi   |   Ravi Das       |   65    |
|   meena  | Meena Srinivas   |   79    |
|   priya  | Priya Sharma     |   72    |
|   anand  |   Anand Patel    |   83    |
(4 rows)
</pre>

Now, in the second (red) terminal, also start a transaction and add an account for `Karuna`:

<pre style="background-color: #FF7F7F;">
    username=> BEGIN;
    BEGIN
    username=> INSERT INTO "accounts" vALUES ('karuna', 'Karuna Ramaswamy', 55);
    INSERT 0 1
    username=> 
</pre>    

**Exercise 5**. Generate a list of all accounts in the first (<span style="color:blue">blue</span>) terminal, and in the second (<span style="color:red">red</span>) terminal. What output do you get? Are they the same or not? Why?

Now, commit the transaction in the second (<span style="color:red">red</span>) terminal, by issuing the `COMMIT` statement:

<pre style="background-color: #FF7F7F;">
    username=> COMMIT;
    COMMIT
    username=> 
</pre>    

**Exercise 6**. Generate a list of all accounts from the first (<span style="color:blue">blue</span>) terminal again. Does it include `Karuna`? Why or why not?

**Exercise 7**. Commit the transaction in the first (<span style="color:blue">blue</span>) terminal and generate a new list of all accounts:

<pre style="background-color: lightblue;">
    username=> COMMIT;
    COMMIT
    username=> SELECT * FROM "accounts";
    ...
</pre>

What output do you get? Is it different than the output you received in exercise 6? Why or why not?

Now, let's try to modify the same account from two different transactions. In the first (<span style="color:blue">blue</span>) terminal, start a transaction and deposit $5 into `Anand`'s account:

<pre style="background-color: lightblue;">
    username=> BEGIN;
    BEGIN
    username=> UPDATE "accounts" SET "balance"="balance"+5 where "username"='anand';
    UPDATE 1
</pre>
In the second (<span style="color:red">red</span>) terminal, start a transaction and withdraw $10 from `Anand`'s account:

<pre style="background-color: #FF7F7F;">

    username=> BEGIN;
    BEGIN
    username=> UPDATE "accounts" SET "balance"="balance"-10 where "username"='anand';
    UPDATE 1
</pre>

**Exercise 8**. What happens to the execution of the second update? Why?

Let's try aborting a transaction: enter the `ABORT` command in the first (<span style="color:blue">blue</span>) terminal, undoing the $5 deposit:

<pre style="background-color: lightblue;">
    username=> ABORT;
    ROLLBACK
    username=> 
</pre>

**Exercise 9**. What happens to the second (<span style="color:red">red</span>) terminal's transaction?

**Exercise 10**. If you now commit the transaction in the second terminal, what is the resulting balance in `Anand`'s account?

Now let's perform an atomic transfer of $15 from `Ravi` to `Meena`, using two UPDATE statements in a single transaction. In the first (<span style="color:blue">blue</span>) terminal, list the balances of all accounts. Then start a transaction and do a part of a transfer in the second (<span style="color:red">red</span>) terminal:

<pre style="background-color: #FF7F7F;">
    username=> BEGIN;
    BEGIN
    username=> UPDATE "accounts" SET "balance"="balance"-15 where "username"='ravi';
    UPDATE 1
    username=> 
</pre>

**Exercise 11**. If you now look at the list of all account balances in the first (<span style="color:blue">blue</span>) terminal, have the results changed compared to before you started the transaction in the second (<span style="color:red">red</span>) terminal?

**Exercise 12**. Finish the transfer by executing the following commands in the second (<span style="color:red">red</span>) terminal. After each command, list all of the account balances in the first (<span style="color:blue">blue</span>) terminal, to see at what point the effects of the second terminal's transaction become visible. What is that point, and why?

<pre style="background-color: lightblue;">
    username=> UPDATE "accounts" sET "balance"="balance"+15 where "username"='meena';
    UPDATE 1
    username=> COMMIT;
    COMMIT
    username=> 
</pre>

## Transaction isolation levels

Postgres supports different transaction isolation levels. So far, you have been using `READ COMMITTED` transactions, meaning that one transaction can immediately see the results of any other committed transaction. Postgres also has a `SERIALIZABLE` isolation level in which transactions execute in serial order. You can read more about it in the Postgres manual [here](https://www.postgresql.org/docs/current/transaction-iso.html).

**Exercise 13**. By default, Postgres uses the `READ COMMITTED` isolation level. Why do you think Postgres developers chose to make the default isolation level `READ COMMITTED`, which allows non-serializable schedules?

To set the transaction isolation level to `SERIALIZABLE`, you can issue the following command:

    username=> SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL SERIALIZABLE;
    SET
    username=> 
    
Keep in mind that the effects of this command last for the duration of a single session. This means you have to issue it every time you re-start `psql`, and issue it in every session of `psql` that you start, to get `READ COMMITTED` isolation between your transactions.

**Exercise 14. (optional):** Repeat exercises 5-12 with the `SERIALIZABLE` isolation level. What differences do you observe in the behavior of the transactions?