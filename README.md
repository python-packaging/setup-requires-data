# setup-requires-data

This repo contains extracted `setup_requires` for every package on pypi, in the format

```
{
  canonical_name: [setup_requirements, ...],
  ...
}
```

Last update: 2020-03-04

# why

There's a bootstrapping problem because many packages specify something in
`setup_requires` that's necessary to even run setup.py (for example, to import
`numpy.distutils` without any guards).

If guarded with `except ImportError: pass` then you could use something like
[run_setup](https://docs.python.org/3/distutils/apiref.html#distutils.core.run_setup)
but this will not work for many packages.

# what is `??`

```
{
  "pkg1": ["setuptools", "??"],
  "pkg2": ["??"],
}
```

This is used anywhere the structure was too complex to infer.  It might not be
the only value, and might stand for more than one value.

Concatenation, function calls, imports, etc are not currently handled.  If you
see a `??` it means a human should review.

# technical details

I used `opine.metadata` (from
[`tmp-setup-py-parsing`](https://github.com/python-packaging/opine/tree/tmp-setup-py-parsing)
branch), which uses `libcst` to walk the nodes in setup.py to find likely
calls, and convert that to json.

Right now it only stores values that are defined in super-obvious ways.  For
conditionals, it generally choses the first one.

A future version may encode the cases that were true for conditionals, e.g.
comparison with `sys.version_info` could turn into markers.

# license

Short answer: I chose CC0

Longer answer:

I am not a lawyer.  I think the list of requirements count as facts in the
U.S., but at the same time aggregating metadata isn't specifically called out
as a use in the [pypi ToU](https://pypi.org/policy/terms-of-use/).

I am happy to exclude individual projects if you object via github issue here.
