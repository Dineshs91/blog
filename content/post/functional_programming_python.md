+++
date = "2015-06-28T16:46:33+05:30"
title = "Functional programming in python"

+++

Python offers some good functions to the developers to write functional like code. But their use is not
recommended as it reduces code readability. They may come handy in some places though. 

This is an example program showing the use of functional concepts in python.
<!--more-->

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

