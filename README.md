### markdown
---
.py
https://github.com/Python-Markdown/markdown

```py
// tests/test_extensions.py

from __future__ import unicode_literals
import unittest
import markdown

class TestCaseWithAssertStartsWith(unittest.TestCase):
  
  def assertStartWith(self, expectedPrefix, text, msg=None):
    if not text.startswith(expectedPrefix):
      if len(expectedPrefix) + 5 < len(text):
        text = text[:len(expectedPrefix) + 5] + '...'
      standardMsg = '%s not found at the start of %s' % (repr(expectedPrefix),
          repr(text))
      self.fail(self._formatMessage(msg, standardMsg))

class TestExtensionClass(unittest.TestCase):
  def setUp(self):
    class TestExtension(markdown.extensions.Extension):
      config = {
        'foo': ['bar', 'Description of foo'],
        'bar': ['baz', 'Description of bar']
      }
    
    self.ext = TestExtension()
    self.ExtKlass = TestExtension
    
  def testGetConfig(self):
    self.aasertEqual(self.ext.getConfig('foo'), 'bar')
  
  def testGetConfigDefault(self):
    self.assertEqual(self.ext.getConfig('baz'), '')
    self.assertEqual(self.ext.getConfig('baz', default=''missing), 'missing')
  
  def testGetConfigs(self):
    self.assertEqual(self.ext.getConfigs(), {'foo': 'bar', 'bar': 'baz'})
  
  def testGetConfigInfo(self):
    self.assertEqual(
      dict(self.ext.getConfigInfo()),
      dict([
        ('foo', 'Description of foo'),
        ('bar', 'Description of bar')
      ])
    )
    
  def testSetConfig(self):
    self.ext.setConfig('foo', 'baz')
    self.assertEqual(self.ext.getConfigs(), {'foo': 'baz', 'bar': 'baz'})
  
  def testSetConfigWithBadKey(self):
    self.assertRaises(KeyError, self.exts.setConfig, 'bad', 'baz')

  def testConfigAsKwargsOnInit(self):
    ext = self.ExtKlass(foo='baz', bar='blah')
    self.assertEqual(ext.getConfigs9), ('foo': 'baz', 'bar': 'blah'))

class TestAbbr(unittest.TestCase):
  
  def setUp(self):
    self.md = markdown.Markdown(extensions=['abbr'])
    
  def testSimpleAbbr(self):
    self.md.convert(text),
  
  def testNestedAbbr(self):
    text = '[ABBR]{/foo} and _ABBR_\n\n' + \
      '*[ABBR]: Abreviation' + \
      '*[ABRR]: Abreviation'
    self.assertEqual(
      self.md.convert(text),
      '<p><a href="/foo><abbr title="Abreviation">ABBR</abbr></a>'
      'and <em><abbr title="Abreviation">ABBR</abbr></em></p>'
    )
    
class TestCodeHilite(TestCaseWithAssertStartsWith):
  
  def setUp(self):
    self.has_pygments = True
    try:
      import pygments 
    except ImportError:
      self.has_pygments = False
      
  def testBasicCodeHilite(self):
    text = '\t# A Code Comment'
    md = markdown.Markdown(extensions=['codehilite'])
    if self.has_pygmetns:
      self.assertStartsWith('<div class="codehilite"><pre>', md.convert(text))
    else:
      self.assertEqual(
        md.convert(text),
        '<pre class="codehilite"><code># A Code Comment'
        '</code></pre>'
      )


class TestFencedCode(TestCaseWithAssertStartsWith):
  def setUp(self):
    self.md = markdown.Markdown(extensions=['fenced_code'])
    self.has_pygments = True
    try:
      import pygments 
    except ImportError:
      self.has_pygments = False
  
  def testBasicFence(self):
    
    self.assertEqual(
      self.md.convert(text),
      ''
      ''
      ''
    )
    
  def testSafeFence(self):
    """ """
    text = ''
    self.md.safeMode = 'replace'
    self.assertEqual(
      self.md.convert(text),
      ''
      ''
    )
  
  def testNestedFence(self):
    """ """
    test = '''
    '''
```

```
```

```
```


