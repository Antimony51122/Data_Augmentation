<h1 align="center">
	Boost Python Wrapper
</h1>

After developed my part of the algorithm, I have to make the algorithms produced portable for all other colleagues to utilize by wrapping into libraries functions and output to other languages (normally python). I achieved by learning the Boost.Python package which can enable using simple python lines to call very complicated C++ functions. Furthermore, through learning the package, I got an insight of how industry standard programming manages and coordinate portability and the finishing of one’s work. 

<img src="cpp_vs_versus_py_pycharm.jpg">

The Boost Python Library is a framework for interfacing Python and C++. It allows you to quickly and seamlessly expose C++ classes functions and objects to Python, and vice-versa, using no special tools -- just your C++ compiler. It is designed to wrap C++ interfaces non-intrusively, so that you should not have to change the C++ code at all in order to wrap it, making Boost.Python ideal for exposing 3rd-party libraries to Python. The library's use of advanced metaprogramming techniques simplifies its syntax for users, so that wrapping code takes on the look of a kind of declarative interface definition language (IDL).

### Sample: Hello World

Following C/C++ tradition, let's start with the "hello, world". A C++ Function:

```C++
char const* greet()
{
   return "hello, world";
}
```

can be exposed to Python by writing a Boost.Python wrapper:

```C++
#include <boost/python.hpp>

BOOST_PYTHON_MODULE(hello_ext)
{
    using namespace boost::python;
    def("greet", greet);
}
```

We can now build this as a shared library. The resulting DLL is now visible to Python. Here's a sample Python session:

```bash
>>> import hello_ext
>>> print hello_ext.greet()
hello, world
```