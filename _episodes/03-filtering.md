---
title: "Filtering"
teaching: 0
exercises: 0
questions:
- "FIXME"
objectives:
- "FIXME"
keypoints:
- "FIXME"
---

Vi så hvordan vi kunne udvælge bestemte kolonner. og sortere resultatet.

Hvad hvis vi er ude efter bestemte rækker. Eksempelvis alle film fra 1999

    SELECT title, release_year, imdb_score
    FROM netflix
    WHERE release_year = 1999;

Eller hvor imdb_score er større end 7?

    SELECT title, release_year, imdb_score
    FROM netflix
    WHERE imdb_score > 7;
    
Eller hvor imdb_score er mellem 7 og 8

    SELECT title, release_year, imdb_score
    FROM netflix
    WHERE imdb_score > 7 AND imdb_score < 8;



{% include links.md %}
