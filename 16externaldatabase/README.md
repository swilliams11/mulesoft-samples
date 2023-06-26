# Heroku - Create a Postgres Table

1. Register for Heroku

2. Install Heroku CLI with npm.

3. [Install Postgress Locally with CLI tools.](https://postgresapp.com/)

4. Configure the Database Configuration Select component in Mulesoft.
* Select Generic Connection
* JDBC Driver: select `add Maven Dependency` and then select the most current Maven dependency from the Maven repository.  https://mvnrepository.com/artifact/org.postgresql/postgresql/42.6.0

5. [Login](https://devcenter.heroku.com/articles/connecting-to-heroku-postgres-databases-from-outside-of-heroku#connect-via-heroku-pg-psql)

6. Create table and insert data

```
CREATE TABLE People (
    ID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
);

# list your tables
\dt

INSERT INTO People (ID, LastName, FirstName, Address, City)
VALUES ('1001', 'Cardinal', 'Grey', '124 Packers Street', 'York Land' );

INSERT INTO People (ID, LastName, FirstName, Address, City)
VALUES ('1002', 'Short', 'Martin', '10 Hollywood', 'Hollywood' );

SELECT * FROM People;
``` 

7. Create the `dbconfig.yaml` in the `src/main/resources` directory. 

8. Then you have to add a `Configuration Properties` file in the `Global Elements` tab and select your `dbconfig.yaml` file.

9. Make sure to update all the values in `dbconfig.yaml`.
