---
layout: post
title:  "Getting the current datetime in XQuery for Transact-SQL"
date:   2014-04-20
description: I had to insert the current datetime in a piece of XML which was stored in a Transact-SQL xml datafield. But the XQuery in Transact-SQL doens’t support ‘fn:current-dateTime()’. So we had to devise another solution.
tags:
 - Transact-SQL
 - XQuery
---
Transact-SQL has a very handy data type: xml. You can use this to store pieces of XML in your database. The database will force this XML to be valid. It’ll make it valid if there is no large issue found or it’ll throw an error:

>XML
{:.filename}
{% highlight xml %}
<Test></Test>
<!-- Will automatically change into -->
<Test />

<Test></Error>
<!-- Will throw an error -->
{% endhighlight %}

You can also perform XQuery statements on xml-fields of a record from your SQL-query. So you can also query into the XML from your query:

>SQL
{:.filename}
{% highlight sql %}
-- Return the text-node value of the test-node as a varchar(20) for each record
SELECT XmlField.value('(/test/text())[1]', 'varchar(20)') AS Test
FROM XMLTestTable
{% endhighlight %}

And recently I had to be able to alter the xml in a record with the current datetime. I had to insert a cancel-date in an empty node that could also not exist yet. I had to refresh my XQuery for starters, but then I didn’t find a direct solution for this. Regular XQuery can use the XPath function ‘[fn:current-dateTime()](http://www.w3.org/TR/xpath-functions/#func-current-dateTime)‘, which will return the current datetime as I required. But T-SQL doens’t support this function… So I cooked up this solution by creating a T-SQL variable and using that variable as text in the XQuery statement.

>SQL
{:.filename}
{% highlight sql %}
-- Declare the variable which will contain the datetime and fill it
DECLARE @DateString varchar(25)
SET @DateString = CONVERT(varchar(25), GETDATE(), 20)

-- Check if the cancel-node already exists
DECLARE @HasCancel bit
SET @HasCancel = (SELECT TOP 1 XMLField.exist('/test/cancel')
                  FROM XMLTestList 
                  WHERE ID = 1)

--When the cancel-node exists, insert the datetime into the textnode. Otherwise insert the cancel-node with the datetime as text
IF @HasCancel = 1
     UPDATE XMLTestList
     SET XMLField.modify('insert text{sql:variable("@DateString")} into (/test/cancel)[1]')
     WHERE ID = 1
ELSE
     UPDATE XMLTestList
     SET XMLField.modify('insert (<cancel>{sql:variable("@DateString")}</cancel>) as last into (/test)[1]')
     WHERE ID = 1
{% endhighlight %}

And like so, I managed to insert the canceldate via the ‘[sql:variable](http://technet.microsoft.com/en-us/library/ms188254.aspx)‘-function. I also found this [page](http://www.allaboutmssql.com/2012/09/xqueryxpathxmlschemaxml-index_6.html) to be quite interesting to get me quickly up to speed on using XQuery in T-SQL.