reveal.js
=========

-  `reveal.js <https://github.com/hakimel/reveal.js/>`__
-  `Configuration <https://github.com/hakimel/reveal.js/#configuration>`__

Installation (Full setup)
-------------------------

-  Install Node.js (LTS Version): ``nvm install --lts``
-  Fix security problems: ``npm audit fix``
-  Change to reveal.js directory
-  Install via npm: ``npm install``
-  run on port 8000: ``npm start``
-  tun on other port: ``npm start -- --port=8001``

Using external Markdown file
----------------------------

Place this as a ``section`` block. This needs reveal.js to run in a
Node.js server. A local file is not sufficient.

::

   <section data-markdown="example.md"
            data-separator="^\n\n\n"
            data-separator-vertical="^\n\n"
            data-separator-notes="^Note:"
            data-charset="iso-8859-15">
       <!--
           Note that Windows uses `\r\n` instead of `\n` as its linefeed character.
           For a regex that supports all operating systems, use `\r?\n` instead of `\n`.
       -->
   </section>

Keyboard Shortcuts
------------------

-  ``N``, ``SPACE``: Next slide
-  ``P``: Previous slide
-  ``←``, ``H``: Navigate left
-  ``→``, ``L``: Navigate right
-  ``↑``, ``K``: Navigate up
-  ``↓``, ``J``: Navigate down
-  ``Home``: First slide
-  ``End``: Last slide
-  ``B``, ``.``: Pause (Blackout)
-  ``F``: Fullscreen
-  ``ESC``, ``O``: Slide overview / Escape from full-screen
-  ``S``: Speaker notes view
-  ``?``: Show keyboard shortcuts
-  ``alt`` + ``click``: Zoom in. Repeat to zoom back out.
