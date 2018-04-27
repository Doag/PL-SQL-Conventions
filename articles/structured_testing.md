## Structured Testing 
In discussions about test automation one often hears sentences like 'Once the test is created...' There are lots of methods to handle tests systematically once they are created. But for databases the major challenge is to create a test to begin with. 
## Database Concepts for Automatic Testing
One of the strong points of Database Design is the possibility to incorporate many Business Rules in the structure of the database itself. But for testing this is a big disadvantage. To create an order record for testing the Foreign Key relations enforce that you must have a customer and a salesperson, to create an orderline you need product and tax records. And so on.

In real life testing situations the complexity grows exponentionally. To manage this, one needs a concept that can be used to automatically create and delete every needed parent or child record. 

This can be done by reducing the complexity of all possible FK paths to a tree at every point in the database that should be tested. That is possible if there are no cyclical FK paths where al FK relations are mandatory. 
One can structure the whole database into heirarchical levels if this condition is fulfilled.

 1. All tables without mandatory FK relations become level 0
 2. All Tables with only mandatory FK relations to level 0 tables become Level 1
 3. All Tables with only mandatory FK relations to level 0 and 1 tables become Level 2 and so on

Next you must define a range of values that makes it possible to identify any testcase PK.  In my opinion no Table definition is complete, unless it includes both a single column technical PK and a (possibly multi-column) UK that defines how the endusers identify the uniqueness of its records.

With such a single column technical PK it is always possible to define a range that is reserved for automatic tests. My favorite is to simply use negative numbers. One of its advantages is that it makes it very easy to mark result records as coming from testcases too. Simply multiply any generated key number with SIGN( ORIGINAL_ID ) and testresults are as easy to identify as the testcase records themselves.

The combination of predefined test range and  hierarchy make it possible to automatically remove all testcases after the tests are done. 

 1.  Nullify all optional FK columns in Test Records
 2.  Starting at the highest level delete all Test Records 

Automatically creating records is a bit more complicated. A big help here would be a generated API. Such an API can be extended to generate a "plain vanilla" testrecord. Using all mandatory FK columns it can also generate secondary cals to generate all necessary parent records before generating the asked for record.
Once generated this datastructure must of course be customized to express the actual situation that should be tested. 

To facilitate customisation the APi should automatically register which keys have been generated for wich tables in this testcase, so that it becomes possible to attach extra children below each parent where needed. Such a key registry can also be used to delete this specific testcase afterwards instead of wiping all testcases at once.
