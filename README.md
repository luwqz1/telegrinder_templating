# telegrinder templating tool

This module is intended for the Telegrinder framework. It allows for text templating.

Install using PyPI:

`pip install telegrinder_templating`


# Documentation

There is templating in order to avoid using large text. Module `telegrinder_templating` has an `ABCTemplating` interface for creating templating classes.

```python
from telegrinder_templating import ABCTemplating
```

telegrinder_templating has class `JinjaTemplating` to work with jinja templates.

```python
from telegrinder_templating import JinjaTemplating
```

`JinjaTemplating` methods:
* `render(template_name: str, **data: Any) -> str`
* `render_from_string(template_source: str, **data: Any) -> str`
* `@add_filter(key: str | None) -> wrapper(func: JinjaFilter)`


Example:
```python
import asyncio
import pathlib

import jinja2

from telegrinder_templating import JinjaTemplating

jt = JinjaTemplating(pathlib.Path(__file__).resolve().parent / "templates")


@jt.add_filter("title")
def title_string(value: str, must_strip: bool = False) -> str:
    if must_strip:
        return value.strip().title()
    return value.title()


async def main():
    await jt.render("template.j2", digits=[1, 2, 3])
    await jt.render_from_string("{{ name|title(must_strip=True) }}", name="  alex")


asyncio.run(main())
```

Usage example [*CLICK*](https://github.com/timoniq/telegrinder/blob/dev/examples/templating.py)

More details about `Jinja` can be found [*HERE*](https://jinja.palletsprojects.com/en)
