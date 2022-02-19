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

   Branch: u/wyang007/panda-rucio-cric. Preliminary. 

.. contents:: Table of Contents
  :depth: 1

**Overview.**

This document is intended for Rubin Data Facility (DF) and Data Access Center (DAC) administrators. 
It will provide the technical specification for CEs (Grid Computer Element) and RSEs (Rucio Storage 
Elelment) that will used at Rubin DFs and DACs, as well as instruction on registering these resources
to Rubin CRIC (Computing Resource Information Catalogue).

The CE and RSE information in CRIC will be used by Rubin's Panda system and Rucio system for data
processing. In the future, it is possible that their usage scope will go beyond the Rubin Data 
Release Production (DRP).

.. _AA-mechanism:

Authentication and Authorization Mechanism in DRP
-------------------------------------------------
Rubin Data Release Production will use X509 and VOMS for authentication and authorization. The Virtual 
Organization (VO) name for Rubin is "lsst". Administrators can register themselves at `Rubin VOMS server
<https://voms.slac.stanford.edu:8443/voms/lsst>`_, subject to approval (a X509 certificate needs to be
loaded in the web browser in order to access the URL)

Rubin will use the following VO attributes for Panda job execution and data movement.

* **/lsst**: Rucio download
* **/lsst/Role=pilot**: Panda job execution and Rucio upload and download
* **/lsst/Role=ddmopr**: Rucio data transfer, upload and download

Specification of Computing Element (CE)
---------------------------------------
Rubin recommends their DFs to use a `ARC-CE version 6 <http://www.nordugrid.org/arc/arc6/admins/ce_index.html>`_
as the gateway to their local batch systems. Rubin's workflow
management system, Panda will submit jobs to the ARC-CE via its REST interface. The ARC-CE should be configured
to support the VO attributes listed in the above :ref:`Authz section<AA-mechanism>`. 

In addition, the following are also needed on the ARC-CE host in order for it to function:

#. /etc/grid-security/certificates (or another location defined by Unix environment variable $X509_CERT_DIR).
#. /etc/grid-security/vomsdir (or another location defined by Unix environment variable $X509_VOMS_DIR).
#. A client tools/library to submit jobs to DFs' local batch systems.

HTCondor-CE version 8 and early releases of version of 9 are also supported. But HTCondor-CE will likely drop
support of GSI/X509 authentication in version 9.3+. 


Specification of Rucio Storage Element (RSE)
--------------------------------------------
Rubin will use the Thirty Party Copy (TPC) mechanism developed by the LHC/WLCG community, in particular, the 
HTTP TPC (xrootd TPC is acceptable if necessary). The TPC service can either come from the storage system 
provider, or by using a Xrootd based standalone service. The latter covers a number of Posix storage, s3
storage, and storages that uses xrootd or HTTP protocols.

If you are using a `dCache system <https://www.dcache.org>`_, the HTTP TPC and xrootd TPC are already supported.
Similar storage systems like EOS, DPM also support TPC.

Xrootd storage (including Xrootd on shared Posix file system such as Lustre and GPFS, and non-local Xrootd 
storage) and s3 storage use `variant of Xrootd service to provide HTTP and xrootd TPCs 
<https://xrootd-howto.readthedocs.io/en/latest/>`_. 

Rucio and FTS will manage the data transfer among RSEs. Users may also download or upload against RSEs. The
required VO support is listed in the above :ref:`Authz section<AA-mechanism>`.

In the future, we may also ask storage systems to provide periodic dumps (list of files) to discover 
dark and missing data.


Registering Resources in CRIC
-----------------------------------

on technical requirements to deploy CEs and RSEs at Rubin DFs (and DACs), as well as registering those info in Rubin Computing Resource Information Catalogue (CRIC) 

.. Add content here.
.. Do not include the document title (it's automatically added from metadata.yaml).

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
