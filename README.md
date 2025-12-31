# Banking-Transaction-Risk-and-Customer-Analysis
Banking analytics project using SQL, Python, and Power BI to analyze customer transaction 
create database bank_risk;
create table bank_analysis (
    TransactionID CHAR(50),
    AccountID CHAR(50),
    TransactionAmount DOUBLE,
    TransactionDate DATETIME,
    TransactionType CHAR(20),
    Location CHAR(100),
    DeviceID CHAR(50),
    IP_Address CHAR(50),
    MerchantID CHAR(50),
    Channel CHAR(20),
    CustomerAge INT,
    CustomerOccupation CHAR(100),
    TransactionDuration INT,
    LoginAttempts INT,
    AccountBalance DOUBLE,
    PreviousTransactionDate DATETIME
);
truncate table bank_analysis;
select count(*) from bank_analysis;
-- data cleaning check missing values --
SELECT COUNT(*) FROM bank_analysis
WHERE TransactionAmount IS NULL
OR TransactionDate IS NULL
OR AccountID IS NULL;
SELECT * FROM bank_analysis
WHERE AccountBalance < 0;
-- high transaction amounts--
SELECT * FROM bank_analysis
WHERE TransactionAmount > 1000;
-- multiple login attempts--
SELECT * FROM bank_analysis
WHERE LoginAttempts > 3;
-- frequent transacdtion in short duration--
SELECT AccountID, COUNT(*) AS TransactionCount
FROM bank_analysis
GROUP BY AccountID
HAVING TransactionCount > 10;
--- suspecious transaction --
SELECT *
FROM bank_analysis
WHERE
    (TransactionAmount > 0.9 * AccountBalance)
    OR (LoginAttempts > 3 AND TransactionDuration < 60);
-- Age distribution --
SELECT CustomerAge, COUNT(*) AS Count
FROM bank_analysis
GROUP BY CustomerAge
ORDER BY CustomerAge;
-- occupation analysis--
SELECT CustomerOccupation, COUNT(*) AS Count
FROM bank_analysis
GROUP BY CustomerOccupation;
-- channel usage--
SELECT Channel, COUNT(*) AS Count
FROM bank_analysis
GROUP BY Channel;
