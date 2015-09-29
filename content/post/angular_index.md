+++
Description = ""
Tags = []
date = "2015-09-29T23:30:32+05:30"
title = "Problem with using angular's $index in ng-repeat"

+++

ng-repeat is used to iterate through objects and act upon them individually. $index gives the index of an
object in the array.
<!--more-->

    <div ng-repeat="log in logs">
        <span class="remove" ng-click="remove($index)"></span>
    </div>
    

    $scope.remove = function($index) {
        // Code for removing log
        remove($scope.logs[$index]);
    }
    

The above code snippet, works fine for most of the scenarios. But the moment, we start to use orderBy or filter in ng-repeat,
things start to break. Why does it break ?

It breaks because $index gives the index from the origin array ```logs```. It doesn't work on filtered or ordered logs, but
works on the original array.

    logs = [
        'log1': {
            timestamp: 123456
        },
        'log2': {
            timestamp: 123499
        }
    ]
    

If an orderBy is enforced (Descending order), this is what we get after orderBy

    logs = [
        'log2': {
            timestamp: 123499
        },
        'log1': {
            timestamp: 123456
        },
    ]
    

Now if you use $index to find the index of **log2**, it gives 0 (Indexing starts from 0) whereas it should have been 1,
because 1 is the index of **log2** in the original logs array. If you use ```$scope.logs[$index]``` it will select **log1** not **log2**.
But you will be under the impression, that you have selected **log2**.
From the above example, its clear what is the problem.

So, how to overcome this ?
Good question, there is an easy solution.

    <div ng-repeat="log in logs">
        <span class="remove" ng-click="remove(log)"></span>
    </div>

    $scope.remove = function(log) {
        // Code for removing log
        var index = $scope.logs.indexOf(log);
        remove($scope.logs[index]);
    }
    

Instead of passing $index into the function, pass the object itself and use ```indexOf``` to get the index, which returns
index 1 for **log2**, since indexOf **log2** in original logs is 1.