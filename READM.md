# no _runamel_ymal.pyx issue for ruamel_yaml==0.15.80 library

1. Copy [_ruamel_yaml.h](https://raw.githubusercontent.com/ruamel/yaml.clib/0.2.6/_ruamel_yaml.h) to this folder
2. change code in setup.py

```python
extensions = [Extension("ruamel_yaml.ext._ruamel_yaml",
                        ['ruamel_yaml/ext/_ruamel_yaml.c'],
                        # ['ruamel_yaml/ext/_ruamel_yaml.pyx'],
                        libraries=['yaml'],
                        library_dirs=library_dirs,
                        include_dirs=[os.path.join(PREFIX, 'include')],
                        runtime_library_dirs=[] if sys.platform == 'win32' else library_dirs)]
```
