---
layout: default
title: Queries using SODA2
---

# {{ page.title }} 

SODA2 provides a new rich API for querying against data on the Socrata Platform.  This API borrows a lot from SQL to
provide a generic ability to query data.  This part of the API is aimed towards web applications and mobile applications
for quickly and easily retrieving data.

At the root of the query API is a language called SoQL, however, there are several ways to express the SoQL in a query
to make it easier for different programming languages as uses.

SoQl can then be used on any of the dataset [endpoints](/soda2/endpoints).

## SoQL Clauses

SoQL statements are broken into "Clauses" similar to how SQL statements are.  Each clause can be expressed either directly
as a URL parameter or as a SoQL statement.  The clauses for SoQL are:

|Clause|Query Parameter|Description|
|---|---|---|
|**select**|$select|Which columns to return.  If not specified, all columns will be returned.|
|**where**|$where|Filter on results to return.  If not specified all results will be returned, up to the limit.|
|**order**|$orderBy|Order to return the results.  If not specified, the order will be consistent across paging, but unspecified.|
|**group**|$group|Column to group results on.  This is similar to SQL Grouping.|
|**limit**|$limit|Maximum number of results to return.  If not specified, defaults to 1000.|
|**offset**|$offset|Offset count into the results to start at.  If not specified, defaults to 0.|
|**search**|$q|Search clause.  This wll do a full text search for a value.|

Any clause can be specified either directly in a SoQL statement, or as part of the URI.  For example, a SoQL query would look like:

    select agency_name
    where position='Member'

This can either be directly added to a URL, or added in pieces.  E.g.

    https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$query=select+agency_name+where+position='Member'
    https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$select=agency_name&$where=position='Member'
    https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$select=agency_name&position=Member


### Select Clause

A select clause is similar to a select in SQL.  It allows expressions, aliases and aggregations in the case of using a
group by clause.

#### Listing Columns

The most basic use of a select is simply listing the columns you are returning.  To do this, you can simply list out the columns
you are interested in, separated by a `','`, e.g.

    select agency_name,position


#### Aliasing Columns

In SoQL, you can create column aliases in the Select clause.  There are two main reasons for doing this:

*  Make it easier to access computed values elsewhere in the expression
*  Allows the query to rename the columns for the response

They syntax for aliases is the "as" keyword.  Currently, aliases need to be lower case and cannot have white space.  These
restrictions may be relaxed in the future.

[https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$select=agency_name **as myalias**&position=Member](https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$select=agency_name+as+myalias&position=Member)

#### Column Expressions

In SoQL we provide the ability to create expressions in the select clause as well.  This provides a way to have the reponses converted into more readily usable
values.  Look at the [datatypes](/soda2/datatypes) section to get a list of all the expressions supported per datatype.

[https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$select=**upper(agency_name)**&position=Member](https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$select=upper(agency_name)&position=Member)

#### Groupings Expresssions

In addition to transforms and data conversions on returned values, SoQL provides a set of grouping expressions that can
be used in concert with the **grouping** keyword.  The currently supported grouping expressions are:

|Function|DataTypes|Description|
|---|---|---|
|sum|Number|This sums up all the values in a grouping|
|count|All|This simply counts the number of values.  This will not count null values for this column|
|avg|Number|Finds the average value of numbers in this column.|
|min|Number|Finds the minimum value of numbers in this column.|
|max|Number|Finds the maximum value of numbers in this column.|

An example of how this would work on the familiar nominationsCopy dataset:

[https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$select=agency_name,count(agency_name)&amp;$group=agency_name](https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$select=agency_name,count(agency_name)&$group=agency_name)


#### Names with spaces and capital letters

It is important to note, that the column names in a SoQL query are not always exactly the same as the column names in the UI.
For example, if you go to the [NominationsCopy](https://sandbox.demo.socrata.com/resource/nominationsCopy) dataset, you will see the
name of the `agency_name` column is really "Agency Name".  There is a simple conversion from the user names to the query langauges name:

1.  Lower case the names
2.  replace white space (spaces, tabs, etc) to `_`

So, "Agency Name" becomes `agency_name`.

### Where Clause

The Where clause contains any filtering for the request.  This is how filters are added to the statement.  Where clauses
support a variety of different comparison operations.  See the [datatypes](/soda2/datatypes) section to get the full
list of operations.

In addition to the data type specific operations, the Where clause supports:

|Operator|Description|Example|
|---|---|---|
|AND|The logical **and** of two expressions.|`a AND b` will return true ONLY if `a` and `b` are both true.|
|OR|The logical **or** of two expressions.|`a or b` will return true if either `a` or `b` are true.|
|NOT|The logical **not** of an expression.|`NOT a` will return true, ONLY if a is false.|
|IS NULL|Whether a value is null or not.|`a IS NULL` will return true, ONLY if `a` is null.|
|IS NOT NULL|Whether a values is not null.|`a IS NOT NULL` will return true, ONLY if `a` is not null|
|()|Parentheses are used for defining order of operations.|`b>3 AND (a=1 OR a=2)`|


|Examples|
|---|
|[?$where=**position='Member'**](https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$where=position=%27Member%27)|
|[?$where=position='Member' **AND confirmation_vote IS NULL**](https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$where=position=%27Member%27%20AND%20confirmation_vote%20IS%20NULL)|


### Order Clause

The Order clause determines how the results should be sorted.  This takes on columns, and can sort in either ascending or descending order.

|Examples|
|---|
|[**?$order=nomination_date**](https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$order=nomination_date)|
|[?$order=nomination_date **desc**](https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$order=nomination_date%20desc)|

### Group Clause

The Group clause works similar to how it works in SQL.  It will group results based on the column chosen, allowing the select
clause to use the *Grouping Expressions*.

[https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$select=agency_name,count(agency_name)&amp;$group=agency_name](https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$select=agency_name,count(agency_name)&$group=agency_name)

### Limit + Offset Clauses

Limit and offset are both used for paging results.  **Limit** will determine the maximum number of rows to return.
**Offset** will return the count into the result set to start returning results from.

[https://sandbox.demo.socrata.com/resource/nominationsCopy.json**?$limit=10&$offset=2**](https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$limit=10&$offset=2)

### Search Clause

In addition to traditional database searches, SODA2 allows for full text search as well.

[https://sandbox.demo.socrata.com/resource/nominationsCopy.json**?$q=National**](https://sandbox.demo.socrata.com/resource/nominationsCopy.json?$q=National)

## Returning type information

In many of the returned formats, the original type information of the columns can be lost.  For example, in JSON a Number may look like a String, and a String may
look like a String.  Some consumers may want to operate in a more type safe way, to make sure their language bindings are in line with the original dataset's types.

To aid in these clients, the Socrata API returns two custom headers that allow a caller to determine the original Socrata types.

|Header|Description|
|---|---|
|X-SODA2-Fields|This lists the fields returned in this response in a JSON array.|
|X-SODA2-Types|This is a list of SODA2 types in a JSON array.  These will match up in the same order as the fields in X-SODA2-Fields|

