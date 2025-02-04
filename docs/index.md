<p align="center" style="margin: 0 0 10px">
  <img width="75" src="img/book-open-variant.svg" alt='django-readers'>
</p>

<h1 align="center" style="font-size: 2.5rem; margin: -15px 0">django-readers</h1>

---

<div align="center">
  <p>
    <a href="https://github.com/dabapps/django-readers/actions">
      <img src="https://github.com/dabapps/django-readers/workflows/CI/badge.svg?branch=main" alt="Test Suite">
    </a>
    <a href="https://pypi.org/project/django-readers/">
      <img src="https://badge.fury.io/py/django-readers.svg" alt="Package version">
    </a>
  </p>
</div>

_A lightweight function-oriented toolkit for better organisation of business logic and efficient selection and projection of data in Django projects._

Install from [PyPI](https://pypi.org/project/django-readers/):

```shell
pip install django-readers
```

!!! important "A word from our sponsor"
    `django-readers` is developed and supported by [DabApps](https://www.dabapps.com), a UK-based web and mobile agency. We design and build bespoke software that helps our clients succeed. We ❤️ Django and use it for all server-side code. And [we're hiring](https://www.dabapps.com/careers/)!

## What is django-readers?

`django-readers` is both a **small library** (less than 500 lines of Python) and a **collection of recommended patterns** for structuring your code. It is intended to help with code that performs _reads_: querying your database and presenting the data to the user. It can be used with views that render HTML templates as well as [Django REST framework](https://www.django-rest-framework.org/) API views, and indeed anywhere else in your project where data is retrieved from the database.

It lets you:

* Query your database more efficiently, by specifying precisely which fields and relationships you need to load. It can help eliminate [the N+1 queries problem](https://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem-in-orm-object-relational-mapping).
* Create [Django REST framework](https://www.django-rest-framework.org/) API endpoints using a declarative specification of their data shape, without needing to write serializer classes.
* Optionally, structure your business logic code following function-oriented idioms that may result in a more maintainable, testable codebase and helps prevent "fat models" turning into [big balls of mud](https://en.wikipedia.org/wiki/Big_ball_of_mud).

`django-readers` is intended to feel like Django. You can use as much or as little of the library or the patterns as you like, and mix it in with an existing project. It doesn't attempt to hide away any parts of Django itself, and it introduces only a few straightforward concepts.

## Show me some code

`django-readers` lets you write Django views like this:

```python
def author_list(request):
    spec = [
        "id",
        "name",
        {
            "book_set": [
                "id",
                "title",
                "publication_date",
            ]
        },
    ]

    prepare, project = specs.process(spec)
    queryset = prepare(Author.objects.all())
    return render(
        request,
        "author_list.html",
        {"authors": [project(instance) for instance in queryset]},
    )
```

And [Django REST framework](https://www.django-rest-framework.org/) views like this:

```python
class AuthorListView(SpecMixin, ListAPIView):
    queryset = Author.objects.all()
    spec = [
        "id",
        "name",
        {
            "book_set": [
                "id",
                "title",
                "publication_date",
            ]
        },
    ]
```
