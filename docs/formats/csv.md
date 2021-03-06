---
layout: default
title: CSV Format
---

# {{ page.title }}

The CSV format is a comma separated list.  It is the easiest output to use, if you want to consume the results in Excel.

## Format

CSV output will always start with the first row returning the column names.  Then each additional row will return
a result from the query.

    Name,Position,Agency Name,Agency Website,Nomination Date,Confirmation Vote,Confirmed,Holdover
    "Aaron, Henry Jacob",Member,Social Security Advisory Board,SSAB (http://www.ssab.gov),02/14/2011 12:00 AM,,,

Although the format is basically simple, there are a few things to notice:

1. `Aaron, Henry Jacob` has quotes around it.  The reason for this is because it has a `,` in it.  The quotes are needed to distinguish the comma in the value from a comma used to delimit columns.
2.  Empty fields are denoted by no value.  E.g. `,,`
3.  If a value had a quote in it, it needs to be escaped.  This happens by putting quotes around the value and doubling the existing quote.  E.g.  `"String` would be escaped as `"""String"`.
