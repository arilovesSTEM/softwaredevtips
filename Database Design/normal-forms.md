---
tags:
  - database-design
  - normal-forms
  - 1nf
  - 2nf
  - 3nf
  - bcnf
aliases:
  - normal forms
---
# normal forms
## first normal form (1NF)
first normal form is when all of the columns in a database consist of atomic values; they are simple, primitive data types and not arrays or objects.

for example, this table is in 1NF.

|**carReg**|**make**|**model**|**custNo**|**custName**|**hireDate**|**outletNo**|**outletLoc**|
|---|---|---|---|---|---|---|---|
|M565 0GD|Ford|Escort|C100|Smith, J|14/5/98|01|Killarney|
|M565 0GD|Ford|Escort|C201|Hen, P|15/5/98|01|Killarney|
|N734 TPR|Nissan|Sunny|C100|Smith, J|16/5/98|01|Killarney|
|M134 BRP|Ford|Escort|C313|Blatt, O|14/5/98|02|Tralee|
|M134 BRP|Ford|Escort|C100|Smith, J|20/5/98|02|Tralee|
|M611 0PQ|Nissan|Sunny|C295|Pen, T|20/5/98|02|Tralee|

## second normal form (2NF)
second normal form involves the elimination of partial dependencies within tables. in this one table, we have three candidate keys (columns which, when put together, form a unique value on every row), `carReg`, `custNo` and `outletNo`, which make up the whole primary key (you can think of it as a tuple `(carReg, custNo, outletNo)`).

There are other columns here which depend on only parts of this primary key. `custName` relates to `custNo`, but not to `carReg`. `make` and `model` relate to `carReg`, but not to `custNo`. these are all partial dependencies.

**note:** `custName` is only dependant on one third of this tuple.

we can begin to solve this by moving `customer` information into a new table (note: we have left `custNo` as a foreign key so we can still retrieve customer information from the main table):

**unsorted table:**

|**carReg**|**make**|**model**|**custNo**|**hireDate**|**outletNo**|**outletLoc**|
|-|-|-|-|-|-|-|
|M565 0GD|Ford|Escort|C100|14/5/98|01|Killarney|
|M565 0GD|Ford|Escort|C201|15/5/98|01|Killarney|
|N734 TPR|Nissan|Sunny|C100|16/5/98|01|Killarney|
|M134 BRP|Ford|Escort|C313|14/5/98|02|Tralee|
|M134 BRP|Ford|Escort|C100|20/5/98|02|Tralee|
|M611 0PQ|Nissan|Sunny|C295|20/5/98|02|Tralee|

**customers:**

|**custNo**|**custName**|
|---|---|
|C100|Smith, J|
|C201|Hen, P|
|C313|Blatt, O|
|C295|Pen, T|

this is close to what we want, but we still have a partial dependency. we have information about our car (`carReg`, `make`, `model`, `outletNo`, and `outletLoc`) which does not depend on `custNo`. this can also be solved by creating a new table:

**hires:**

|**carReg**|**custNo**|**hireDate**|
|-|-|-|
|M565 0GD|C100|14/5/98|
|M565 0GD|C201|15/5/98|
|N734 TPR|C100|16/5/98|
|M134 BRP|C313|14/5/98|
|M134 BRP|C100|20/5/98|
|M611 0PQ|C295|20/5/98|

**customers (unmodified):**

|**custNo**|**custName**|
|---|---|
|C100|Smith, J|
|C201|Hen, P|
|C313|Blatt, O|
|C295|Pen, T|

**cars:**

|**carReg**|**make**|**model**|**outletNo**|
|-|-|-|-|
|M565 0GD|Ford|Escort|01|
|N734 TPR|Nissan|Sunny|01|
|M134 BRP|Ford|Escort|02|
|M611 0PQ|Nissan|Sunny|02|

now, for each of these three tables, we either have a suitable singular primary key (`custNo` for customers, `carReg` for cars), or a composite primary key with no partial dependencies (hires uses `(carReg, custNo)` as a primary key, but `hireDate` is fully dependant on both columns). this fulfils second form normalisation!

## third normal form (3NF)
third normal form involves the elimination of transitive dependencies within tables. with partial dependencies now removed, there now exists a transitive relationship in the **cars** table.

`carReg` relates to `outletNo`, which relates to `outletLoc`, but `carReg` also directly relates to `outletLoc`, as they are in the same table. ($A \rightarrow B \rightarrow C$ and $A \rightarrow C$)

this can be easily handled by moving `outlet` into its own table:

**hires (unmodified):**

|**carReg**|**custNo**|**hireDate**|
|-|-|-|
|M565 0GD|C100|14/5/98|
|M565 0GD|C201|15/5/98|
|N734 TPR|C100|16/5/98|
|M134 BRP|C313|14/5/98|
|M134 BRP|C100|20/5/98|
|M611 0PQ|C295|20/5/98|

**customers (unmodified):**

|**custNo**|**custName**|
|---|---|
|C100|Smith, J|
|C201|Hen, P|
|C313|Blatt, O|
|C295|Pen, T|

**cars:**

|**carReg**|**make**|**model**|**outletNo**|
|-|-|-|-|
|M565 0GD|Ford|Escort|01|
|N734 TPR|Nissan|Sunny|01|
|M134 BRP|Ford|Escort|02|
|M611 0PQ|Nissan|Sunny|02|

**outlets:**

|**outletNo**|**outletLoc**|
|-|-|
|01|Killarney|
|02|Tralee|

now, `carReg` related to `outletNo`, which relates to `outletLoc`, but `carReg` does not relate to `outletLoc` directly, as it's been moved into its own table. this fulfils third form normalisation!

## boyce-codd normal form (BCNF)
boyce-codd normal form involves the elimination for functional dependency on any attribute that is not a primary key. in the case of `customers`, `cars`, and `outlets`, BCNF is satisfied; they each wholly depend on a unique identifier (`custNo`, `carReg`, and `outletNo` respectively).

however, `hires` does not have any viable primary key. a simple demonstration of this would be one customer hiring the same car on the same day. in this case, we must introduce a new column, `hireNo`:

**hires:**

|**hireNo**|**carReg**|**custNo**|**hireDate**|
|-|-|-|-|
|1|M565 0GD|C100|14/5/98|
|2|M565 0GD|C201|15/5/98|
|3|N734 TPR|C100|16/5/98|
|4|M134 BRP|C313|14/5/98|
|5|M134 BRP|C100|20/5/98|
|6|M611 0PQ|C295|20/5/98|

**customers (unmodified):**

|**custNo**|**custName**|
|---|---|
|C100|Smith, J|
|C201|Hen, P|
|C313|Blatt, O|
|C295|Pen, T|

**cars (unmodified):**

|**carReg**|**make**|**model**|**outletNo**|
|-|-|-|-|
|M565 0GD|Ford|Escort|01|
|N734 TPR|Nissan|Sunny|01|
|M134 BRP|Ford|Escort|02|
|M611 0PQ|Nissan|Sunny|02|

**outlets (unmodified):**

|**outletNo**|**outletLoc**|
|-|-|
|01|Killarney|
|02|Tralee|

now, for each table in the database, there exists one absolute primary key which can uniquely identify any row in the table. this fulfils boyce-codd normalisation!