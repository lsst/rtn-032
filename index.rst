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

   Branch: main

.. contents:: Table of Contents
  :depth: 1

.. include:: <isonum.txt>

**Overview.**

This document is intended for Rubin Data Facility (DF) and Data Access Center (DAC) administrators. 
It provides technical specification for CEs (Grid Computer Element) and RSEs (Rucio Storage 
Elelment) that will be used at Rubin DFs and DACs. This document also provides instruction on 
registering these resources to the Rubin CRIC `(Computing Resource Information Catalogue) 
<https://core-cric-docs.web.cern.ch/core-cric-docs/latest/xyz.html>`_.

The CE and RSE information in CRIC will be used by Rubin's Panda workflow management system and 
Rucio distributed data management system for data
production. In the future, it is possible that their usage scope will go beyond the Rubin Data 
Release Production (DRP).

.. _AA-mechanism:

Authentication and Authorization Mechanism in DRP
=================================================
Rubin Data Release Production will use X509 and VOMS for authentication and authorization. The Virtual
Organization (VO) name for Rubin is **lsst**. Administrators can register themselves at the `Rubin VOMS 
server <https://voms.slac.stanford.edu:8443/voms/lsst>`_, subject to approval (a X509 certificate 
needs to be loaded in the web browser in order to access the VOMS server URL)

Rubin will use the following VO attributes for Panda job execution and data movement.

* **/lsst**: for Rucio download
* **/lsst/Role=pilot**: for Panda (production) job submission and Rucio upload, download and deletion
* **/lsst/Role=ddmopr**: for Rucio data transfer, upload, download and deletion

Specification of Computing Element (CE)
=======================================
Rubin recommends their DFs to use a `ARC-CE version 6 <http://www.nordugrid.org/arc/arc6/admins/ce_index.html>`_
as the gateway to their local batch systems. Rubin's workflow
management system, Panda will submit jobs to the ARC-CE via its REST interface. Rubin ARC-CEs should 
be configured to support the VO attributes listed in the above :ref:`Authz section<AA-mechanism>`. 

In addition, the following are also needed on a ARC-CE host in order for it to function:

#. /etc/grid-security/certificates (or another location defined by Unix environment variable **X509_CERT_DIR**).
#. /etc/grid-security/vomsdir (or another location defined by Unix environment variable **X509_VOMS_DIR**).
#. A client tools/library to submit jobs to DF's local batch system.
#. Enable ENV/PROXY: this ARC CE Run Time Environment (RTE) creates a delegated x509 proxy and makes it available to the corresponding batch job via Unix environment variable $X509_USER_PROXY. To enable this RTE, run command **arcctl rte enable ENV/PROXY**. 

In Rubin, we agreed that Panda/Harvester needs to expicitly request ENV/PROXY when submitting jobs. In HTCondor 
job submission (not to confused with HTCondor-CE) using **grid_resource = arc ..."**, this means adding
**arc_resources = <RuntimeEnvironment>ENV/PROXY</RuntimeEnvironment>** to the job description.

HTCondor-CE version 8 and early releases of version of 9 are also supported. But HTCondor-CE will 
likely drop support of GSI/X509 authentication in version 9.3 and beyond. 

There is currently no plan to require a CE at DACs.

Requirement on batch nodes
=============================
The following are required on batch nodes:

#. x86_64 CentOS 7 or equivalent
#. Outbound TCP connection. NAT is acceptable
#. /cvmfs/sw.lsst.eu
#. valid /etc/grid-security/certificates and /etc/grid-security/vomsdir

So far there is no requirement for the number of core and RAM per core (This may change in the 
future). The following are recommended for batch nodes:

* 4GB+ local scratch space per core
* Singularity container.

Specification of Rucio Storage Element (RSE)
================================================
Rubin will use the Thirty Party Copy (TPC) mechanism developed by the LHC/WLCG community, in 
particular, the HTTP TPC (xrootd TPC is acceptable if necessary) to move data. The WLCG TPC 
supports dCache, EOS, DPM, Xrootd, s3, and Posix storage systems. 

If you are using a `dCache system <https://www.dcache.org>`_, EOS and DPM, the HTTP TPC and xrootd 
TPC support are built in to those systems.

Xrootd storage (including Xrootd on shared Posix file system such as Lustre and GPFS, and non-local 
Xrootd storage) and s3 storage use `variants of Xrootd service to provide HTTP and xrootd TPCs 
<https://xrootd-howto.readthedocs.io/en/latest/tpc/#an-example-of-wlcg-tpc-configuration-with-x509-authentication>`_. 

Depend what **local data accessing protocols** Rubin will use, Rubin may support a subset of the above 
storage systems. Currently this protocol is **s3**, though plain webdav/HTTP protocol is also 
supported. This is not a final list.

