+++
Description = ""
Tags = []
date = "2015-10-11T10:36:21+05:30"
title = "Registering and uploading python package to pip"

+++

Packaging and distributing in python is a bit difficult. [PyPI](https://pypi.python.org/), the python package index is the most common
one. I will explain step by step, how to upload your package to PyPI.
<!--more-->
## Setup file
The first thing is to create a setup.py file in the root of your project.

    from setuptools import setup
    
    setup(
        name = "project",
        version = "0.1.0",
        author = "author name",
        author_email = "author@gmail.com",
        description = ("Description of the package"),
        license = "MIT",
        keywords = "keywords",
        url = "http://url-of-project/",
        packages = ['packages'],
        long_description = "Provide a detailed explanation of your "\
                           "project and how it can be used",
        install_requires = [
            "click>=5.0",
            "pygments==2.0.2"
        ],
        entry_points = {
            'console_scripts': [
                'project=project:main'
            ],
        }
    )
    

This is a template for setup.py. Just fill in your information like name, email, project description etc.
Mention all your project dependencies in ```install_requires``` section. You also have to provide the entry_point
for your program. You can do that with ```entry_points``` section.

```project=project.main```
project is the name of the binary. Project in ```project.main``` is the package name and ```main``` is the name of
the function to be called. So this will be the first function, that python calls.

## Test the package locally

Once you are done with setup, we can generate the distributable file.

```$ python setup.py sdist```

This command generates the distributable zip file inside ```dist/``` directory.

Extract the zip file, that is present in dist directory and cd into it.

```$ python setup.py install```

This command installs your package locally. You can do this to verify if your setup is correct or not. If setup.py is 
correct then the local install works fine.

## Register with PyPI

To upload packages to PyPI, you have to be registered with them. Its simple, you can do it from the command line itself.
Run the below command.

```$ python setup.py register```

Here you can register as a new user. Provide your username, email and password and you're done. In this step, you will
register yourself and not the package.

Next step is to register your package.

```$ python setup.py register```

This time, choose login as existing user. If you get Server response (200): OK, then your project is registered succesfully.
Sometimes you might get the below error.

```Registering lookup to http://pypi.python.org/pypi
Server response (403): You are not allowed to store 'project' package information```

Check if there is any existing project in PyPI with the same name as your project.
If yes, then you have to rename your project. After making the necessary changes register again (Run the command again).

## Upload

Before running the upload command, you have to create ```~/.pypirc``` file

Contents of .pypirc

    [server-login]
    username:username
    password:password<Plain text>

After creating the file, run the following command

```$ python setup.py sdist upload```

This will generate the distributable and uploads it to PyPI. If everything goes well you will see 
```Server response (200): OK``` on your terminal. 

You have successfully uploaded your package to PyPI. Next time you can skip the user registration. No need to register
again, you can use your existing credentials.