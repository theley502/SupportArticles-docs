---
title: Error in Receivables Management Inquiry window
description: Provides a solution to an error that may occur when you look up some customers in the Receivables Management Inquiry window.
ms.reviewer: theley
ms.topic: troubleshooting
ms.date: 2/4/2025
ms.custom: sap:Financial - Receivables Management
---
# "Violation of PRIMARY KEY constraint 'PK1003255' attempt to insert a duplicate key" error message in Receivables Management Inquiry window in Microsoft Dynamics GP

This article provides a solution to an error that may occur when you look up some customers in the Receivables Management Inquiry window.

_Applies to:_ &nbsp; Microsoft Dynamics GP  
_Original KB number:_ &nbsp; 864730

## Symptoms

When you look up some customers in the Receivables Management Inquiry window, you may receive the following error message:

> Violation of PRIMARY KEY constraint 'pk1003255' attempt to insert a duplicate key.

## Cause

There is a duplicate record in Receivables Management violating a primary key on the table. The same record is in Work and Open, Open and History, or Work and History tables.

## Resolution

To solve this issue, follow these steps:

1. Identify duplicate records

    Identify duplicate records in the Receivables Management tables yourself. The tables are:

    - RM10301 - RM Sales Work File
    - RM10201 - Cash Receipts Work File
    - RM20101 - RM Open File
    - RM30101 - RM History File

Below are some example scripts that may work to help you find duplicates in the above tables.

/* Script to find duplicates between Work,Open, and History tables in RM */ 

 

PRINT 'Duplicates between RM open(RM20101) and RM sales work(RM10301)' 

Print '' 

select a.DOCNUMBR,a.CUSTNMBR,a.RMDTYPAL from RM20101 a, RM10301 b where 

a.DOCNUMBR = b.DOCNUMBR and  

a.RMDTYPAL = b.RMDTYPAL and  

a.CUSTNMBR = b.CUSTNMBR 

 

go 

 

PRINT 'Duplicates between RM open(RM20101) and RM cash receipts work(RM10201)' 

Print '' 

select a.DOCNUMBR,a.CUSTNMBR,a.RMDTYPAL from RM20101 a, RM10201 b where 

a.DOCNUMBR = b.DOCNUMBR and  

a.RMDTYPAL = b.RMDTYPAL and  

a.CUSTNMBR = b.CUSTNMBR 

go 

 

PRINT 'Duplicates between RM open(RM20101) and RM history(RM30101)' 

Print '' 

select a.DOCNUMBR,a.CUSTNMBR,a.RMDTYPAL from RM20101 a, RM30101 b where 

a.DOCNUMBR = b.DOCNUMBR and  

a.RMDTYPAL = b.RMDTYPAL and  

a.CUSTNMBR = b.CUSTNMBR 

go 

 

PRINT 'Duplicates between RM history(RM30101) and RM sales work(RM10301)' 

Print '' 

select a.DOCNUMBR,a.CUSTNMBR,a.RMDTYPAL from RM30101 a, RM10301 b where 

a.DOCNUMBR = b.DOCNUMBR and  

a.RMDTYPAL = b.RMDTYPAL and  

a.CUSTNMBR = b.CUSTNMBR 

go 

 

PRINT 'Duplicates between RM history(RM30101) and RM cash receipts work(RM10201)' 

Print '' 

select a.DOCNUMBR,a.CUSTNMBR,a.RMDTYPAL from RM30101 a, RM10201 b where 

(a.DOCNUMBR = b.DOCNUMBR and  

a.RMDTYPAL = b.RMDTYPAL and  

a.CUSTNMBR = b.CUSTNMBR) 

2. Delete duplicate records

    Once you have determined which records are duplicates, research them to determine which record is valid and manually delete the duplicated record using SQL Server Management Studio. 
