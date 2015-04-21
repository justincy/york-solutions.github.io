At Find-A-Record we are preparing to support multiple languages.[^1] This is a
tremendous amount of work. Translating all the content is the easy part. What's
hard is refactoring our architecture to support multiple languages. We're starting
with the open source libraries we've developed for the research assistant:
[fs-check][fs-check][^2], [fs-traversal][fs-traversal], and 
[fs-traversal-relationship-display][fs-traversal-relationship-display]. All three
libraries have display components which need to be translated and therefore
need to be refactored to support multiple languages.

While we're in the code to perform significant refactors, we are also adding other
major features and fixing bugs and updating Find-A-Record with the new versions
as we go. Thanks to [{TrackJS}][track-js] we are quickly notified of errors.[^3]

[fs-traversal]: https://github.com/genealogysystems/fs-traversal
[fs-check]: https://github.com/genealogysystems/fs-check
[fs-traversal-relationship-display]: https://github.com/genealogysystems/fs-traversal-relationship-display
[track-js]: /2015/04/16/javascript-error-logging-trackjs.html

[^1]: Spanish will be the first language we add support for.
[^2]: In a previous blog post we mused about how to [manage multiple languages](/2015/04/02/managing-language-packs-jacascript-lib.html).
[^3]: Yes, we do testing before pushing updates live but we don't catch everything.