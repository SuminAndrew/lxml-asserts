## lxml-asserts

Handy functions for testing [lxml](http://lxml.de/) etree objects for equality and compatibility.

[![Build Status](https://travis-ci.org/SuminAndrew/lxml-asserts.svg?branch=master)](https://travis-ci.org/SuminAndrew/lxml-asserts)
[![codecov](https://codecov.io/gh/SuminAndrew/lxml-asserts/branch/master/graph/badge.svg)](https://codecov.io/gh/SuminAndrew/lxml-asserts)


### Quick overview

Testing for equality is pretty straightforward:

```python
from lxml_asserts import assert assert_xml_equal

# This raises AssertionError
assert_xml_equal('<a x="1"><b/></a>', '<a y="2"><c/></a>')

# That's ok
assert_xml_equal('<a x="1" y="2"><b/><c/></a>', '<a y="2" x="1"><c/><b/></a>')

# Tags order can matter if we'd like it
# This code raises AssertionError
assert_xml_equal('<a x="1" y="2"><b/><c/></a>', '<a y="2" x="1"><c/><b/></a>', check_tags_order=True)
```

One xml is compatible with the other if removing a certain set of attributes and children from the latter
can make them equal (up to the order of the tags). In other words to be compatible a tree must be an extension
of another tree.

```python
from lxml_asserts import assert assert_xml_compatible

# Second xml is an extension of the first
assert_xml_equal('<a><b/></a>', '<a y="2"><b/><c/></a>')

# But in this case it isn't
# The code raises AssertionError
assert_xml_equal('<a><b x="1"/><b y="2"/></a>', '<a><b x="1" y="2"/><b/></a>')
```

There is also a `unittest.TestCase` mixin:

```python
import unittest

from lxml_asserts.testcase import LxmlTestCaseMixin

class TestXmlEqual(unittest.TestCase, LxmlTestCaseMixin):
    def test(self):
        self.assertXmlEqual('<a><b/></a>', '<a><b/></a>')
        self.assertXmlCompatible('<a><b/></a>', '<a><b x="1"/></a>')
```

For more information, see docstrings.
