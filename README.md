-- SQL Script to create and populate tables for the MegaFirm case study

-- Drop the tables in case they already exist
DROP TABLE IF EXISTS employees;
DROP TABLE IF EXISTS departments;
DROP TABLE IF EXISTS locations;
DROP TABLE IF EXISTS jobs;

-- Create the locations table
CREATE TABLE locations
(
	location_id  	INTEGER,
	street_address	VARCHAR(50) unique not null,
	postal_code		VARCHAR(10) not null,
	city			VARCHAR(50) not null,
	state_province	VARCHAR(50) not null,
	country			VARCHAR(50) not null,
	constraint 		l_lid_pk PRIMARY KEY (location_id)
);

-- Create the jobs table
CREATE TABLE jobs
(
	job_id  		INTEGER,
	job_title		VARCHAR(50) unique not null,
	min_salary		DECIMAL(8,2) not null,
	max_salary		DECIMAL(8,2) not null,
	constraint 		j_jid_pk PRIMARY KEY (job_id)
);

-- Create the departments table
CREATE TABLE departments
(
	department_id 	INTEGER,
	department_name	VARCHAR(20) unique not null,
	location_id  	INTEGER not null,
	constraint 		d_did_pk PRIMARY KEY (department_id),
	constraint 		d_lid_fk FOREIGN KEY (location_id)
	references 		locations(location_id)
);

-- Create the employees table
CREATE TABLE employees
(
	employee_id 	INTEGER,
	first_name 		VARCHAR(50) not null,
	last_name 		VARCHAR(50) not null,
	email 			VARCHAR(50) not null unique,
	phone_number 	VARCHAR(15) not null,
	hire_date 		DATE not null, 
	salary 			DECIMAL(8,2),
	commission_pct	DECIMAL(2,2),
	manager_id 		INTEGER,
	department_id 	INTEGER,
	job_id 			INTEGER,
	constraint 		e_eid_pk PRIMARY KEY (employee_id),
	constraint 		e_mid_fk FOREIGN KEY (manager_id)
	references 		employees(employee_id),
	constraint 		e_did_fk FOREIGN KEY (department_id)
	references 		departments(department_id),
	constraint 		e_jid_fk FOREIGN KEY (job_id)
	references 		jobs(job_id)
);

-- Populate the locations table
INSERT INTO 
locations (location_id, street_address, postal_code, city, state_province, country)
VALUES 
(100, '2 Nice Road', 'N2 7TH', 'London', 'Greater London', 'UK'),
(200, '23 Pretty Road', 'BS1 8FD', 'Bristol', 'Bristol County', 'UK'),
(300, '26 Great Street', 'BN1 4BF', 'Brigthon', 'Sussex', 'UK'),
(400, '143 Lovely Road', 'CB1 2NV', 'Cambridge', 'Cambridgeshire', 'UK');

-- Populate the jobs table
INSERT INTO 
jobs (job_id, job_title, min_salary, max_salary)
VALUES
(901, 'Managing Director', 75000, 125000),
(902, 'Programmer', 35000, 80000),
(903, 'Sales Rep', 25000, 45000),
(904, 'Project Manager', 45000, 95000),
(905, 'Marketing Manager', 37000, 68000),
(906, 'Marketing Producer', 34000, 70000),
(907, 'Operations Manager', 28000, 41000),
(908, 'Sales Administrator', 38000, 65000),
(909, 'Database Architect', 44000, 73000),
(910, 'Operations Officer', 32000, 61000),
(911, 'IT Tester', 38000, 55000),
(912, 'Finance Director', 72000, 115000);

-- Populate the departments table
INSERT INTO 
departments (department_id, department_name, location_id)
VALUES
(10, 'IT', 100),
(20, 'Operations', 200),
(30, 'Sales', 300),
(40, 'Marketing', 400),
(50, 'Management', 100),
(60, 'Security', 200);

