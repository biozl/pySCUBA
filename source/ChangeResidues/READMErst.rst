SCUBAChangeResidues
===================

Introduction
------------

SCUBAChangeResidues is aimed at the amino acid mutations of protein. For
example,

The ``SCUBAChangeResidues.py`` contains class ``ChangeResidues`` which
can ## Class ChangeResidues ``ChangeResidues`` will be initialized with
a path containing a protein structure.

functions
---------

+---------------+--------------------+-------------------+------------+
| function      | description        | parameters        | wrapper    |
+===============+====================+===================+============+
| C             | It can mutate many | ``chainidx``:int, | Molm       |
| hangesequence | residues by        | it starts form 0. | odeler.cha |
|               | setting the same   | If you want to    | ngeresidue |
|               | length sequence of | change the        |            |
|               | the target chain.  | sequences of the  |            |
|               |                    | first chain,      |            |
|               |                    | ``chainidx``      |            |
|               |                    | should be set to  |            |
|               |                    | 0.                |            |
|               |                    | :raw-latex:`\n `\ |            |
|               |                    |  ``newsequence``: |            |
|               |                    | string, it should |            |
|               |                    | be set to the     |            |
|               |                    | same length with  |            |
|               |                    | the target chain  |            |
|               |                    | you want to       |            |
|               |                    | mutate.           |            |
+---------------+--------------------+-------------------+------------+
| MakeLVG       | The residues will  | ``chainidx``: it  | Molm       |
|               | be mutated Leu,    | is same with the  | odeler.cha |
|               | Val or Gly         | parameter of      | ngeresidue |
|               | according to the   | function          |            |
|               | secordary          | ``                |            |
|               | structure          | Changesequence``. |            |
|               | information.       |                   |            |
+---------------+--------------------+-------------------+------------+
| Re            | Those residues     | ``chainidx``: it  | Molm       |
| placeresidues | will be mutated by | is same with the  | odeler.cha |
|               | assigning some     | parameter of      | ngeresidue |
|               | positions.         | function          |            |
|               |                    | ``                |            |
|               |                    | Changesequence``. |            |
|               |                    | ``changesi        |            |
|               |                    | tes``:dirtionary, |            |
|               |                    | if you want to    |            |
|               |                    | mutate residue of |            |
|               |                    | second and fourth |            |
|               |                    | position to His   |            |
|               |                    | and Gly,          |            |
|               |                    | respectively, it  |            |
|               |                    | should be set to  |            |
|               |                    | {1:‘H’, 3:‘G’}    |            |
+---------------+--------------------+-------------------+------------+
| writepdb      | The final protein  | ``outputfile``:   | IntrctMo   |
|               | structure can be   | string, the saved | l.writepdb |
|               | saved with this    | path for the      |            |
|               | function.          | protein           |            |
|               |                    | structure.        |            |
+---------------+--------------------+-------------------+------------+

.. raw:: html

   <table>

.. raw:: html

   <thead>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Header

.. raw:: html

   <th>

Another Header

.. raw:: html

   </thead>

.. raw:: html

   <tr>

.. raw:: html

   <td>

field 1

.. raw:: html

   <td>

value one

.. raw:: html

   </table>

https://www.daimajiaoliu.com/daima/7b73864bfa98000

First of all, we need to build a changeresidues object with
``ChangeResidues(pdbfile)``

::

   pdbfile='/home/zhanglu/workspace/new/pybind11/backbone/test/3nir_pre.pdb'
   changeresidues=ChangeResidues(pdbfile)

main process

::

   #initialization
   #parameters
   changedchainid=0
   newsequence='TTCCPSIVHHHHHHVCRLPGHHHHHHATYTGCIIIPGATCPGDYAN'
   changesites={0:'H',10:'H',15:'C',20:'V'}

   #function
   cr.Changesequence(changedchainid,newsequence)
   cr.MakeLVG(changedchainid)
   cr.Replaceresidues(changedchainid,changesites)

   #output
   outputfile='/home/zhanglu/workspace/new/pybind11/backbone/test/changesequence.pdb'
   cr.writepdb(outputfile)