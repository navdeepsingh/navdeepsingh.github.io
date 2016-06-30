---
layout:     post
title:      "Laravel Eloquent belongsToMany relationship"
subtitle:   "Realtime example to understand"
date:       2016-06-29 15:46:00 +0530
author:     "Navdeep Singh"
header-img: "img/laravel-eloquent-belongstomany-relationship.jpg"
categories: laravel
---

Laravel Eloquent belongsToMany relationship explained using real time example of a quiz app. 

<p>Participants registered via Facebook API in Participants table and each day have a one question. And Each day participants need to give answer to that question.</p>

<p>So relation like Participants have many Questions with Answers Table. In Laravel belongsToMany relationship is like many-to-many relationship. Here we can say Participants have many Questions  with Pivot table Particpant_Answers</p>

<p>The table structure is like below:</p>
{% highlight %}
Schema::create('participants', function (Blueprint $table) {
            $table->increments('id');
            $table->bigInteger('fb_id');
            $table->string('name');
            $table->string('email');
            $table->timestamps();
        });

Schema::create('questions', function (Blueprint $table) {
            $table->increments('id')->unsigned();
            $table->string('question');
            $table->timestamps();
        });

Schema::create('participant_answers', function (Blueprint $table) {
            $table->increments('id');
            $table->integer('participant_id')->unsigned();
            $table->foreign('participant_id')
                ->references('id')->on('participants');
            $table->integer('question_id')->unsigned();
            $table->foreign('question_id')
                ->references('id')->on('questions');
            $table->string('answer');
            $table->timestamps();
});        
{% endhighlight %}

<p>In Participant Model we make a connection with Question and Participant Answers via belongsToMany like below</p>