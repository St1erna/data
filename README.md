create table registration 
(
    number bigint primary key,
	stu_id integer,
	cou_code char(5), 
	date date,
	status varchar(2),
	
	foreign key(cou_code)REFERENCES course(code) ON UPDATE CASCADE, 
	foreign key(stu_id)REFERENCES student(id) ON UPDATE CASCADE
);
create table student
(
	id int primary key,
	f_name varchar(10),
	l_name varchar(30),
	city varchar(15),
	dep_code int,
	tel char(10),
	
	foreign key(dep_code)REFERENCES department(code) ON UPDATE CASCADE
);
create table course
(
	code char(5) primary key,
	name varchar(15),
	tea_id int,
	dep_code int,
	
	foreign key(tea_id)REFERENCES teacher(id) ON UPDATE CASCADE,
	foreign key(dep_code)REFERENCES department(code) ON UPDATE CASCADE
);
create table department
(
	code int primary key,
	name varchar(30)
);
create table teacher
(
	id int primary key,
	fullname varchar(40),
	dep_code int,	
	
	foreign key(dep_code)REFERENCES department(code) ON UPDATE CASCADE
);

insert into department(code, name) values 
(1, 'computer science'),
(2, 'physic');

insert into student(id, f_name, l_name, city, dep_code, tel) values
(100, 'Michale', 'Robbin', 'Malmö', 1, 0969213828),
(101, 'Carlos', 'Manuel', null, 2, null),
(102, 'Enric', 'Sitaraman', 'Malmö', 1, 0764211125),
(103, 'Joseph', 'Dosni', 'Lund', 2, 01699013522),
(104, 'Mario', 'Robbin', null, 1, null);

insert into teacher(id, fullname, dep_code) values
(138, 'Maria Foster', 1),
(221, 'George Mardy', 2);

insert into course(code, name, tea_id, dep_code) values
('CAP21', 'Programming', 138, 1),
('CAP33', 'Database', 138, 1),
('PIS32', 'Thermodynamics', 221, 2);

insert into registration(number, stu_id, cou_code, date, status) values
(12, 102, 'PIS32', '2020-02-06', 'VG'),
(13, 104, 'CAP21', '2020-02-06', 'G'),
(14, 100, 'CAP21', '2020-11-18', 'NS'),
(15, 102, 'CAP21', '2020-11-18', 'NS'),
(16, 103, 'PIS32', '2020-11-18', 'NS');

Alter table registration 
Add check(status in ('NS', 'VG', 'G', 'U'));

Update registration 
set status = 'G'
Where (number = 13);

select id, f_name, l_name from student
Where f_name Like 'M%' and city IN ('Malmö');

select Count(*) from student
Where (dep_code = 1);

select dep_code, Count(*) from student
group by dep_code;

select city from student
where city like '%';

select city from student 
where city is not null;

select id from student
where id not in (select stu_id from registration);

select f_name from student
left join registration
ON student.id = registration.stu_id
where student.id not in (select stu_id from registration);
