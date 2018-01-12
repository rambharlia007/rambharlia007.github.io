---
layout: post
title: SQL tips
categories: [MS-Sql-Server]
tags: [MS-Sql-Server,What-Did-I-Learn-Today]
description: Handle SELECT INTO TempTable In IF/ELSE Statement
fullview: true
comments: true.
---

Some times you might want to create a temp table with same name based on different input types.
If you try to use same temp table name in if and else block, sql query fail with error as object with
name already exists.

This happens because sql parses your script and finds that you are attempting to create it in two different places.
The following snippet will not work.

{% highlight sql %}
-- Assume you want to include cities in temp table based on state selected by the user
DECLARE @State NVARCHAR(50) = 'Karnataka'
IF (@State = 'Karnataka')
BEGIN
	SELECT [Name] AS [Place]
	INTO #Places
	FROM Cities
	WHERE typeid = 1
END
ELSE
BEGIN
	SELECT [Name] AS [Place]
	INTO #Places
	FROM Cities
	WHERE typeid = 2
END

{% endhighlight %}

We can solve this problem by first creating the temp table and then inserting the data in it. therefore avoiding
the into clause

{% highlight sql %}
-- Assume you want to include cities in temp table based on state selected by the user
DECLARE @State NVARCHAR(50) = 'Karnataka'
IF OBJECT_ID('tempdb..#Places') IS NOT NULL
DROP TABLE #Places
Create Table #Places(Place nvarchar(100))
IF (@State = 'Karnataka')
BEGIN
    INSERT INTO #Places
	SELECT [Name] AS [Place]
	FROM Cities
	WHERE typeid = 1
END
ELSE
BEGIN
    INSERT INTO #Places
	SELECT [Name] AS [Place]
	FROM Cities
	WHERE typeid = 2
END

{% endhighlight %}
