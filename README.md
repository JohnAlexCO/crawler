# crawler
A simple python library for rapid website development, designed primarily for use with cPanel's `wsgi` wrapper.

#### Day-By-Day

###### Day 0, missing timestamps
Somewhere in the ballpark of 2 to 5 hours put into test 1. Got `environ` and `start_response` dumps.

###### Day 1, October 25 2021, about 4 hours
Did multiple tests, and are capable of serving plain test, and probably HTML too, using basic functions. With very little tweaking, this could serve a static or even partially dynamic website such as a user counter. However, I don't know exactly how stable it is.
With this code in our `passenger_wsgi.py` file --
```python
from crawler import *

@route
def index():
	return 'Hello world'

application = app
```
-- we are able to host a page with simple text, and by just adding
```python
@route
def main():
  return 'Aha! You found main()'
```
we are able to have another page hosted at `./main`, and the first def will catch any `404` that's local.

##### Day 2, November 2 2021, about 3 hours
Re-designed from scratch, unable to get static files to work but can stably serve HTML and some types of other content with proper `mimetype`s. Declaring a page with multiple names now might look like
```python
@route
def page(data):
    return 'This page was called by the name %s' % data['uri']
addnames(page, '/page /of /many /names /page/of /pagenames')
```
and changing the `mimetype` of a file looks like
```python
filetype(page, 'svg')
```

##### NOTE:
I do not know how much development happened (if any) between these two points because of poor notetaking between 0.0.4 and 0.0.05a

##### Day "3", March 23 2022, 50 minutes
Re-re-designed from scratch. Is a lot prettier, but does not support filetype definitions. This can be re-added later, but this massive update was done to allow building [Friendly](https://github.com/JohnAlexCO/Friendly). The same example from before looks like
```python
from crawler import *

def page(request):
    return 'This page was called by the name %s' % request.request

Route(page, [
    '/page',
    '/of',
    '/many',
    '/names'
])
```
And we can over-write the now-added default page by saying
```python
Route.default = Main
```
