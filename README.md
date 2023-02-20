<!-- markdownlint-disable MD029 -->
# Purpose of this repository

To fix no _runamel_ymal.pyx issue for ruamel_yaml==0.15.80 library.

When you run `pip install ruamel_yaml_conda==0.15.80` on conda 4.10.3 environment, the follow issue raised up:

```bash
(base) Orlando:~ llv23$ pip install ruamel-yaml-conda
Collecting ruamel-yaml-conda
  Using cached ruamel_yaml_conda-0.15.80.tar.gz (202 kB)
  Preparing metadata (setup.py) ... error
  error: subprocess-exited-with-error
  
  × python setup.py egg_info did not run successfully.
  │ exit code: 1
  ╰─> [12 lines of output]
      Traceback (most recent call last):
        File "<string>", line 2, in <module>
        File "<pip-setuptools-caller>", line 34, in <module>
        File "/private/var/folders/p8/91_v9_9d12q9wmlydb406rbr0000gn/T/pip-install-ya5i4iwu/ruamel-yaml-conda_2f6ae5c608de416d87e4dc7d6731b18b/setup.py", line 35, in <module>
          ext_modules=cythonize(extensions),
        File "/Users/llv23/opt/miniconda3/lib/python3.8/site-packages/Cython/Build/Dependencies.py", line 965, in cythonize
          module_list, module_metadata = create_extension_list(
        File "/Users/llv23/opt/miniconda3/lib/python3.8/site-packages/Cython/Build/Dependencies.py", line 815, in create_extension_list
          for file in nonempty(sorted(extended_iglob(filepattern)), "'%s' doesn't match any files" % filepattern):
        File "/Users/llv23/opt/miniconda3/lib/python3.8/site-packages/Cython/Build/Dependencies.py", line 114, in nonempty
          raise ValueError(error_msg)
      ValueError: 'ruamel_yaml/ext/_ruamel_yaml.pyx' doesn't match any files
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
error: metadata-generation-failed

× Encountered error while generating package metadata.
╰─> See above for output.

note: This is an issue with the package mentioned above, not pip.
hint: See above for details.
```

## Root cause

Miss the `ruamel_yaml/ext/_ruamel_yaml.pyx` file

## How to fix for source code

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

3. use source code install

```bash
python setup.py install
```

## How to install on your local

1. Download the release ruamel_yaml_conda-0.15.80.tar.gz to your local
2. Run `pip install ruamel_yaml_conda-0.15.80.tar.gz`

In case you only have `missing ruamel_yaml_conda` issue via `pip check` in conda 4.10.3, either of the aforementioned ways could work.
