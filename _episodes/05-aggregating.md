---
title: "Aggregating & calculating values"
teaching: 0
exercises: 0
questions:
- "FIXME"
objectives:
- "FIXME"
keypoints:
- "FIXME"
---

An aggregate is something formed or calculated by combining several 
separate elements. A total eg. or an average.

We can do that.


The average imdb_rating in the netflix table:

    SELECT avg(imdb_score)
    FROM netflix;


The max-rating:

    SELECT max(imdb_score)
    FROM netflix;
    
Minium



Another way of getting the maximum score would be to order the table by score
in descending order, and picking out the first result:

    SELECT *
    FROM netflix
    ORDER BY imdb_score DESC
    LIMIT 1;

Limit is also useful to get eg the top 10 results:

    SELECT *
    FROM netflix
    ORDER BY imdb_score DESC
    LIMIT 10;


{% include links.md %}
