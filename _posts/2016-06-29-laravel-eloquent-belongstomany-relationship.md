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

{% highlight php %}
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
{% highlight php %}
class Participant extends Model {
 public function answers()
    {
        return $this->belongsToMany('App\Question', 'participant_answers', 'participant_id', 'question_id')->withPivot('answer')
            ->withTimestamps();
    }
}
{% endhighlight %}

<p>'participant_answers' is pivot table. Btw what is Pivot table? you may doubted. Pivot table is table that only come into existence to serve a many-to-many relationship. Say here to get all participant answers we need a table which carry both participant_id, question_id and of course answer.</p>

<p>Insert data into pivot table like below:</p>
{% highlight php %}
$participant->answers()->sync([$question->id => ['answer' => $request_answer] ]);
{% endhighlight %}

<p>And to access all answers of a participant is like :</p>
{% highlight php %}
$participant->answers;
{% endhighlight %}

<p>Any doubt you can contact me, love to help.</p>
