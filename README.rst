############################################
                 Keypoint GUI
############################################
.. image:: /data/micmac_demo.png
   :alt: micmac_demo

Introduction
============

Origin code is from `keypointgui <https://github.com/Kitware/keypointgui>`_.

This project provides a GUI to select pairs of points between two images (see 
`image correspondence <https://en.wikipedia.org/wiki/Correspondence_problem>`_),
which can be saved or used to fit a homography. The GUI functionality is
implemented with wxPython 4.X with opencv-python image processing. Though, the 
tag `wxPython3X` provides compatibility with wxPython 3.X.


Change 
============
1. No error when close the window.

2. Exit boutton.

3. support `Micmac <https://github.com/micmacIGN/micmac>`_ text/bin format, add the data path in the parameter

.. code-block :: console
  
  $ cd data
  $ ./cmd.sh
  $ cd ../keypointgui
  $ python gui.py --dir ../data

Then open the input files:

  Load Left Image -> im0.png
  
  Load Right Image -> im1.png
  
  Load Points -> Homol/Pastisim0.png/im1.png.txt 

or :

  Load Points -> Homol/Pastisim0.png/im1.png.dat

Project Layout
==============
The "source" code resides under the `keypointgui directory`:

- `gui.py` - the implementation of the GUI, which calls upon the layout defined in`form_builder_output.py`. This is the main "executable".

- `gui.fbp` - wxFormBuilder format file (`necessary version to edit GUI <https://ci.appveyor.com/api/projects/jhasse/wxformbuilder-461d5/artifacts/wxFormBuilder_win32.zip?branch=master>`_ or newer `repository here <www.wxformbuilder.org>`_).

- `form_builder_output.py` - automatically generated from `gui.fbp` using wxFormBuilder.

- `/tests/demo.py` - GUI demo.

Installation
============
1. Make sure Python is installed and visible from a command terminal:

.. code-block :: console

  $ python -V

2. Clone this repository into the desired directory:

.. code-block :: console

  $ git clone git@kwgitlab.kitware.com:matt.brown/keypointgui.git
  $ cd keypointgui

3. If wxPython 3.X is already on the system and you do not want to upgrade to 4.x:

.. code-block :: console

  # Check if wxPython 3.X is already installed (print version)
  $ python -c "import wx;print wx.__version__"
  
  # If version 3.X is present, checkout the wxPython3X branch:
  $ git checkout wxPython3X
 
4.1 If using Ubuntu 16.04 (otherwise pip will try to build from source and fail):

.. code-block :: console
  
  $ sudo pip install -U -f https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-16.04 wxPython

4.2 If using Ubuntu with sudo, `GTK should be installed <https://askubuntu.com/questions/1073145/how-to-install-wxpython-4-ubuntu-18-04>`_ (otherwise pip will try to build from source and fail):

.. code-block :: console
  
  $ sudo apt install make gcc libgtk-3-dev libwebkitgtk-dev libwebkitgtk-3.0-dev libgstreamer-gl1.0-0 freeglut3 freeglut3-dev python-gst-1.0 python3-gst-1.0 libglib2.0-dev ubuntu-restricted-extras libgstreamer-plugins-base1.0-dev
  

4.3 If using Ubuntu 20.04 without sudo (otherwise pip will try to build from source and fail):

.. code-block :: console
  
  $ pip install -U -f https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-20.04 wxPython
  
when there is an error `libSDL2-2.0.so.0: cannot open shared object file <https://stackoverflow.com/questions/29711336/libsdl2-2-0-so-0-cannot-open-shared-object-file>`_, install  SDL library, and then 
  
.. code-block :: console
  
  $ export LD_LIBRARY_PATH=/mypath/SDL2-2.0.22/lib:$LD_LIBRARY_PATH
  
  
5. Install with all dependencies (OpenCV and wxPython):

.. code-block :: console

  $ pip install .

6. Use python3.10
 .. code-block ::  console
  
  $ pip install attrdict3
  $ sudo apt install libgtk-3-dev
 

This package can be uninstalled by:

.. code-block :: console

  $ pip uninstall keypointgui

Usage Instructions
==================

You can launch the GUI with:

.. code-block :: console

  $ python -m keypointgui.gui

The GUI is initially empty, but you can load your images using the menu options:

  File -> Load Left Image

  File -> Load Right Image

The top two panes are global views of the loaded images, and the red rectangles
indicate the regions shown magnified in the associated bottom panes. Clicking in
either top pane (or right clicking in the bottom pane) will recenter the zoomed
region, and the mousewheel controls the magnification. Left clicking in either
of the lower images will create a temporary blue point. The same feature should
be left clicked in the other lower image, and then both points will turn red,
establishing an image point correspondence. This process is repeated to build up
a set of image point correspondences between the two images.

Image Alignment
---------------

If the two source images differ in scale or orientation, the task of selecting
points can be challenging. After at least four pairs of points have been
selected, an alignment homography can be fitted to the points using the
`Left-->Right` or `Right-->Left` buttons. To get an accurate alignment, these
initial four points should be selected from the four corners of the image or
spread out as much as possible. In the aligned state, point selection can
proceed in the same manner as previously detailed, and the selected points are
automatically transformed back to the full-resolution, source-image coordinate
system when saving points or generating a homography.

In the aligned state, the `Sync Zooms` options defaults to checked. With this
feature enabled, clicking on either top panel will recenter the zoom regions for
both images onto roughly the same feature.

Saving Points
-------------

The menu option:

  File -> Save Points

will save a text file of the currently selected points. In this file, each row
represents one pair of points, with the first two columns representing the (x,y)
coordinates of the point in the left image and the last two columns representing
the (x,y) coordinates of the point in the right image. The convention for image
coordinates is such that the center of the top left pixel has coordinates (0,0).

Saving Homography
-----------------

The menu options:

  File -> Save Left->Right Homography

  File -> Save Right->Left Homography

saves a homography to a text file that warps coordinates from the left image
into the right image or the right image into the left image, respectively.

