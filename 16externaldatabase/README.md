# Heroku - Create a Postgres Table

1. Register for Heroku

2. Install Heroku CLI with npm.

3. [Install Postgress Locally with CLI tools.](heroku pg:psql postgresql-fitted-55798 --app postgres923849238492)

4. [Login](https://devcenter.heroku.com/articles/connecting-to-heroku-postgres-databases-from-outside-of-heroku#connect-via-heroku-pg-psql)

5. Create a database and insert data

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

6. Create the `dbconfig.yaml` in the `src/main/resources` directory. 

7. Then you have to add a `Configuration Properties` file in the `Global Elements` tab and select your `dbconfig.yaml` file.

8. Make sure to update all the values in `dbconfig.yaml`.
