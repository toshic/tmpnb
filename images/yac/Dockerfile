# Designed to be run as 
# 
# docker run -it -p 9999:8888 ipython/latest

FROM ipython/scipystack

MAINTAINER IPython Project <ipython-dev@scipy.org>

# The ipython/ipython image has the full working copy of IPython
WORKDIR /srv/ipython/
#RUN chmod a+rwX /srv/ipython/examples

# Switch IPython to python2
RUN sed -e 's/python3/python2/' -i /usr/local/bin/ipython
RUN apt-get install -y libgeos-dev && ln -s /usr/lib/libgeos_c.so.1 /usr/lib/libgeos.so
RUN pip install apachelog pygeoip basemap --allow-external basemap --allow-unverified basemap

EXPOSE 8888

# jupyter is our user
RUN useradd -m -s /bin/bash jupyter

USER jupyter
RUN ipython profile create

# Workaround for issue with ADD permissions
USER root
# Add examples for YaC2014
ADD notebooks/ /home/jupyter/

RUN chown -R jupyter:jupyter /home/jupyter

USER jupyter

## Final actions for user

ENV HOME /home/jupyter
ENV SHELL /bin/bash
ENV USER jupyter

WORKDIR /home/jupyter/

RUN find . -name '*.ipynb' -exec ipython trust {} \;

CMD ipython2 notebook --no-browser --port 8888 --ip=0.0.0.0 --NotebookApp.base_url=/$RAND_BASE --NotebookApp.tornado_settings="{'template_path':['/srv/ga/', '/srv/ipython/IPython/html', '/srv/ipython/IPython/html/templates']}"
