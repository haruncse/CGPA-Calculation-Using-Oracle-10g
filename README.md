CGPA calculation Using Oracle 10g



--------------------CGPA----------------Calculation--------By------Harun----


----------------------Student Information:---------------------

CREATE TABLE student_info(
 stid char(10) NOT NULL,
 sm_id varchar(6),
 sb1 float,
 sb2 float, 
 sb3 float, 
 sb4 float, 
 sb5 float, 
 sb6 float );

INSERT INTO student_info VALUES('6','1-1',3,3.5,4,2,4,3);

INSERT INTO student_info VALUES('9','1-2',3,3.5,2,1,4,2);

INSERT INTO student_info VALUES('4','2-1',3,2.5,4,2,1,3);

INSERT INTO student_info VALUES('10','2-2',3,3.5,1,4,4,4);


SELECT * FROM student_info;

TRUNCATE TABLE student_info;

DROP TABLE student_info;


-------------------------------Semester Information:-----------------------

CREATE TABLE semester_info(
 sm_id varchar(6),
 sb1 float,
 sb2 float, 
 sb3 float, 
 sb4 float,  
 sb5 float, 
 sb6 float, 
 total float 
);

INSERT INTO semester_info VALUES('1-1',3,2,1,3,2,3,14);

INSERT INTO semester_info VALUES('1-2',3,1,1,3,2,3,13);

INSERT INTO semester_info VALUES('2-1',3,2,1,3,3,3,15);

INSERT INTO semester_info VALUES('2-2',3,2,3,3,2,3,16);

INSERT INTO semester_info VALUES('3-1',3,2,1,3,2,3,14);

INSERT INTO semester_info VALUES('3-2',3,1,1,3,2,3,14);

INSERT INTO semester_info VALUES('4-1',3,2,1,3,2,3,14);

INSERT INTO semester_info VALUES('4-2',3,1,1,3,2,3,14);

SELECT * FROM semester_info;

TRUNCATE TABLE semester_info;

DROP TABLE semester_info;


-------------------------------------CGPA Multiplear Table:-----------------------

CREATE TABLE cmr(
stid char(15),
mlr float
);


SELECT * FROM cmr;

TRUNCATE TABLE cmr;

DROP TABLE cmr;

-------------------------------------Result Table:---------------------------------


CREATE TABLE student_result
(
Id varchar(10),
CGPA float
);

SELECT * FROM student_result;

TRUNCATE TABLE student_result;

DROP TABLE student_result;

-----------------------------Trigger Of Student table on Each Subject:--------------

CREATE OR REPLACE TRIGGER sb1_before_insert
BEFORE INSERT
 ON student_info
 FOR EACH ROW

DECLARE
  vml NUMBER(10,5);

BEGIN

 SELECT sb1 INTO vml FROM semester_info where sm_id=:new.sm_id;

insert into cmr values((:new.stid),(:new.sb1 * (vml)));

INSERT INTO student_result VALUES((:new.stid),((SELECT SUM(mlr)FROM cmr where stid =(:new.stid))/(SELECT total FROM semester_info WHERE sm_id=:new.sm_id)));

END;
/


CREATE OR REPLACE TRIGGER sb2_before_insert
BEFORE INSERT
 ON student_info
 FOR EACH ROW

DECLARE
  vml NUMBER(10,5);

BEGIN

 SELECT sb2 INTO vml FROM semester_info where sm_id=:new.sm_id;

insert into cmr values((:new.stid),(:new.sb2 * (vml)));

END;
/


CREATE OR REPLACE TRIGGER sb3_before_insert
BEFORE INSERT
 ON student_info
 FOR EACH ROW

DECLARE
  vml NUMBER(10,5);

BEGIN

 SELECT sb3 INTO vml FROM semester_info where sm_id=:new.sm_id;

insert into cmr values((:new.stid),(:new.sb3 * (vml)));

END;
/


CREATE OR REPLACE TRIGGER sb4_before_insert
BEFORE INSERT
 ON student_info
 FOR EACH ROW

DECLARE
  vml NUMBER(10,5);

BEGIN

 SELECT sb4 INTO vml FROM semester_info where sm_id=:new.sm_id;

insert into cmr values((:new.stid),(:new.sb4 * (vml)));

END;
/


CREATE OR REPLACE TRIGGER sb5_before_insert
BEFORE INSERT
 ON student_info
 FOR EACH ROW

DECLARE
  vml NUMBER(10,5);

BEGIN

 SELECT sb5 INTO vml FROM semester_info where sm_id=:new.sm_id;

insert into cmr values((:new.stid),(:new.sb5 * (vml)));

END;
/


CREATE OR REPLACE TRIGGER sb6_before_insert
BEFORE INSERT
 ON student_info
 FOR EACH ROW

DECLARE
  vml NUMBER(10,5);

BEGIN

 SELECT sb6 INTO vml FROM semester_info where sm_id=:new.sm_id;

insert into cmr values((:new.stid),(:new.sb6 * (vml)));

END;
/



--------------------------Showing The Result-------------------------

SELECT * FROM student_result;

SELECT * FROM semester_info;

SELECT * FROM student_info;

SELECT * FROM cmr;
