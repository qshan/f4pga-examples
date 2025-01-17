.. _Getting:

Getting F4PGA
#############

This section describes how to install F4PGA and set up a fully working
environment to later build example designs.

Prerequisites
=============

To be able to follow through this tutorial, install the following software:

.. tabs::

   .. group-tab:: Ubuntu

      .. code-block:: bash
         :name: install-reqs-ubuntu

         apt update -y
         apt install -y git wget xz-utils

   .. group-tab:: Debian

      .. code-block:: bash
         :name: install-reqs-debian

         apt update -y
         apt install -y git wget xz-utils

   .. group-tab:: CentOS

      .. code-block:: bash
         :name: install-reqs-centos

         yum update -y
         yum install -y git wget which xz

   .. group-tab:: Fedora

      .. code-block:: bash
         :name: install-reqs-fedora

         dnf install -y findutils git wget which xz


Next, clone the F4PGA examples repository and enter it:

.. code-block:: bash
   :name: get-f4pga

   git clone https://github.com/chipsalliance/f4pga-examples
   cd f4pga-examples

Toolchain installation
======================

Now we are able to install the F4PGA toolchain. This procedure is divided
into three steps:

- installing the Conda package manager,
- choosing an installation directory,
- downloading the architecture definitions and installing the toolchain.

Conda
-----

Download Conda installer script into the f4pga-examples directory:

.. code-block:: bash
   :name: wget-conda

   wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O conda_installer.sh

Choose the install directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The install directory can either be in your home directory
such as ``~/opt/f4pga`` or in a system directory such as ``/opt/f4pga``.
If you choose a system directory, you will need root permission to perform the installation,
and so you will need to add some ``sudo`` commands to the instructions below.

.. code-block:: bash
   :name: conda-install-dir

   export F4PGA_INSTALL_DIR=~/opt/f4pga

Setup and download assets
~~~~~~~~~~~~~~~~~~~~~~~~~

Select your target FPGA family:

.. tabs::

   .. group-tab:: Artix-7

      .. code-block:: bash
         :name: fpga-fam-xc7

         export FPGA_FAM=xc7

   .. group-tab:: EOS S3

      .. code-block:: bash
         :name: fpga-fam-eos-s3

         export FPGA_FAM=eos-s3

Next, setup Conda and your system's environment:

.. code-block:: bash
   :name: conda-setup

   bash conda_installer.sh -u -b -p $F4PGA_INSTALL_DIR/$FPGA_FAM/conda;
   source "$F4PGA_INSTALL_DIR/$FPGA_FAM/conda/etc/profile.d/conda.sh";
   conda env create -f $FPGA_FAM/environment.yml

Download architecture definitions:

.. tabs::

   .. group-tab:: Artix-7

      .. NOTE::
         The ``*-install-*`` package is required regardless of the target device, but you can avoid installing the
         ``*-xc7*_test-*`` packages for architectures that you don't need.

      .. code-block:: bash
         :name: download-arch-def-xc7

         mkdir -p $F4PGA_INSTALL_DIR/xc7/install

         F4PGA_TIMESTAMP='20220523-230829'
         F4PGA_HASH='934b12d'

         for PKG in install xc7a50t_test xc7a100t_test xc7a200t_test xc7z010_test; do
           wget -qO- https://storage.googleapis.com/symbiflow-arch-defs/artifacts/prod/foss-fpga-tools/symbiflow-arch-defs/continuous/install/${F4PGA_TIMESTAMP}/symbiflow-arch-defs-${PKG}-${F4PGA_HASH}.tar.xz | tar -xJC $F4PGA_INSTALL_DIR/${FPGA_FAM}/install
         done

   .. group-tab:: EOS-S3

      .. code-block:: bash
         :name: download-arch-def-eos-s3

         wget -qO- https://storage.googleapis.com/symbiflow-arch-defs-install/quicklogic-arch-defs-d6d05185.tar.gz | tar -xzC $F4PGA_INSTALL_DIR/$FPGA_FAM/

If the above commands exited without errors, you have successfully installed and configured your working environment.

.. IMPORTANT::
  With the toolchain installed, you are ready to build the example designs!
  Examples are provided in separated directories:

  * Subdir :ghsrc:`xc7` for the Artix-7 devices
  * Subdir :ghsrc:`eos-s3` for the EOS S3 devices
