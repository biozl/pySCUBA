SCUBAChangeResidues
===================

Introduction
------------

SCUBAChangeResidues is aimed at the amino acid mutations of protein. For
example,

The ``SCUBAChangeResidues.py`` contains class ``ChangeResidues`` which
can mutate residues with three functions, ``Changesequence``,
``MakeLVG`` and ``Replaceresidues``.

Class ChangeResidues
--------------------

``ChangeResidues`` will be initialized with a path containing a protein
structure.

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

First of all, we need to build a changeresidues object with
``ChangeResidues(pdbfile)``

::

   pdbfile='/home/zhanglu/workspace/new/pybind11/backbone/test/3nir_pre.pdb'
   changeresidues=ChangeResidues(pdbfile)

We can mutate residues with class functions of ``changeresidues``
object. If you want to change many positions of one chain,
``Changesequence`` is recommended to be used. It just needs two
parameters which were introduced in the above table clearly.

::

   changedchainid=0
   newsequence='TTCCPSIVHHHHHHVCRLPGHHHHHHATYTGCIIIPGATCPGDYAN'
   changeresidues.Changesequence(changedchainid,newsequence)

Note: ``newsequence`` should be assigned to the same length sequence
with the original chain.

Residues can be mutated with ``MakeLVG`` according to the secondary
structure. If residues are in helix, they will be mutated to HIS. If
they are in strand or loop, they will be changed to VAL or GLY,
respectively.

::

   changedchainid=0
   changeresidues.MakeLVG(changedchainid)

This is the final function to mutate residues. It and ``Changesequence``
can achieve same function. The difference between them is that
``Replaceresidues`` just needs to be assigned the changed positions not
all length of chain.

::

   changesites={0:'H',10:'H',15:'C',20:'V'}
   changeresidues.Replaceresidues(changedchainid,changesites)

Finally, you can save the modified structure to the path you assigned.
\``\` out
