---
title: "Instructor Notes"
---
FIXME

Indsæt kode: 
blanklinie
to tabs, og kode
blankline. Så indsættes kode

Walkthrough af mysteriet.

Our first clue is that a murder happened on january 15 2018 in SQL City.

The structure of the crime_scene_report table can be found in this way:

    PRAGMA table_info(crime_scene_report)

So we can do a select, picking out the correct date, type of crime, and city:

    SELECT description FROM crime_scene_report
    WHERE date = '20180115' AND type = 'murder' AND city = 'SQL City'

We are told:

Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave".

So we have two witnesses.

We can find them individually by:

    select id from person
    where address_street_name = 'Northwestern Dr'
    order by address_number desc limit 1

The adress number of the last house on that road is the largest number on 
that road, so we order ny adress_number, in descending order, and only retrieves 
the first - which is the largest number.

That returns the id 14887

The second witness is named Annabel, and lives somewhere on Franklin Ave. 
We can find her by:

    select id from person
    where  name like 'Annabel%' 
    AND address_street_name = 'Franklin Ave'

That returns the id 16371

Here we will have to consider if we have introduced aliassing.

If, we could go by:

    WITH witness1 AS (
    SELECT id FROM person
    WHERE address_street_name = 'Northwestern Dr'
    ORDER BY address_number DESC LIMIT 1
    ), witness2 AS (
    SELECT id FROM person
    WHERE INSTR(name, 'Annabel') > 0 AND address_street_name = 'Franklin Ave'
    ), witnesses AS (
    SELECT *, 1 AS witness FROM witness1
    UNION
    SELECT *, 2 AS witness FROM witness2
    )
    SELECT witness, transcript FROM witnesses
    LEFT JOIN interview ON witnesses.id = interview.person_id

and get the two transcripts in one go:

Witness 1: 
I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".

Witness 2:
I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.


Otherwise we will do it one by one.


The complete solution from here would be:
    WITH gym_checkins AS (
      SELECT person_id, name
      FROM get_fit_now_member
      LEFT JOIN get_fit_now_check_in ON get_fit_now_member.id = get_fit_now_check_in.membership_id
      WHERE membership_status = 'gold' -- Only gold members have those bags
        AND id REGEXP '^48Z' -- membership number on the bag started with "48Z"
        AND check_in_date = '20180109' -- Witness 2 recognized him on January the 9th
    ), suspects AS (
      SELECT gym_checkins.person_id, gym_checkins.name, plate_number, gender
      FROM gym_checkins
      LEFT JOIN person ON gym_checkins.person_id = person.id
      LEFT JOIN drivers_license ON person.license_id = drivers_license.id
    )
    SELECT * FROM suspects -- The man got into a car with a plate that included "H42W"
    WHERE INSTR(plate_number, 'H42W') > 0 AND gender = 'male'

And we can put the name into:

    INSERT INTO solution VALUES (1, "Jeremy Bowers");
    SELECT value FROM solution;

To check our solution.

We are probably going step by step.

We do get at hint:

Congrats, you found the murderer! But wait, there's more... If you think you're up for a challenge, try querying the interview transcript of the murderer to find the real villain behind this crime. If you feel especially confident in your SQL skills, try to complete this final step with no more than 2 queries. Use this same INSERT statement with your new suspect to check your answer.

The information we get from the murderer tells us:

    SELECT transcript FROM interview WHERE person_id = 67318

I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.

The complete sql-call to find her is:

    WITH red_haired_tesla_drivers AS (
      SELECT id AS license_id
      FROM drivers_license
      WHERE gender = 'female' AND hair_color = 'red' -- She has red hair
      AND car_make = 'Tesla' AND car_model = 'Model S' -- and she drives a Tesla Model S
      AND height >= 64 AND height <= 68 -- she's around 5'5" (65") or 5'7" (67")
    ), rich_suspects AS (
      SELECT person.id AS person_id, name, annual_income
      FROM red_haired_tesla_drivers AS rhtd
      LEFT JOIN person ON rhtd.license_id = person.license_id
      LEFT JOIN income ON person.ssn = income.ssn
    ), symphony_attenders AS (
      SELECT person_id, COUNT(1) AS n_checkins
      FROM facebook_event_checkin
      WHERE event_name = 'SQL Symphony Concert' -- she attended the SQL Symphony Concert
        AND `date` REGEXP '^201712' -- in December 2017
      GROUP BY person_id
      HAVING n_checkins = 3 -- 3 times
    )
    SELECT name, annual_income
    FROM rich_suspects
    INNER JOIN symphony_attenders ON rich_suspects.person_id = symphony_attenders.person_id

And we can test the result:

    INSERT INTO solution VALUES (1, "Miranda Priestly");
    SELECT value FROM solution;

Once again, we will do this step by step.

{% include links.md %}