-- Populate the employees table
INSERT INTO 
employees (employee_id, first_name, last_name, email, phone_number, hire_date, salary, 
			commission_pct, manager_id, department_id, job_id)
VALUES 
(1001, 'Jim', 'King', 'jk@firm.com', '02079111001', '2011-01-21', 98000, NULL, NULL, 50, 901),
(1002, 'Jane', 'Queen', 'jq@firm.com', '02079111002', '2012-02-05', 99000, NULL, NULL, 50, 901),
(1003, 'Jen', 'Probert', 'jp@firm.com', '02079111003', '2014-11-23', 79000, NULL, 1001, 10, 904),
(1004, 'Mike', 'Brent', 'mb@firm.com', '02079111004', '2013-10-06', 51000, NULL, 1003, 10, 902),
(1005, 'Nadia', 'Tamsa', 'nt@firm.com', '02079111005', '2013-10-08', 62000, NULL, 1003, 10, 902),
(1006, 'Mo', 'Ali', 'ma@firm.com', '02079111006', '2015-11-24', 41000, 0.15, 1002, 30, 903),
(1007, 'Dania', 'Kolova', 'dk@firm.com', '02079111007', '2016-05-15', 38000, 0.25, 1006, 30, 903),
(1008, 'Manu', 'Ogoda', 'mo@firm.com', '02079111008', '2017-08-12', 33000, 0.35, 1007, 30, 903),
(1009, 'Marc', 'Daniel', 'md@firm.com', '02079111009', '2014-01-02', 35000, 0.35, 1007, 30, NULL),
(1010, 'Louise', 'Matos', 'lm@firm.com', '0207911110', '2017-11-05', 53000, NULL, 1002, 40, 905),
(1011, 'Ram', 'Binghi', 'rb@firm.com', '0207911111', '2012-03-30', 35000, NULL, 1010, 40, 906),
(1012, 'Tim', 'Norm', 'tn@firm.com', '0207911112', '2018-03-31', 48000, NULL, 1010, 40, 906),
(1013, 'Alex', 'Smart', 'as@firm.com', '0207911113', '2012-03-30', 39000, NULL, 1001, NULL, 907),
(1014, 'Bruno', 'Silba', 'bs@firm.com', '0207911114', '2014-05-08', 37000, NULL, 1013, 20, 910),
(1015, 'Laurie', 'Kaldav', 'lk@firm.com', '0207911115', '2017-08-11', 34000, NULL, 1013, 20, 910),
(1016, 'Sophie', 'Lanou', 'sl@firm.com', '0207911116', '2017-08-19', 34000, 0.10, 1007, 30, 908),
(1017, 'Yann', 'Taylor', 'yt@firm.com', '0207911117', '2018-09-03', 44000, NULL, 1002, 10, 909),
(1018, 'Sam', 'Tring', 'st@firm.com', '0207911118', '2018-09-05', 47000, NULL, 1003, 10, 909),
(1019, 'Don', 'Matos', 'dm@firm.com', '0207911119', '2017-10-04', 49000, NULL, 1003, NULL, 909),
(1020, 'Dan', 'Ritch', 'dr@firm.com', '0207911120', '2019-01-14', 35000, NULL, 1013, 20, 910),
(1021, 'Jenna', 'Novski', 'jn@firm.com', '0207911121', '2019-02-13', 38000, NULL, 1005, 10, 911),
(1022, 'Malia', 'Mundi', 'mm@firm.com', '0207911122', '2019-07-15', 43000, NULL, 1005, 10, 911),
(1023, 'Kurt', 'Thorpe', 'kt@firm.com', '0207911123', '2020-04-21', 44000, NULL, 1018, 10, 911),
(1024, 'Nikolai', 'Mikonov', 'nm@firm.com', '0207911124', '2020-11-02', NULL, NULL, NULL, NULL, NULL),
(1025, 'Gino', 'Gondolini', 'gg@firm.com', '0207911125', '2020-11-02', NULL, NULL, NULL, NULL, NULL);



