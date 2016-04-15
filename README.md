# Translation with Transifex

Translation for the content as code workflow using Jekyll or another static website generator.

 - Markdown + YAML Frontmatter directory structure.
 - Transifex account.

## Example usage

### 0. Setup

> Define source language and target languages (Done via `tx config`)

 - `en` is the source language (and is assumed to be in `_posts/en`)
 - `fr` and `cn` are the target languages (after step #4 the translations will be in `_posts/fr` and `_posts/cn`)

### 1. Preparation of Translation Files

> This will parse the Markdown posts and prepare a JSON Key Value pair file for transifex.

 - `tx-before-send` 
     + Converts Markdown files (in `_posts/en`)
     + Creates (or updates) source language files (`_locales/en.json`)

### 2. Sending it for translation

> This will upload the prepared source language file to transifex where it can be translated. If this is an update, transifex will show which strings need translation (and adjust the percentage of untranslated strings).

 - Manually (without txgh)
     + `tx push`
     + `git commit -ma "Updated source translation files"`
     + `git push`

 - Automatic (with txgh)
     + `git commit -ma "something"` 
     + `git push`
         * This will trigger txgh which will run `tx push`.

### 3. Getting in new or updated translations

> Translators work on Transifex to create or update translations

 - Manually (without txgh)
     + `tx pull` gets the `_locales/fr.json` and `_locales/cn.json` files from transifex. 

### 4. Update the translated Markdown

> The transifex language files are processed to be available in the site. 

 - `tx-after-receive`
     + Converts target language files (`_locales/fr.json` and `_locales/cn.json`)
     + Creates (or updates) Markdown files (in `_posts/fr` and `_posts/cn`)

### 5. Publish

> After the translations have been created or updated the website can be generated and deployed.

 - `jekyll server` or `jekyll build` should generate a site that can then be deployed.

## Requirements

 - jq
 - tx (Transifex CLI)
 - Python 2.7
 - node 
 - yamljs 0.2.6 (`npm install yamljs`)
 - (Optional) txgh: which will simplify the workflow but requires a server deployment.

## Currently

 - `_locales/lib/tx_md2jsonKV.py` 
     + `_posts` as a source
     + `_locales` as a destination

# TODO

 - Create a `fixture-jekyll-transifex` 
 - Implement Pages translation
 - Document the more complex PanicButton workflow. (It also involves building the Android app)
 - Document the more general case using `docsmith`
 - Apply this to the `contentascode` website.
 - Apply this to the `open-mentoring` project.

# notes

 - Jekyll Multilingual.
     + [multilingual jekyll/prose approach](https://github.com/contentascode/multilingual) is used.
     + Good things to take from here: [Octopress Multilingual](https://github.com/octopress/multilingual)
 + Using `_posts/en` as the directory for the source language means that:
     + With a static site, it will need a JS redirect.
     + In a Content as Code context, this could be turned into a server side redirect.
