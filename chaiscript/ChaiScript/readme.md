Master Status: [![Build Status](https://travis-ci.org/ChaiScript/ChaiScript.png?branch=master)](https://travis-ci.org/ChaiScript/ChaiScript) [![Coverage Status](https://coveralls.io/repos/ChaiScript/ChaiScript/badge.png?branch=master)](https://coveralls.io/r/ChaiScript/ChaiScript?branch=master)

Develop Status: [![Build Status](https://travis-ci.org/ChaiScript/ChaiScript.png?branch=develop)](https://travis-ci.org/ChaiScript/ChaiScript) [![Coverage Status](https://coveralls.io/repos/ChaiScript/ChaiScript/badge.png?branch=develop)](https://coveralls.io/r/ChaiScript/ChaiScript?branch=develop)


ChaiScript

http://www.chaiscript.com

(c) 2009-2012 Jonathan Turner
(c) 2009-2015 Jason Turner

Release under the BSD license, see "license.txt" for details.


Introduction
============

[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/ChaiScript/ChaiScript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

ChaiScript is one of the only embedded scripting language designed from the 
ground up to directly target C++ and take advantage of modern C++ development 
techniques, working with the developer like he expects it to work.  Being a 
native C++ application, it has some advantages over existing embedded scripting 
languages:

1. It uses a header-only approach, which makes it easy to integrate with 
   existing projects.
2. It maintains type safety between your C++ application and the user scripts.
3. It supports a variety of C++ techniques including callbacks, overloaded 
   functions, class methods, and stl containers.


Requirements
============

ChaiScript requires a C++11 compiler to build with support for variadic 
templates.  It has been tested with gcc 4.6 and clang 3.1 (with libcxx). MacOS 
10.8 (Mountain Lion) is also known to support the C++11 build with Apple's 
clang 4.0. MSVC 2013 or newer is supports also. For more information see the build 
[dashboard](http://chaiscript.com/ChaiScript-BuildResults/index.html).

Usage
=====

* Add the ChaiScript include directory to your project's header search path
* Add `#include <chaiscript/chaiscript.hpp>` to your source file
* Instantiate the ChaiScript engine in your application.  For example, create a 
  new engine with the name `chai` like so: `chaiscript::ChaiScript chai`
* The default behavior is to load the ChaiScript standard library from a 
  loadable module. A second option is to compile the library into your code,
  see below for an example.

Once instantiated, the engine is ready to start running ChaiScript source.  You
have two main options for processing ChaiScript source: a line at a time using 
`chai.eval(string)` and a file at a time using `chai.eval_file(fname)`

To make functions in your C++ code visible to scripts, they must be registered 
with the scripting engine.  To do so, call add:

    chai.add(chaiscript::fun(&my_function), "my_function_name");

Once registered the function will be visible to scripts as "my_function_name"

Examples
========

ChaiScript is similar to ECMAScript (aka JavaScript(tm)), but with some 
modifications to make it easier to use.  For usage examples see the "samples" 
directory, and for more in-depth look at the language, the unit tests in the 
"unittests" directory cover the most ground.

For examples of how to register parts of your C++ application, see 
"example.cpp" in the "src" directory. Example.cpp is verbose and shows every 
possible way of working with the library. For further documentation generate 
the doxygen documentation in the build folder or see the website 
http://www.chaiscript.com.


The shortest complete example possible follows:

    /// main.cpp

    #include <chaiscript/chaiscript.hpp>

    double function(int i, double j)
    {
      return i * j;
    }

    int main()
    {
      chaiscript::ChaiScript chai;
      chai.add(chaiscript::fun(&function), "function");

      double d = chai.eval<double>("function(3, 4.75);");
    }



Or, if you want to compile the std lib into your code, which reduces
runtime requirements.

    /// main.cpp

    #include <chaiscript/chaiscript.hpp>
    #include <chaiscript/chaiscript_stdlib.hpp>

    double function(int i, double j)
    {
      return i * j;
    }

    int main()
    {
      chaiscript::ChaiScript chai(chaiscript::Std_Lib::library());
      chai.add(chaiscript::fun(&function), "function");

      double d = chai.eval<double>("function(3, 4.75);");
    }


