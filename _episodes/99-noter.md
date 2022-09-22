---
title: "Noter"
teaching: 0
exercises: 0
questions:
- "Hvordan pokker organiserer vi det her?"
- "Why SQL"
- "Relational database and SQL - what is the relation?"
objectives:
- "Define a relational database"
- "Explain what SQL is, and why we use it"
keypoints:
- "SQL is a powerful language to retrieve and manipultate data in relational databases."
---

Vi har et imdb datasæt. Og et netflix datasæt. Daniel sender os links.

Hvad er average rating på det der ligger på netflix. Og det der ikke ligger på
netflix. sammenlignet med alt hos imdb.

Åben db browser (efter du har downloaded data filerne.)
Ny database. giv den et navn. giv dem et navn at give den.

import table from csv-fil
vælg rette fil - title fra netflix
hak af i columnnames in first line.
OK.

Gentag 
Men vælg tab-separated fil format.
og field separator - tab.
Tænk over tabelnavngivning! 

Gå over i execute sql - vi kan have forskellige scripts.

Vi inspicerer data. tconst lader til at matche imdb_id i netflix basen
Det allerførste er måske at begrænse hvor mange kolonner vi gider se på
i netflixbasen. 

tconst og average rating. imdb_id, imdb_score, title

SELECT title, imdb_id, imdb_score FROM netflix_tabel
klik på command-enter (eller ctrl-enter)

Vi kunne godt tænke os at finde den bedste film!
SELECT title, imdb_id, imdb_score FROM netflix_tabel ORDER BY imdb_score desc

Der er null-værdier med.
Den kan vi sortere fra. 

Prøv
select 
where
from
order by.

Det går galt! 
SQL er hys med rækkefølgen!

Den går også gal fordi vi har imdb_votes registreret som text!

Tilbage i database structur - modify table. Tag dem alle...

Så kører det !


SELECT avg(imdb_score), age_certification
FROM netflix_titles
WHERE imdb_votes > 500
GROUP BY age_certification
ORDER BY avg(imdb_score)

det er måske værd at nævne kommentarer.

SELECT avg(imdb_score) AS average, age_certification
FROM netflix_titles
WHERE imdb_votes > 500
GROUP BY age_certification
ORDER BY avg(imdb_score)

LIMIT 100 begrænser.

Lad os alle joine

4987


{% include links.md %}

