### This is a C++ wrapper for SQLite C API database engine. ###

It was written to simplify and speedup coding of local database access.

easySQLite was written and tested with Visual C++ 2010 Express, but is not using any nonstandard extensions, so should compile with any other C++ compiler.

Why **easySQLite** solution is better than others ?

  * elegant, objective solution
  * explicit naming and calling
  * uses exceptions or methods return values
  * clear, understandable usage
  * flexible and expandable
  * strongly tested (tests included)

Just take a look at sample code.

Find more information in included html file and usage examples in project package.

```
//define table structure
Field definition_tbPerson[] = 
{
	Field(FIELD_KEY),
	Field("fname", type_text, flag_not_null),
	Field("lname", type_text, flag_not_null),
	Field("birthdate", type_time),
	Field(DEFINITION_END),
};

//define database object
sql::Database db;

try
{
	//open database file
	db.open("test.db");

	//define table object
	Table tbPerson(db.getHandle(), "person", definition_tbPerson);

	//remove table from database if exists
	if (tbPerson.exists())
		tbPerson.remove();

	//create new table
	tbPerson.create();

	//define new record
	Record record(tbPerson.fields());

	//set record data
	record.setString("fname", "Jan");
	record.setString("lname", "Kowalski");
	record.setTime("birthdate", time::now());

	//add 10 records
	for (int index = 0; index < 10; index++)
		tbPerson.addRecord(&record);

	//select record to update
	if (Record* record = tbPerson.getRecordByKeyId(7))
	{
		record->setString("fname", "Frank");
		record->setString("lname", "Sinatra");
		record->setNull("birthdate");

		tbPerson.updateRecord(record);
	}

	//load all records
	tbPerson.open();

	//list loaded records
	for (int index = 0; index < tbPerson.recordCount(); index++)
		if (Record* record = tbPerson.getRecord(index))
			sql::log(record->toString());

	sql::log("");
	sql::log("ALL OK");

} catch (Exception e) {
	printf("ERROR: %s\r\n", e.msg().c_str());
}
```