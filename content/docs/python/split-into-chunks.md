---
title: "Split into sized chunks"
---
```python
def split_into_chunks(input_list, chunk_size):
    return [
        input_list[i : i + chunk_size]
        for i in range(0, len(input_list), chunk_size)
    ]
```