Differential Expression
======================

Who: Meeta Mistry

When: April 27, 2016

Times: 9:15am-12:15pm PST

UC Davis Location: `DSI Space, Shields Library, UC Davis Campus  <http://dib-training.readthedocs.org/en/pub/DSI-space-directions.html>`__ 


Contact: Please contact `Jessica Mizzi <mailto:jessica.mizzi@gmail.com>`__ with any questions


`> UC Davis Register Here < <https://www.eventbrite.com/e/differential-expression-workshop-tickets-24603796618>`__
------------------------------------------------------------------------------------------------------------------


.. `> Materials Link Here < <http://chendaniely.github.io/2016-04-06-dib-git-intro-intermediate/>`__
.. -------------------------------------------------------------------------------------------------

.. `> Watch Here < <http://www.youtube.com/watch?v=IpUDlhh8I2E>`__
.. ---------------------------------------------------------------

.. `> Etherpad < <https://etherpad.wikimedia.org/p/2016-04-06-intro-git>`__
.. ------------------------------------------------------------------------


Description
-----------


For RNA-seq data, the strategy taken is to count the number of reads 
that fall into annotated genes and to perform statistical analysis on 
the table of counts to discover quantitative changes in expression 
levels between experimental groups. Easy, right? Not, exactly.

1. We have integer counts and not continous measurements. Data is 
not normally distributed, so statistical methods we applied to 
microarray data don't work here.

2. Replication levels in designed RNA-Seq experiments tend to be 
modest, often not much more than two or three. As a result, there 
is a need for statistical methods that perform well in small-sample 
situations.

3. There is a dependence of variance on the mean (which changes with 
increasing number of replicates)

Solution: Appropriate modelling of the mean-variance relationship in 
DGE data is important for making inferences about differential expression. 
Employing methods which assess the mean-variance relationship to help with 
the problem of estimating biological variability for experiments with small 
number of replicates.

In this module, learners will use `R Statistical Software 
<https://www.r-project.org/>`__ to walk-through activities designed to 
compare the performance of different tools (edgeR, DESeq2, limma-voom) 
for differential expression analysis of RNA-Seq data, and how the 
mean-variance relationship is addressed in datasets with increasing 
number of replicates.

This workshop will be taught remotely and broadcast to our classroom
via Google Hangouts on Air. We will have helpers in our local room to
facilitate the lesson. The lesson will also be streamed to YouTube and
saved there for viewing at a later time. **You only need to register to
this event if you plan on coming to a classroom.**


Computer and workshop requirements
----------------------------------

Attendees will need to bring a computer with an Internet connection.

Please install `R Statistical Software 
<https://www.r-project.org/>`__ before the workshop by following the
instructions below.

Installation instructions
-------------------------

Windows:

Download `R Statistical Software <https://www.r-project.org/>`__

OS X:

Download `R Statistical Software <https://www.r-project.org/>`__
