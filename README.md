# Crossplane-examples

Document crossplane examples for various use cases

## Setting up mkdocs

1 Install `virtualenv` Python module

The setup is using `virtualenv` that has built-in support for Nushell by using its [overlay feature](https://www.nushell.sh/book/overlays.html#overlays).


```sh
 ~/Some/path> python3 -m pip install virtualenv 
 ~/Some/path> python3 -m virtualenv .venv
 ~/Some/path> overlay use .venv/bin/activate.nu
(.venv) ~/Some/path>
```

2 Install Python requirements 

```sh
(.venv) ~/Some/path> pip install -r requirements.txts
```

## Kubernetes provider

Explore Cross plane concepts with the simple Kubernetes provider which does not have any requirements on integration with 3rd party platforms. All the concepts explored in this example could be applied to any other provider.

