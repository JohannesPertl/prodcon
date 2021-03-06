# prodcon

An easy to use implementation of the producer consumer pattern with threading

## Installation
```shell script
pip install prodcon
```
## Example usage 
```python
import random
import time
from queue import Queue

from prodcon import Producer, Consumer


def generate_items(start, end):
    for i in range(start, end + 1):
        if i == end:
            # Add poison pill to stop producer and consumers
            yield None
        # Simulate producing time
        time.sleep(random.random())
        print(f'Produced item #{i}')
        yield f'Item #{i}'


def process_item(item):
    # Simulate processing time
    time.sleep(random.random() + 1)
    print(f'Processed {item}')



q = Queue()
p = Producer(q, generate_items, args=(1, 100))
c = Consumer(q, process_item)

p.start()
c.start()
```
## Example usage with decorators

```python
import random
import time

from prodcon import produces, consumes


@produces
def generate_items(start, end):
    for i in range(start, end + 1):
        if i == end:
            # Add poison pill to stop producer and consumers
            yield None
        # Simulate producing time
        time.sleep(random.random())
        print(f'Produced item #{i}')
        yield f'Item #{i}'


@consumes
def process_item(item):
    # Simulate processing time
    time.sleep(random.random() + 1)
    print(f'Processed {item}')


generate_items(1, 100)

# Use multiple consumers
for _ in range(5):
    process_item()
```