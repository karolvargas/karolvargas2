---
title: Fantasy Football Consistency Checker
date: "2019-02-05"
featuredImage: './featured.jpg'
---

link to repository: https://github.com/karolvargas/TerminalFantasyNFL




Python app to look at NFLstat leaders from week to week up to the 2017 season. Feel free to edit this code how you like. For deeper documentation into the NFLGame library feel free to visit https://github.com/BurntSushi/nflgame for info into creating your own application. To process user input was a little bit tricky since we had to create a tuple to handle to different variable types. The documentation on the actual datatypes being processed by the RestAPI was not very well but below I have a attached a snippet of a function which takes in user input as the multiple paramaters required to get the JSON information from GameCenter. This example will demonstrate how the code works if someone wanted to see the top passers in week 5 of 2008. REMEMBER YOU RUN "python2" and not "python" or "python3" when running the proram.:

```
import nflgame
from nflgame import games

def welcome():

    print("Welcome to Python NFL Consistency Checker! ")
    stats = raw_input('Are you looking for offense or defense?(must be lowercase) ')
    print(type(stats))
    stats = str(stats)
    #stats = stats.strip()
    o = str("offense")
    d = str("defense")
    if stats == o:
        offense()
    elif stats == d:
        defense()
    else:
        print("Um lets try that again...")
        welcome()


    def offense():
        statis = raw_input('Enter the word: passing, rushing, or receiving. To see those stats: and the year you would like to see.(must blowercase) ')
        p = str("passing")
        r = str("rushing")
        rc = str("receiving")
        if statis == p:
            passing(statis)
        elif statis == r:
            rushing(statis)
        elif statis == rc:
            receiving(statis)
        else:
            print("Error!!! That input did not work! Try again: ")
            offense()


    def passing(statis):
        usr_first = raw_input("What year and week  would you like to view? (ex. 2018 8): ")
        usr_year = int(usr_first.split(" ")[0])
        week = int(usr_first.split(" ")[1])
        timeList = [usr_year, week]
        limit_in = raw_input("Enter the amount of people you would like to see for top stats for this week of this year. I.E. TOP 5, 10, 20: ")
        limit = int(limit_in)
        timeList.append(limit)
        print(usr_year)
        print(week)
        g = games(int(usr_year), int(week))
        players = nflgame.combine_game_stats(g)
        for p in players.passing().sort('passing_yds').limit(int(limit_in)):
            msg = '%s %d completions for %d yards and %d TDs'
            print msg % (p, p.passing_att, p.passing_yds, p.passing_tds)

```
