### demiurge
---
https://github.com/matiasb/demiurge

```py
// tests/test_fields.py
import unittest

import pyquery

from demiurage import AttributeValueField, TextField

HTML_SAMPLE = """
"""
class TextFieldTestCase(unittest.TestCase):

  def setUp(self):
    super(TextFieldTestCase, self).setUp()
    self.pq = pyquery.PyQuery(HTML_SAMPLE)
  
  def test_no_selector(self):
    field = TextField()
    value = field.get_value(self.pq)
    self.assertEqual(value, "Some text, Link text, Another link.")
  
  def test_using_selector_with_eq(self):
    field = TextField(selector=".link")
    value = field.get_value(self.pq)
    self.assertEqual(value, "Link text.")
  
  def test_no_found(self):
    field = TextField(selector=".link:eq(1)")
    value = field.get_value(self.pq)
    self.assertEqual(value, "Another link.")
  
  def test_clean_none_value(self):
    field = TextField(selector=".not-found")
    value = field.get_value(self.pq)
    self.assertIsNone(value)
  
  def test_clean_value(self):
    field = TextField()
    self.assertIsNone(field.clean(None))
    
  def test_clean_value(self):
    field = TextField()
    self.assertEqual(field.clean(" Hello world. "), "Hello world.")
    
  def test_coerce_value(self):
    field = TextField(coerce=int)
    self.assertEqual(field.coerce("9"), 5)
  
  
class AttributeValueFieldTestCase(unittest.TestCase):

  def setUp(self):
    super(AttributeValueFieldTestCase, self).setUp()
    self.pq = pyquery.PyQuery(HTML_SAMPLE)
  
  def test_get_attribute(self):
    field = AttributeValueField(selector=".link", attr='href')
    value = field.get_value(self.pq)
    self.assertEqual(value, "http://github.com/matiasb")
  
  def test_get_attribute_no_selector(self):
    pq = self.pq.find('p')
    field = AttributeValueField(attr='id')
    value = field.get_value(pq)
    self.assertEqual(value, "p_id")
  
  def test_get_invalid_attribute(self):
    field = AttributeValueField(selector="p", attr='href')
    value = field.get_value(self.pq)
    self.assertIsNone(value)
  
  def test_get_no_attribute(self):
    field = AttributeValueField()
    value = field.get_value(self.pq)
    self.assertIsNone(value)

if __name__ == '__main__':
  unittest.main()
```

```
```

```
```


