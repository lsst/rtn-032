..
  Technote content.

  See https://developer.lsst.io/restructuredtext/style.html
  for a guide to reStructuredText writing.

  Do not put the title, authors or other metadata in this document;
  those are automatically added.

  Use the following syntax for sections:

  Sections
  ========

  and

  Subsections
  -----------

  and

  Subsubsections
  ^^^^^^^^^^^^^^

  To add images, add the image file (png, svg or jpeg preferred) to the
  _static/ directory. The reST syntax for adding the image is

  .. figure:: /_static/filename.ext
     :name: fig-label

     Caption text.

   Run: ``make html`` and ``open _build/html/index.html`` to preview your work.
   See the README at https://github.com/lsst-sqre/lsst-technote-bootstrap or
   this repo's README for more info.

   Feel free to delete this instructional comment.

:tocdepth: 1

.. Please do not modify tocdepth; will be fixed when a new Sphinx theme is shipped.

.. sectnum::

.. TODO: Delete the note below before merging new content to the main branch.

.. note::

   **Overview.**

   This document is intended for Rubin Data Facility (DF) and Data Access Center (DAC) administrators. 
   It will provide the technical specification for CEs (Grid Computer Element) and RSEs (Rucio Storage 
   Elelment) that will used at Rubin DFs and DACs, as well as instruction on registering these resources
   to Rubin CRIC (Computing Resource Information Catalogue).

   The CE and RSE information in CRIC will be used by Rubin's Panda system and Rucio system for data
   processing.

   Specification of Computing Element (CE)
   =======================================

   Specification of Rucio Storage Element (RSE)
   ============================================

   Registering Resources in CRIC
   =============================


on technical requirements to deploy CEs and RSEs at Rubin DFs (and DACs), as well as registering those info in Rubin Computing Resource Information Catalogue (CRIC) 

.. Add content here.
.. Do not include the document title (it's automatically added from metadata.yaml).

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
