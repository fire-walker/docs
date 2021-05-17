## Extract Requirements

``` py3
# install tool
python3 -m pip install -U pipreqs

# go to script folder
pipreqs .
```

## List Deduplication

``` py
>>> some = [1, 2, 3, 4]
>>> filtered = set(some)
>>> filtered
# {1, 2, 3}
```

## Zero Padding

``` py
>>> i = 2
>>> padded = f"Three {i:03d}"
>>> padded = '{foo:03d}'.format(foo=i)
>>> padded
# Three 002
```