+++
date = "2015-06-28T16:46:33+05:30"
title = "Functional programming in python"

+++

Python offers some good functions to the developers to write functional like code. But their use is not
recommended as it reduces code readability. They may come handy in some places though. 
<!--more-->
This is an example program showing the use of functional concepts in python.

**Problem:** Given a song, find out if the song is a pi song or not. If it is a pi song print "It's a pi song."  else
print "It's not a pi song."

A song is a pi song, if the lenght of the words in it matches the digits of pi.

    T = input()

    pi = "31415926535897932384626433833"

    def calculate(song):
          words = map(lambda x: str(len(x)), song.split(" "))
          p = reduce(lambda a, b: a + b, words)

          if(p == pi[:len(p)]):
              print "It's a pi song."
          else:
              print "It's not a pi song."
              

    for _ in range(T):
        song = raw_input()
        calculate(song)

**Note:** This problem is a challenge from hackerrank. I couldn't find a link to the problem in it, I guess the challenge is probably removed.