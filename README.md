    csvsql --db mysql+mysqlconnector://root:akUras1083@localhost:3366/big_alien_witness_data_SCH --insert --tables raw_input /home/ak/dblab/raw_input.csv
---
    mysql -h 127.0.0.1 -P 3366 -u root -p
    CREATE VIEW aliv as select DISTINCT alien_id, alien_name, alien_type, alien_color FROM raw_input;
	CREATE VIEW vvit as select DISTINCT witness_id, witness_name, witness_last_name, witness_address, witness_age FROM raw_input;
	CREATE VIEW vloc as select DISTINCT location_id, lat, lon, place, country, region, time_of_day FROM raw_input;
	CREATE VIEW td as select DISTINCT time_of_day FROM raw_input;
    select place, count(*) from raw_input group by  place order by count(*) desc limit 1;
    Ramla |  1401
    select region, count(*) from raw_input group by region order by count(*) limit 1;
    Europe/Zaporozhye |   507
    select time_of_day, count(*) from raw_input group by time_of_day order by count(*) desc limit 1;
    day         |  5061
    SELECT DISTINCT count(Distinct location_id) from raw_input;
    165
    
    psql -U postgres -h localhost -p 5533
    docker exec -it postgres bash

    pgloader  script.lisp

> LOAD DATABASE
FROM mysql://root:akUras1083@62.244.0.10:3366/big_alien_witness_data_SCH
INTO postgresql://postgres@localhost/alien
WITH include drop, create tables, create indexes, reset sequences
BEFORE LOAD DO
$$ create schema if not exists alien; $$;

	CREATE TABLE main (
	alien_id INT NOT NULL,
	witness_id INT NOT NULL,
	location_id INT NOT NULL,
	PRIMARY KEY (alien_id, witness_id, location_id),
	FOREIGN KEY (alien_id)
	REFERENCES aliens (alien_id)
	ON DELETE CASCADE ,
	FOREIGN KEY (witness_id)
	REFERENCES witnesses (witness_id)
	ON DELETE CASCADE ,
	FOREIGN KEY (location_id)
	REFERENCES locations (location_id)
	ON DELETE CASCADE 
	);
	CREATE TABLE aliens (
	alien_id SMALLSERIAL PRIMARY KEY,
	alien_name VARCHAR (255) NOT NULL,
	alien_type VARCHAR (255) NOT NULL,
	alien_color VARCHAR (255) NOT NULL
	);
	CREATE TABLE witnesses (
    witness_id SMALLSERIAL PRIMARY KEY,
    witness_name VARCHAR (255),
    witness_last_name VARCHAR (255),
    witness_address VARCHAR (255),
    witness_age INT
	);
	CREATE TABLE locations (
    location_id serial PRIMARY KEY,
    lat DOUBLE PRECISION,
    lon DOUBLE PRECISION,
    place VARCHAR (255),
    country VARCHAR (255),
    region VARCHAR (255),
    time_of_day VARCHAR (255)
	);


    INSERT INTO aliens(alien_name, alien_type, alien_color)
    SELECT DISTINCT alien_name, alien_type, alien_color FROM raw_input;

    INSERT INTO witnesses(witness_name, witness_last_name, witness_address, witness_age)
    SELECT DISTINCT witness_name, witness_last_name, witness_address, witness_age FROM raw_input;

    INSERT INTO locations(lat, lon, place, country, region, time_of_day)
    SELECT DISTINCT lat, lon, place, country, region, time_of_day FROM raw_input;

    DELETE FROM aliens
    WHERE alien_id = 8;
