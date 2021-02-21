# Moodle_Reports
<b> These reports were designed for admins to help catagorize students </b>
<br>
<h4> Active User Report </h4>
<pre>
SELECT CONCAT(u.firstname , ' ' , u.lastname) AS 'Name', 
c.shortname AS 'Course Name'
FROM prefix_course_completions AS p
JOIN prefix_course AS c ON p.course = c.id
JOIN prefix_user AS u ON p.userid = u.id
WHERE p.timecompleted IS NULL AND u.city NOT LIKE 'Admin' AND c.shortname NOT LIKE 'Welcome to WGT (master)' AND c.shortname NOT LIKE 'Intro to Moodle' AND c.shortname NOT LIKE 'Admin Intro to Moodle' AND c.shortname NOT LIKE '%Workshop%' 
ORDER BY STR_TO_DATE(c.shortname,'%d ,%m') ASC
</pre>
<p> This report filtered out any admin to intro to moodle courses and any shortman that are not equal to anything that includes the word workshop </p> <br>

<h4> Full Completion Report </h4>
<pre>
SELECT CONCAT(u.firstname , ' ' , u.lastname) AS 'Name',
c.shortname AS 'Course Name',
DATE_FORMAT(FROM_UNIXTIME(p.timecompleted),'%d-%m-%Y') AS 'Completed Date'
FROM prefix_course_completions AS p
JOIN prefix_course AS c ON p.course = c.id
JOIN prefix_user AS u ON p.userid = u.id
WHERE p.timecompleted IS NOT NULL AND p.timecompleted IS NOT NULL AND u.deleted = 0 AND u.city NOT LIKE 'Admin'  AND c.category NOT LIKE 00 AND c.shortname NOT LIKE 'Welcome to WGT (master)' AND c.shortname NOT LIKE 'Intro to Moodle' AND c.fullname NOT LIKE '%Master%'
</pre>
<p>This report tells the admins who has completed courses and filters out the outs the courses that are assigned to you when you register an account with moodle </p><br>

<h4> Weekly Completion Report </h4>
<pre>SELECT CONCAT(u.firstname , ' ' , u.lastname) AS 'Name',
c.shortname AS 'Course Name',
FROM_UNIXTIME(p.timecompleted) AS 'TIme Completed'
FROM prefix_course_completions AS p
JOIN prefix_course AS c ON p.course = c.id
JOIN prefix_user AS u ON p.userid = u.id
WHERE p.timecompleted NOT LIKE 0 AND p.timecompleted > UNIX_TIMESTAMP(now()- INTERVAL 7 DAY) AND c.shortname NOT LIKE 'Intro to Moodle'
ORDER BY c.shortname ASC
</pre> 
<p>This report returns a list of students that have completed there courses that week and uses a fitler of now()- Interval 7 Day to filter out anything that has been completed in the last 7 days </p> <br> 

<h4> Weekly Enrolment </h4>
<pre>
SELECT CONCAT(u.firstname , ' ' , u.lastname) AS 'Name',
c.shortname AS 'Course Name',
FROM_UNIXTIME(u.timecreated) AS 'Account Created'
FROM prefix_user_enrolments as ue 
JOIN prefix_enrol AS e ON e.id = ue.enrolid
JOIN prefix_user AS u ON u.id = ue.userid
JOIN prefix_course AS c ON c.id= e.courseid
WHERE ue.timecreated > UNIX_TIMESTAMP(now()- INTERVAL 7 DAY) AND u.city NOT LIKE 'Admin' AND c.shortname NOT LIKE 'Welcome to WGT (master)' AND c.shortname NOT LIKE 'Intro to Moodle' AND c.shortname NOT LIKE '%Workshop%'
ORDER BY CONCAT(u.firstname , ' ' , u.lastname) ASC,DATE(FROM_UNIXTIME(u.timecreated)) DESC
</pre>
<p>This reports returns a list of students that have enroled that week </p><br>
