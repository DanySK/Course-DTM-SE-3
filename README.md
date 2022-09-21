# How to build

This repository requires a pre-processing step to generate the markdown files that in turn generate the slides.

Clone with submodules:

``git clone --recurse-submodules git@github.com:DanySK/Course-DTM-SE-3.git``

Run the pre-processor (requires Ruby)

``./shared-slides/preprocess.rb``

Launch Hugo to build: ``hugo``

Or keep Hugo running for having the slides served locally ``hugo server``.

For development, it is recommended to keep the generator running listening to changes:

``trap '[ -z $! ] || kill $!' SIGHUP SIGINT SIGQUIT SIGTERM && hugo server & while [ -e /proc/$! ]; do inotifywait -e modify,create,delete -r content shared-slides && shared-slides/preprocess.rb; done &``

and then terminate with ``killall hugo``
