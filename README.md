# SQL_1
Handson Practice Basic SQL

create database Career_247;
use Career_247;
create table Students
(Student_ID int(4),
First_Name varchar(20),
Last_Name varchar(20),
Age int(2), 
Marks int(2), 
City varchar(20));
insert into Students values
(101,"Anjani","Tripathi",32,89,"Delhi"),
(102,"Shikha","Yadav",33,90,"Lucknow"),
(103,"Shweta","Singh",32,78,"Mau"),
(104,"Ranjana","Shuk;a",34,80,"Mainpuri"),
(105,"Param","Shukla",32,60,"Deoria"),
(106,"Shanvi","Singh",25,79,"Jaunpur");

select * from Students;

select First_Name,Age,Marks from Students;

select * from Students where Marks >= 70;

select * from students where Age>30;

select Student_ID, First_Name, Last_Name from students where marks>80;

select Student_ID, First_Name, Last_Name, Age from students where marks>60 and Age > 30;

alter table students add course varchar(20);

alter table students add Gender varchar(20);

select * from Students;



SET SQL_SAFE_UPDATES=0;

update students set course = "Data Analytics";

update Students set Gender = "Male" Where First_Name= "Param";

update Students set Gender = "Male" Where First_Name= "Anjani";

delete from students where Student_ID= 105;

update students set Student_ID = 105 where Student_ID=106;

## How many students are there more than 26 of Age

select count(*) as Student_Count from students where Age > 32;

update students set course = "Data Science" where First_Name = "Shanvi";

update students set course = "Digital Marketing" where First_Name = "Shikha";

select count(Student_ID) as Student_Course_Count from students  where course = "Data Analytics";

select sum(marks) as Total_Marks from Students;

select sum(marks) as Total_marks_Female from Students where Gender="Female";

select avg(Age) as Avg_age from Students;

select max(Marks) as Max_Marks from Students;

select min(Marks) as Min_Marks from Students;

select * from students where marks= (select max(Marks) as Max_Marks from Students);

select * from students;

select* from students where marks= ((select max(Marks) as Max_Marks from Students));

Select Max(Marks) from students where marks < (Select Max(Marks) from students); ##Highest Marks

SELECT *
FROM students
WHERE marks = (
    SELECT MAX(marks) 
    FROM Students
    WHERE marks < (SELECT MAX(marks) FROM Students)
); ## 2nd Highest Marks

select round(avg(Age),1) as Avg_age from Students;

insert into students values (106,"Shivam","Dhoni",26,69,"Delhi","Digital MArketing","Male");

delete from Students where Student_ID= 104;

update students set Student_ID = 104 where Student_ID = 105;

update students set Student_ID = 105 where Student_ID = 106;

select count(*) from Students where Age >= 26;

Alter table Students drop column Gender;

Select count(Student_ID) from Students where Course="Data Analytics";

update students set course = "Digital Marketing" where Student_ID = 105;

select max(marks) as Highest_Marks, min(marks) as Lowest_Marks from students;

select round(avg(Age),2) as Avg_Age from Students;

select floor(Avg(Age)) from Students;

select ceiling(Avg(age)) from Students;

select concat(First_Name," ", Last_Name) as Full_Name, Age from Students;

select First_Name, substring(First_Name,3,4) from Students;

select First_Name, length(First_Name) as Char_Count from Students;

Select concat(First_name," ", Last_Name) as Full_Name, Length(concat(First_name," ", Last_Name))-1 as Actual_Char_Count from Students;

select first_name, concat(upper(Left(First_Name,1)), Lower(substring(first_name,2,Length(first_name)))) as Proper_Case from Students;

