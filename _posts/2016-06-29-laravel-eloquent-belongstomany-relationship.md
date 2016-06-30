---
layout: post
title:  "Laravel Eloquent belongsToMany relationship"
date:   2016-06-29 15:46:00 +0530
author:     "Navdeep Singh"
header-img: "img/laravel-eloquent-belongstomany-relationship.jpg"
categories: laravel
---

<p>Laravel Eloquent belongsToMany relationship explained using real time example of a quiz app. </p>

<p>Participants registered via Facebook API in Participants table and each day have a one question. And Each day participants need to give answer to that question.</p>

<p>So relation like Participants have many Questions with Answers Table. In Laravel belongsToMany relationship is like many-to-many relationship. Here we can say Participants have many Questions  with Pivot table Particpant_Answers</p>

<p>The table structure is like below:</p>
