# Java Database Class

To use the SILK Java Database Class in your own Java class or JSP code you have to import the `com.oopsclick.silk.dbo.*` class package together with other classes you may need.

For a java class use: `import com.oopsclick.silk.dbo.*;`

For a JSP code use: `<%@ page import="com.oopsclick.silk.dbo.*" %>`

This package provides multiple classes to submit and retrieve data between the application and the database. The main class is the DataProvider.

## DataProvider

The DataProvider is the only class needed to connect to an ORM object and use its methods to update data into the connected database. A DataProvider can only be connected to one ORM object. However an application, or program, can contain multiple DataProviders connected to different ORM objects.

There are multiple ways to use a DataProvider in an application and it is up to the programer to decide the level of complexity based on application requirements. This documentation will start with the simple way to use the DataProvider and scale to a more complex scenarios.

### Constructor

A DataProvider is connected to an ORM object. The ORM Object path has to be provided during the class instantiation. In order to easily recognize a DataProvider it is suggested to add the postfix "**DP**" at the end of the object name.

If you know the exact location of the ORM file use this constructor.

```java
DataProvider dataDP = DataProvider("/test/testPerson");
```

If you are using the the SILK-Builder environment to define a DataProvider in a Java code inside a JSP file use this constructor.

```java
<%
  DataProvider dataDP = DataProvider("/Test/testPerson", session);
%>
```

In the example above the DataProvider dataDP is connected to the testPerson.orm object. All the operation are going to be related to this ORM Object configuration. Read the chapter [The ORM Structure](https://github.com/italoosorio/SilkBuilder-Doc/tree/114abd74d7dc54aed208ee35453af719b54ff910/docs/.01_The_ORM_Structure.md) to understand how the ORM is configured.

### Call Select

To extract information from the database the `select()` method is used. This method will call a SQL SELECT command store in a `<sqlSelect>` tag in the `testPerson.orm` object. The `select()` method return the number of record extracted after executing the SQL SELECT command.

```java
int totalRecords = dataDP.select();
```

The `select()` method can received a selectName parameter, `select(selectName)`, which allows to call different SQL SELECT commands store in the ORM Object. For example:

```java
int totalRecords = dataDP.select("onlyFemales");
```

In this example the `select("onlyFemales")` command will call for the SQL SELECT stored in the `<sqlSelect name="onlyFemales" >` tag in the ORM Object.

### Get Data Items

After the `select()` method is executed the data extracted is stored in the `DataProvider`. One way to get access to the data is by looping its records and calling the information using the `getItemAt()` method. The loops is usually a `for()` command which will use as top limit the number of records returned by the `select()` command. It should start counting from 0 using single increments.

There are several methods to extract the data from a record depending in how the programer wants to use the data. The basic one is `getItemAt()` method which returns data with the type Object. In this case date data has to be cast to the type in which the programers requirements. This method received two parameters: the index position of the record to accessed, and the column's name of the information to be extracted. For Example:

```java
String name = (String) dataDP.getItemAt(x,"name");
```

There are method which will return data in specific types: `getStringItem()`, `getIntItem()`, `getDoubleItem()`, `getLongItem()`, and `getDateItem()`. These are use in this way:

```java
Double name = dataDP.getDoubleAt(x,"monthlyIncome");
```

Ther are methods retuning string formatted values with parameters to support formatting templates: `getIntStringItem()`, `getDoubleStringItem()`, and `getDateStringItem()`. These are used in this way:

```java
String birthDate = dataDP.getDateStringItem(x,"birthDate");
Below is example of a common loop for getting data from multiple rows. This is using the data specific methods.
for( int x=0; x<totalRecords; x++ ){
    String name = dataDP.getStringItem(x,"name");
    int married = dataDP.getIntItem(x,"married");
    Double monthtlyIncome = dataDP.getDoubleItem(x,"monthtlyIncome");


    /*
     * Other operations
     */
}
```

In the cases when the returned data is expected to be only one row the code could look like this:

```java
String name = dataDP.getStringItem("name");
int married = dataDP.getIntItem("married");
Double monthtlyIncome = dataDP.getDoubleItem("monthtlyIncome");

/*
 * Other operations
 */
```

Notice that in this case the method does not received the index, only the column. Under this conditions the method will always return the first row value. If the SQL SELECT does not return a row the method will return null.

### Call Insert

To insert a record in the database the information has first be loaded in the `DataProvider` using the `setFormItem()` method, and later executing the `insert()` method. The `setFormItem()` method receives to parameters: column and value. The `insert()` method return an integer with the number of affected rows. The programer does not have to send the primary key except if the ORM Object table is configure to `pkMode="value"`. The generated primary key can be extracted using the same methods explained before. It is a good practice to clean the form values using the method cleanForm\(\) before loading values.

The code to insert data looks like this:

```java
dataDP.cleanForm();
dataDP.setFormItem("name", "Bill Gates");
dataDP.setFormItem("phone", "(209) 738-8660");
dataDP.setFormItem("address", "584-7326 Litora Road");
dataDP.setFormItem("email", "bill@microsoft.com");
int totalRecords = dataDP.insert();
String personID = dataDP.getStringItem("personID");
```

### Call Update

To update a record follows a similar process as the insert process. The difference is that in this case the primary key has to be provided together with the data to be modify. and execute the `update()` method.

The following code updates the columns phone and email:

```java
dataDP.cleanForm();
dataDP.setFormItem("personID", personID);
dataDP.setFormItem("phone", "(301) 878-4587");
dataDP.setFormItem("email", "bill@hotmail.com");
int totalRecords = dataDP.update();
```

### Call Delete

To delete a record only the primary key has to be provided, and the `delete()` method executed.

```java
dataDP.cleanForm();
dataDP.setFormItem("personID", personID);
int totalRecords = dataDP.delete();
```

## Batch Process

## Requests Object

## Response Object

