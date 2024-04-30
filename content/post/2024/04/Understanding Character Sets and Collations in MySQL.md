---
title: "Understanding Character Sets and Collations in MySQL"
date: 2024-04-30T11:27:57+08:00
draft: false
---

If you have ever worked with [MySQL](https://severalnines.com/product/clustercontrol/for_mysql), you inevitably came across character sets and collations. In this blog post, we will try to give you a more in-depth look at what those two are and how you should use them.

What Are Character Sets and Collations?
---------------------------------------

Simply put, character sets in [MySQL](https://severalnines.com/product/clustercontrol/for_mysql) are sets of symbols and encodings -- collations are sets of rules for comparing characters in a character set. In other words, character sets are sets of characters that are legal in a string, while collations are a set of rules used to compare characters in a particular character set. Just how each character set has a default collation, character sets can also have several collations. [MySQL](https://severalnines.com/product/clustercontrol/for_mysql) has a default character set and collation for the server and for each database and table too.

Character Sets in MySQL
-----------------------

In general, character sets in [MySQL](https://severalnines.com/product/clustercontrol/for_mysql) work like so:

-   When a database is created, character sets are derived from the server-wide character_set_server variable.
-   When a table is created, character sets are derived from the database.
-   When a column is created, character sets are derived from the table.

As far as character sets are concerned, there are a few variables you should keep an eye on:

-   Character_set_client defines the character set in which statements are sent by the client.
-   Character_set_connection defines the character set that statements are translated into after a server receives a statement from the client.
-   Character_set_results defines the character set in which the server returns query results to the client.

These three settings can be changed by using the SET NAMES or the SET CHARACTER SET statements, or even in the [MySQL](https://severalnines.com/product/clustercontrol/for_mysql) configuration files.

When dealing with character sets sometimes you might also encounter an error #1267:

```
ERROR 1267 (HY000): Illegal mix of collations.
```

The above error is generally caused by comparing two strings that have incompatible collations or by attempting to select data that has a different collation into a combined column. The error is shown because when [MySQL](https://severalnines.com/product/clustercontrol/for_mysql) compares two values with different character sets, it must convert them to the same character set for the comparison, but the character sets are not compatible. To solve this problem, ensure that the collations of each table and their columns are the same.

Collations in MySQL
-------------------

As already mentioned above, collations are closely related to character sets because a collation is a set of rules that defines how to compare and sort character strings. Each character set has at least one collation, some also have more.

While we will not go into the nitty gritty details of all of the things collation related in [MySQL](https://severalnines.com/product/clustercontrol/for_mysql) in this blog post, there are some things you should know:

-   If you're using MySQL 5.7, the default MySQL collation is generally latin1_swedish_ci because MySQL uses latin1 as its default character set. If you're using MySQL 8.0, the default charset is utf8mb4.
-   If you elect to use UTF-8 as your collation, always use utf8mb4 (specifically utf8mb4_unicode_ci). You should not use UTF-8 because MySQL's UTF-8 is different from proper UTF-8 encoding. This is the case because it doesn't offer full unicode support which can lead to data loss or security issues. Keep in mind that utf8mb4_general_ci is a simplified set of sorting rules which takes shortcuts designed to improve speed while utf8mb4_unicode_ci sorts accurately in a wide range of languages. In general, utf8mb4 is the "safest" character set as it also supports 4-byte unicode while utf8 only supports up to 3.

Choosing a Good Character Set and Collation
-------------------------------------------

To choose a good collation and character set for your [MySQL](https://severalnines.com/product/clustercontrol/for_mysql) data set, remember to keep it simple. A mixture of different character sets and (or) collations can be a real mess since they can be very confusing (for example, everything might work fine until certain characters appear, etc.) so it's best to evaluate your needs upfront and choose the best collation and character set upfront. MySQL also has a few valuable queries that can help you do just that, for example, 

```
SELECT * FROM information_schema.CHARACTER_SETS ORDER BY CHARACTER_SET_NAME;
```

would return a list of character sets and available collations together with their description which can be extremely useful if you're planning out your database design.

Do keep in mind that some character sets might require more CPU operations, also they might consume more storage space. Using wrong character sets can even defeat indexing -- for example, MySQL has to convert character sets so that it can compare them when they are not the same: the conversion might make it impossible to use an index.

Also, keep in mind that some people recommend "to just use UTF-8 globally" -- this might not necessarily be a great idea because many applications do not even need UTF-8 at all and, depending on your data, UTF-8 can cause more trouble than it's worth (for example, it might use much more storage space on the disk), so choose wisely.

Summary
-------

Character sets and collations can be your friends or one of your nightmares -- it all depends on how you use them. In general, keep in mind that a "good" character set and collation depend on the data your database holds -- [MySQL](https://severalnines.com/product/clustercontrol/for_mysql) does provide some queries to help you decide what to use, but for your character sets and collations to be effective you should also think about when it makes sense to use a certain collation and why.

转载自[severalnines](https://severalnines.com/blog/understanding-character-sets-and-collations-mysql/#:~:text=If%20you're%20using%20MySQL,use%20utf8mb4%20(specifically%20utf8mb4_unicode_ci).)