Rucio and FTS will manage the data transfer among RSEs. Users may also download or upload against 
RSEs. The required VO support is listed in the above :ref:`Authz section<AA-mechanism>`.

In the future, we may also ask storage systems to provide periodic dumps (list of files) to discover 
dark and missing data.

Registering Resources in CRIC
==============================
The CEs and (in the future) RSEs at DFs and DACs will need to be registered in the CRIC, in order 
for Panda and Rucio to use them. Rubin is currently using a `CRIC instance at CERN <https://datalake-cric.cern.ch>`_.
So for now a CERN account is needed in order to add info to this CRIC. Your browser will also 
need a valid X509 certificate. This instance currently have many place referring to "ATLAS". All 
reference to "ATLAS" will be removed in the future but for now, think of "ATLAS XYZ" as "Rubin XYZ"
when add/configuring resources. Eventually, the USDF will host the Rubin CRIC.

Request privileges
------------------
When asked for login, use CERN SSO and type in your CERN username and password. Then check in the
top menu bar for a green key shape icon. Click it and request "ATLAS_ADMIN" privilege (or at least, 
"PANDA ADMINS" privilege. You will need to wait for the privilege to be setup before you can 
continue with the following steps.

Configuring CE and Panda Queue in CRIC
---------------------------------------

There are two main lines of configurations in CRIC for CEs and Panda Queues:

* Federation |rarr| Resource Center (RC) Site |rarr| Computer Element
* ATLAS Site |rarr| Panda Site |rarr| Panda Queue

The Federation is configured. It is `"Rubin" <https://datalake-cric.cern.ch/core/federation/detail/Rubin/>`_.
(This way of using "Federation" is not the same as how ATLAS uses it, but maybe less confusing, and
will not impact the function of CE and Panda Queues).

For other configuration items, followin the following instructions:

#. `Create Resource Center (RC) Site <https://datalake-cric.cern.ch/core/rcsite/create/>`_ 
   (reference RC site: `"SLAC-Rubin") <https://datalake-cric.cern.ch/core/rcsite/detail/SLAC-Rubin/>`_.
#. `Create Computer Element <https://datalake-cric.cern.ch/core/ce/create/>`_ 
   (reference Computer Element: `"SLAC-Rubin-CE-ARC-CE") <https://datalake-cric.cern.ch/core/ce/detail/73/>`_.
#. To create a new "ATLAS Site", go to `ATLAS site "SLAC" <https://datalake-cric.cern.ch/core/experimentsite/detail/SLAC/>`_ 
   and clone it. Change at least boxes "Site Name", "RC site", "admin contact" and "Object status".
#. To create a new "Panda Site", go to `Panda site "SLAC" <https://datalake-cric.cern.ch/core/computeunit/detail/SLAC/>`_ 
   and clone it. Change boxes "Name", "ATLAS Site", and "Object status".
#. To create a new "Panda Queue", go to `Panda queue "SLAC_TEST" <https://datalake-cric.cern.ch/atlas/pandaqueue/detail/SLAC_TEST/>`_ 
   and clone it. Change at least boxes "Name", "Panda site", "Object status".
#. It is possible that you may need to go back to your newly created Panda Site 
   `(from a list of Panda sites) <https://datalake-cric.cern.ch/core/computeunit/list/>`_, and add 
   your newly create Panda Queue to the Panda Site.
#. From `a list of Panda queues <https://datalake-cric.cern.ch/atlas/pandaqueue/list/>`_, click the
   hyper link to the newly created Panda queue, then click "Manage attached Queues" |rarr| "Search Queues".
   Check the newly create CE (Computing Element) and click "Add select queues".

Mission accomplished! Please info the Panda team about the newly created Panda Queue.

Configuring RSE in CRIC
---------------------------------------

Comming later. Currently not required

Site Validation
=================

The following describes a set of simple validation to check site environment. It is not a replacement 
of validation 
that is proposed to the USDF execution team, using real BPS submittion.

A simple validation will use an ARC CE client or HTCondor to submit a job to the WS interface (not
GridFTP interface) of a site's ARC CE, and check the following

#. CE job submission.
#. Availablity of /cvmfs/sw.lsst.eu
#. Outbound TCP connection. This can be as simple as a ping to a USDF DTN node.
#. OS version and availability of software such as Singularity, client tools to (object) storage.
#. Available posix storage layout (df -h)
#. Pointers to local scrach space, object store, Grid infrastructure (CAs, vomsdir), DBs, secrets.

Detail ARC CE xRSL job (or HTCondor job) example is available at ... (under construction)

.. Add content here.
.. Do not include the document title (it's automatically added from metadata.yaml).

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa

