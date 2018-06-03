# 3. Relational Model and Normalization

A **relation** is a special case of a table. All relations are tables, but not all tables are relations.

Characteristics of Relations:

* Rows contain data about an entity.
* Columns contain data about attributes of the entities.
* All entries in a column are of the same kind. \(**domain integrity constraint**\)
* Each column has a unique name.
* Cells of the table hold a single value.
* The order of the columns/rows is unimportant.
* No two rows may be identical.

{% hint style="info" %}
Some SQL statements do produce tables with duplicate rows. Such row duplication occurs only as a result of SQL manipulation. Tables that you design to be stored in the database should never contain duplicate rows.
{% endhint %}

3 sets of terms are used to describe relation structure:

* \(relation, attribute, tuple\)
* \(table, column, row\)
* \(file, field, record\)

## Functional Dependency

In general, a functional dependency exists when the value of one or more attributes **determines** the value of other attribute\(s\).

If each value of A will be paired with one and only one value of B, then we can say that B is **functionally dependent** upon A and written as `A->B` .

For example, we can say that EmployeeName is functionally dependent on Employee\#, written as:`Employee# -> EmployeeName`where employee\# is called the **determinant**.

The only reason for having relations is to store instances of dependencies. If B can be calculated by algorithm from A, there is no need to have a relation to store.

The determinant of a functional dependency can consist of more than one attribute like `(A, B) -> C`. In this case, the determinant is called a **composite determinant**.

No single skill is more important for designing databases than the ability to identify functional dependencies. Sample data can be incomplete, so the best strategies are to think about the nature of the business activity from which the data arise and to ask the users.

## Keys

In general, a **key** is a combination of one or more columns that is used to identify particular rows in a relation. Keys that have two or more columns are called **composite keys**.

A **candidate key** is a determinant that determines all of the other columns in a relation. Candidate keys identify a _unique_ row in a relation.

When designing a database, one of the candidate keys is selected to be the **primary key**. A table has only one primary key. The primary key can have one column, or it can be a composite.

A primary key, must have unique data values inserted into every row of the table. This is named the **entity integrity constraint**.

A **surrogate key** is an artificial column that is added to a table to serve as the primary key.

