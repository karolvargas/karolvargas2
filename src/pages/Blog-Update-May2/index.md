---
title: Blog Update May 2nd
date: "2018-05-02"
---

##QuizAGator 1.0.0

Quizagator is a web application that provides an interface for creating quizzes without the overhead of a GUI making quizzes creation tedious and in the case that the design of the GUI tool changes confusing. By allowing quiz creation through a text-based syntax it is possible to make quiz creation lightning fast and much more consistent than fiddling with GUI tools. Not to mention a text-based quiz creation allows for easy question duplication and modification. Quizagator supports uploading quizzes in CSV format and allows for grading with a custom grading program. The tool uses Flask with noSQL to manage quizzes and results, as well as storing any custom grading tools uploaded to the quizzes.

The tasks that I completed was helping find a way that we can implement a csv file uploader to allow for parsing of the csv file. This would in turn allow for teachers to upload their quizzes and then have the program parse through the quiz and create a quiz in online format. This quiz also takes in both multiple-choice and open-ended questions. I did my work on the /csv-upload branch and pushed out the button which allows the
teacher to select which csv file they wish to upload as well as upload it to a non-existing directory stored in the repository. The challenges that I faced creating this feature had mostly to do with the route in flask. I have used node extensively and was not very familiar with flask so this was a part of the programming language that I had to adjust to. The code I implemented is shown below.


```
//teachers.py file
@app.route("/teachers/quizzes/create/", methods=["POST"])
@db.validate_teacher
def create_quiz():
    """ create a quiz """
    db.insert_db(
        "INSERT INTO quizzes (topic_id, creator_id, name) VALUES (?, ?, ?);",
        [flask.request.form["topic"], flask.session["id"], flask.request.form["name"]],
    )
    flask.flash("The quiz was created.")
    return flask.redirect("/teachers/quizzes/")


//quizzes.html file
<h2>Create a Quiz</h2>
<form class="form-horizontal well" method="POST" action="/teachers/quizzes/create/">
  <fieldset>
    <div><span>Quiz Name: </span><input type="text" name="name"  required/></div>
    <div><input type="submit" class="btn btn-primary"></div>
```
