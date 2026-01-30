Sphinx internationalization
===========================

This repository contains a basic Sphinx project and instructions for facilitating
your first translation.

Before getting started, we'll need to set up a Python virtual environment with the
necessary dependencies:

```
python3 -m venv .venv
source .venv/bin/activate
pip install sphinx
```

Sphinx uses the [GNU gettext
utilities](https://www.gnu.org/software/gettext/manual/gettext.html) to facilitate the
translation of entire documents. You'll need to install `gettext` on our local machine.
On Ubuntu, this can be done with:

```
sudo apt install gettext
```

Let's extract all of the translatable strings, or _messages_, in our project with:

```
sphinx-build -M gettext src/ build/
```

Each file's messages are extracted into `build/gettext/` as `.pot` files. These `.pot`
files, known as _catalog templates_, only contain the messages in the original language.
Each catalog templates will need to be translated and converted to ``.po`` files, or
_message templates_. Let's do a simple translation. Copy ``build/gettext/index.pot``
to the root of the project.

```
cp build/gettext/index.pot index.po
```

Now, open `index.po` in your preferred text editor so we can convert it to a proper
message template.

Normally, a translator would update all the metadata at the top of the page, but we're
going to skip that for our example. Navigate to the end of the file and note our four
message entries—one for each paragraph in the source. To make this a valid message
template, we need to add translations for each of these messages, in the desired
language.

Let's translate each message to French. Make the following changes to the file:

```diff
  #: ../../src/index.rst:3
  msgid "Example"
- msgstr ""
+ msgstr "Exemple"

  #: ../../src/index.rst:5
  msgid "pen"
- msgstr ""
+ msgstr "stylo"

  #: ../../src/index.rst:7
  msgid "house"
- msgstr ""
+ msgstr "maison"

  #: ../../src/index.rst:9
  msgid "shrimp"
- msgstr ""
+ msgstr "crevette"
```

We've now mapped each of the messages to its equivalent string in French. Now let's
apply our translation!

Using `msgfmt`, we'll need to compile our message catalog into a new locale directory.
This can be done with:

```
mkdir -p src/locales/fr/LC_MESSAGES/
msgfmt "index.po" -o "src/locales/fr/LC_MESSAGES/index.mo"
```

The only thing left to do is let Sphinx know where our `locale` directory is and
which language we want to build in. In our case, this is already done. Note the
following values in the `src/conf.py` file:

```
locale_dirs = ["locales/"]
language = "fr"
```

Finally, let's build our test project in both languages: once in English and once
in French.

```
sphinx-build -b html -D language='en' src/ build/en/
sphinx-build -b html -D language='fr' src/ build/fr/
```

You can also change the language by setting `language` in your `conf.py` file.

If you open `build/fr/index.html` in your browser, you should notice that our
text is now in French! 

