# Google vs reST docstrings

In Python, both Google-style and reStructuredText (reST) docstrings are popular for documenting code. The choice between them often depends on personal or team preferences, the tools you're using, and the project's requirements. Here's a brief overview and examples of both:

## Google-style Docstrings

Google-style docstrings are more human-readable and easier to write. They tend to be less verbose and use indentation to separate sections.

**Example**:

```python

def function(arg1, arg2):
"""
Description of what the function does.

    Args:
        arg1: Description of arg1
        arg2: Description of arg2

    Returns:
        bool: Description of the return value

    Raises:
        ValueError: Description of raised error
    """
```

## reStructuredText (reST) Docstrings

reST docstrings are part of the reStructuredText markup language and are used by Sphinx, a powerful documentation generator. They are more structured and can be more verbose but are highly flexible and powerful for generating detailed documentation.

**Example**:

```python

def function(arg1, arg2):
"""
Description of what the function does.

    :param arg1: Description of arg1
    :type arg1: int
    :param arg2: Description of arg2
    :type arg2: str
    :returns: Description of the return value
    :rtype: bool
    """
```

### Choosing Between Them

- **Google-style** is often preferred for its simplicity and readability, making it a good choice for smaller projects or teams that prioritize quick and easy documentation.

- **reST** style is more suitable for larger projects that require detailed documentation, especially if you're using Sphinx or planning to generate HTML documentation.

Ultimately, the best style is the one that aligns with your project needs and your team's preferences. Consistency within a project is key, so once you choose a style, it's best to stick with it throughout the project.
