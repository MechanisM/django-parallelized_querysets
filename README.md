# django-parallelized_querysets

Handle large Django QuerySets by spreading their execution on multiple cores
and keeping the memory usage low.


## Installation

    pip install django-parallelized_querysets


## Usage

### `parallelized_queryset(queryset, processes=None, function=None)`

Process the given `queryset` and return the result as a list.

**`proceses`**

Number of processes to create. Defaults to the number returned by
`multiprocessing.cpu_count()`.

**`function`**

Apply a function the each result. Does not apply any function by default.


> **Note**
> 
> Each time your function returns `None`, the value won't be in the resulting
> list.


> **Note**
> 
> The order in the QuerySet won't be respected!

#### Example

Return all the `Article` objects:

    >>> from parallelized_querysets import parallelized_queryset
    >>> qs = Article.objects.all()
    >>> parallelized_queryset(qs)

Add all `Article` objects to a Redis index (assuming `Article` has
a `append_to_redis` method):

    >>> from parallelized_querysets import parallelized_queryset
    >>> qs = Article.objects.all()
    >>> parallelized_queryset(qs, function=lambda x: x.append_to_redis())


Do the same but on 6 processes:

    >>> from parallelized_querysets import parallelized_queryset
    >>> qs = Article.objects.all()
    >>> parallelized_queryset(qs, processes=6,
                                  function=lambda x: x.append_to_redis())


### `parallelized_multiple_querysets(querysets, processes=None, function=None)`

Same as `parallelized_queryset` but `querysets` is a list of QuerySets.


## Testing

    ./tests/sample/manage.py test sample

## License

MIT (see LICENSE).
