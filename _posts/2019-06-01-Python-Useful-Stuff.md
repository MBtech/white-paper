---
layout: post
title: "Python Gems"
categories:
  - guide
tags:
  - guide
---
Get URL encoding from String:
```python
import urllib
urllib.pathname2url(stringToURLEncode)
```
or
```python
import urllib
urllib.quote_plus(stringToURLEncode)
```

BeautifulSoup multi-class selector:
```python
import urllib2
from bs4 import BeautifulSoup
quote_page = ‘http://www.bloomberg.com/quote/SPX:IND'
page = urllib2.urlopen(quote_page)
soup = BeautifulSoup(page, 'html.parser')
soup.select('div.A.B')
```
or you can find an element using
```Python
name_box = soup.find(‘h1’, attrs={‘class’: ‘name’})
```

Python datetime:

datetime to string
```python
from datetime import datetime
now = datetime.now()
date_time = now.strftime("%m/%d/%Y, %H:%M:%S")
```

String to datetime
```python
from datetime import datetime
datetime_str = '09/19/18 13:55:26'
datetime_object = datetime.strptime(datetime_str, '%m/%d/%y %H:%M:%S')
```

Check if a file exists
```python
import os.path
os.path.exists('mydirectory/myfile.txt')
```

Check if a file is empty
```python
import os
os.stat("file").st_size == 0
```

Getting boolean as input argument
```python
flag = sys.argv[1].lower() == 'true'
```

Start an process and terminate it is necessary
```python
import multiprocessing, time
p = multiprocessing.Process(target=time.sleep, args=(1000,))
while p.is_alive():
  # Do some stuff
  #if a termination condition is reached
  p.terminate()
  break
```

Multiprocessing with callbacks
```python
from multiprocessing import Pool
def callback_fun():
    return "Finished"
    
def f(x):
    return x*x

if __name__ == '__main__':
    pool = Pool(processes=1)
    result = pool.apply_async(f, [10], callback=callback_fun)
```

**Converting cURL commands to Python requests**

You can use the following site to immediately convert the copied cURL from your browser's dev tools into a python program:
https://curl.trillworks.com/#

**References:**
- https://stackoverflow.com/questions/5607551/how-to-urlencode-a-querystring-in-python
- https://stackoverflow.com/questions/40305678/beautifulsoup-multiple-class-selector/40305745
