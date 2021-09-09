# Translation guide

## Technical

The source of the documentation can be found on [github](https://github.com/interlay/interlay.github.io). To contribute a new translation of this documentation, please open a pull request there. In your fork, create a new folder inside `docs/` with the [ISO 639-1 language code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) as the name. For example, a Spanish translation would be placed in `docs/es`. Place a copy of all existing documentation (excluding translations) into the newly created folder. In linux, this can be achieved by running the following:

    cp -r \
        docs/about \
        docs/_navbar.md \
        docs/_sidebar.md \
        docs/translation \
        docs/_assets \
        docs/collator \
        docs/developers \
        docs/glossary \
        docs/kintsugi \
        docs/README.md \
        docs/start \
        docs/vault \
        <newly created folder>

Next, modify `docs/_sidebar.md` to add a link to the new translation. For example, for Spanish, add a new item to the `Translations` as such: `[Spanish](es/)`.

At this point, it is recommended to make an initial commit with the new files, such that reviewers (if any) are more easily able to check translations. After that, you are ready to start translating these new files. Different files have different priorities for translation. From high to low, these are: the `README` file, the `kinsugi` folder, the `start` folder, the `vault` folder and finally the remaining files. When translating the files, make sure to update any links to go to your translated page, by prepending e.g. `es/` to the link. If you do not do this, the link will go to the English page.

The documentation contains several charts and diagrams, but we currently unable to provide a way to edit these. It is being looked into, but for now the English images can be used.

To display the the modified documentation in the browser, you can use a serve the page on localhost using [docsify](https://docsify.js.org/#/quickstart).

## Translation guidelines

As Interlay will likely not be able to directly check the accuracy of your translation, you will be responsible for providing as accurate of a translation as possible. We ask you to not depend on Google Translate or other similar translation services, as these are not always accurate. If your target language has distinct formal and informal pronouns, preferably use formal ones. Likewise, prefer gender neutral pronouns if your language allows it.

The documentation is a technical writing. As such, it contains a lot of jargon, such as "blockchain". If the language you are translating to has no widely adopted equivalent, it may be appropriate to use the English term. Most importantly, try to be consistent in the used terminology.