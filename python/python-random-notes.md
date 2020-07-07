# python random notes

```python
sys.version # check the python version
locals() # show current local symbol table
globals() # global symbol table

#python 3
import urllib.parse
# can't import urllib and then try to access urllib.parse
urllib.parse.quote_plus(s)

#python2
urllib.quote_plus(s)
```

DictWriter and imap

```python
from csv import DictWriter
from multiprocessing import Pool
import time

def process_line(line):
  # biz logic
  return {
    'name': name,
    'age': age,
  }

NUM_OF_WORKERS = 8

pool = Pool(NUM_OF_WORKERS)
with open('sample-url.csv', 'r') as f:
  headers = [
    'name',
    'age',
  ]

  with open('output.csv', 'w', newline ='') as output:
    writer = DictWriter(output, fieldnames = headers)
    writer = writeheader()
    
    s = time.time()
    # imap returns one result as the worker finishes one task
    # map returns all results when all tasks are done
    for result in pool.imap(process_line, f, NUM_OF_WORKERS):
      writer.writerow(result)
    e = time.time()
    rint(f'finished in {e-s} seconds.')
```

