# PATROL DOCKERFILES PROJECT
# --------------------------
# This is the Dockerfile for PATROL install of CMA repository package
# 
# IMPORTANT
# ---------
# The resulting image will have PATROL components installed which are part of PATROL package used
#
# REQUIRED FILES TO BUILD THIS IMAGE
# ----------------------------------
# (1) patrol_cma_package.tar
#     Please create the tar package of PATROL component from v9.6 and above CMA
#
# HOW TO BUILD THIS IMAGE
# -----------------------
# Create new directory on Docker host and put tar package along with this Dockerfile in it.
# Run: 
#      $ docker build -t "patrol:1" . 
#
# Pull base image
# ---------------
FROM centos

# Maintainer
# ----------
MAINTAINER sgaike@bmc.com

# [REVIEW]Environment variables required for this build (Change the values of PATUSER and INSTALLDIR if required)
# -------------------------------------------------------------
ENV TARGET Linux-2-6-x86-64-nptl
ENV BASEDIR /opt/
ENV INSTALLDIR /opt/bmc
ENV PATUSER patrol

# Add the installer file to container file system
# ------------------------------------------------
ADD PA_1103.tar $BASEDIR

#[REVIEW] Setup filesystem and patrol user
# Encrypted value next to -p argument is the password for patrol user.
# To get encrypted value of your password for patrol user use following command.
# openssl passwd -crypt <password>
# ------------------------------------------------------------
RUN useradd -p FL3NKFVoZzQro $PATUSER
RUN mkdir $INSTALLDIR
RUN chmod -R 777 $INSTALLDIR

# SETUP hostname command as this is being used by PATROL silent installer and not available as part of image
# --------------------------------------------------------------
RUN echo "cat /etc/hostname" > /usr/bin/hostname
RUN chmod +x /usr/bin/hostname

# Install PATROL package
WORKDIR /opt/bmc_products
RUN sh RunSilentInstall.sh

# Setup required PATROL environment
# --------------------------------------------------------------
WORKDIR $INSTALLDIR/Patrol3
ENV PATH $PATH:$INSTALLDIR/Patrol3/$TARGET/bin
ENV PATROL_HOME=$INSTALLDIR/Patrol3/$TARGET
ENV PATROL_ADMIN=$PATUSER

# Remove PATROL installer
# --------------------------------------------------------------
RUN rm -rf /opt/bmc_products
# Define default command to start PATROL Agent
# This will start PATROL agent on default port 3181. To change the port replace command with following. 
# CMD ["PatrolAgent", "-p", "6755"]
# -------------------------------------------------
CMD ["PatrolAgent"]
#***************END of Dockerfile****************
