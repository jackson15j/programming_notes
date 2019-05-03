2018-08-18 PyData Meetup
========================

Data Visualisations
-------------------

* Margriet Groenendijk
    * IBM Developer Advocate.
    * Writes talks.
    * Same talk for Pycon UK.
    * [Github: MargrietGroenendijk].

* Libraries:
    * [Matplotlib].
    * [pandas].
    * [numpy].
    * [seaborn].
    * [bokeh].
    * [Github: Brunel-Visualization/Brunel].
    * [Github: altair-viz/pdvega].
    * [Github: jciskey/pygraph].
    * [Github: pixiedust/pixiedust].

### Pixie Dust

* Good integration with [Jupyter Notebook].
* Renders using some of the above libraries.
* Her IBM team that the others had flaws, so created their own package.
* Renders to charts/maps/globe.
    * Map/Globe rendering can parse country name/codes to auto pickup on the
      map.
    * Only library that displays to map/globe.
* Pixie App:
    * decorators to make your code interactive in your [Jupyter Notebook].
    * Think flask, but interactive buttons.
* Pixie Gateway
    * Publish your Pixie Apps.
* Pixie Dust
    * Open source.
    * request for love.
* Resources:
    * [O'Reilly: Thoughtful Data Science - David Taieb].
    * [Developer IBM: Data Science].

UK Python Association
---------------------

* Owen Campbell
    * Treasurer.
    * Pushing the [PSF (Python Software Foundation)].
* UKPA ([UK Python Assocation]) is a UK Charity for Promotion and to take over
  Pycon UK.
    * Future is to support meetups, groups.

Python in Brain Research
------------------------

* [CUBRIC (Cardiff Uni Brain Research Imaging Centre)].
    * 4x MRI's (Magnetic Resonance Imaging).
    * MEG (Magnetoencephalography) - Better.
    * EEG (Elextroencephalography) - OK.
    * Brain stimulation labs.
    * Cognitive labs.
    * Sleep labs.
* [PsychoPy] is an open-source application allowing you run a wide range of
  neuroscience, psychology and psychophysics experiments.
    * Based off [pygame]
    * [pavlovia] - JS platform to ryn [PsychoPy] online.
    * crossplatform.
    * Controls (physical stimuli):
        * joystick (xinput), buttons.
        * monitors.
        * audio.
        * eyetracking.
    * Physical tests for scientists to apply on subjects using physical
      stimuli.
* [Github: mne-tools]
    * Analysis of EEG/MEG data.
    * Requires pre-processing/filtering of data before analysis, or rejection
      of select trials.
    * Typically passed into [numpy]/[Matplotlib] for display, or displayed via
      [Github: mne-tools] plot functions.
    * Can display functions by brain region _"Wiesley ..."_ (!?) + their
      connections & strengths.
* [SciPy]
    * Stats package.
    * Used to weed out outliers (for rejection).
* Machine Learning in Python:
    * [pandas].
    * [PyMVPA].
    * [NiPy].
    * [scikit-learn].
    * [nilearn] - Neuro-Imaging learning.
    * [Github: mne-tools].
    * [Github: poldracklab/fmriprep] - A robust preprocessing pipeline for MRI
      data - also generates images.
    * [Github: voytekresearch/OpenData].
    * [OpenNeuro].


[Github: MargrietGroenendijk]: https://github.com/MargrietGroenendijk?tab=repositories
[Matplotlib]: https://matplotlib.org
[pandas]: https://matplotlib.org
[numpy]: https://www.numpy.org
[seaborn]: https://seaborn.pydata.org
[bokeh]: https://bokeh.pydata.org/en/latest/
[Github: Brunel-Visualization/Brunel]: https://github.com/Brunel-Visualization/Brunel
[Github: altair-viz/pdvega]: https://github.com/altair-viz/pdvega
[Github: jciskey/pygraph]: https://github.com/jciskey/pygraph
[Github: pixiedust/pixiedust]: https://github.com/pixiedust/pixiedust
[Jupyter Notebook]: https://jupyter.org
[O'Reilly: Thoughtful Data Science - David Taieb]: https://www.oreilly.com/library/view/thoughtful-data-science/9781788839969/
[Developer IBM: Data Science]: https://developer.ibm.com/technologies/data-science/

[PSF (Python Software Foundation)]: https://www.python.org/psf-landing/
[UK Python Assocation]: https://uk.python.org/about/

[CUBRIC (Cardiff Uni Brain Research Imaging Centre)]: https://www.cubrc.org
[PsychoPy]: http://psychopy.org
[pygame]: https://www.pygame.org/news
[pavlovia]: https://pavlovia.org
[Github: mne-tools]: https://github.com/mne-tools
[SciPy]: https://www.scipy.org

[PyMVPA]: http://www.pymvpa.org
[NiPy]: https://nipy.org
[scikit-learn]: https://scikit-learn.org/stable/index.html
[nilearn]: https://nilearn.github.io
[Github: poldracklab/fmriprep]: https://github.com/poldracklab/fmriprep
[Github: voytekresearch/OpenData]: https://github.com/voytekresearch/OpenData
[OpenNeuro]: https://openneuro.org
