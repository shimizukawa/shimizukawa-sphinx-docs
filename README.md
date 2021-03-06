# sphinx-doc.org on the Read The Docs.

This is a experimental project to provide Sphinx official documentation with multiple versions and multiple languages on Read The Docs site.

http://shimizukawa-sphinx.readthedocs.org/

Current procedure is bit tricky because Read The Docs doesn't have a way to specify options for sphinx-build command.
conf.py files for each languages have 'language' and 'locale_dirs' values without having full copy of conf.py of sphinx doc. If we want to specify conf.py file that is out of source directory, we will use '-c' option for sphinx-build command. Unfortunately Read the Docs can't. If there are any better way, please let me know.


## How to update po files

```
cd locale
pip install -r requirements.txt
sphinx-intl create-transifexrc  # to setup transifex account
sh update.sh  # to update po files for each languages
```

After that, you should commit updated po files.


## How to add a language

1. add language to locale/update.sh:

   ```
   - rm -R es ja
   - tx pull -l es,ja
   + rm -R es ja pt_BR
   + tx pull -l es,ja,pt_BR
   ```

2. update po files

3. add language directory in languages/ as:

   ```
   cd languages
   mkdir pt_BR
   cp ja/conf.py pt_BR/conf.py
   vi pt_BR/conf.py  # to set 'pt_BR' to 'language' conf value
   ```

4. commit them

5. add new project on Read The Docs like:

   https://readthedocs.org/projects/shimizukawa-sphinx-pt-br/

6. add translation project to parent project like:

   https://readthedocs.org/dashboard/shimizukawa-sphinx-ja/translations/


## How to add a version

1. branch from master:

   ```
   git checkout -b 1.3
   ```

2. update submodule:

   ```
   cd sphinx
   git checkout 1.3.3
   ```

3. commit it:

   ```
   cd ..
   git add sphinx
   git commit -m "use sphinx-1.3.3"
   ```

4. update po files

5. enable version 1.3 on RTD:

   https://readthedocs.org/dashboard/shimizukawa-sphinx-ja/versions/

