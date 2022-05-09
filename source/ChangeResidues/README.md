# SCUBAChangeResidues
## Introduction
SCUBAChangeResidues is aimed at the amino acid mutations of protein. For example, 


The `SCUBAChangeResidues.py` contains class `ChangeResidues` which can 
## Class ChangeResidues
`ChangeResidues` will be initialized with a path containing a protein structure.



## functions
| function | description | parameters | wrapper|
|----------|-------------|------------|--------|
|Changesequence|It can mutate many residues by setting the same length sequence of the target chain.|`chainidx`:int, it starts form 0. If you want to change the sequences of the first chain, `chainidx` should be set to 0.\n `newsequence`: string, it should be set to the same length with the target chain you want to mutate.|Molmodeler.changeresidue|
|MakeLVG|The residues will be mutated Leu, Val or Gly according to the secordary structure information.|`chainidx`: it is same with the parameter of function `Changesequence`.|Molmodeler.changeresidue|
|Replaceresidues|Those residues will be mutated by assigning some positions.|`chainidx`: it is same with the parameter of function `Changesequence`. `changesites`:dirtionary, if you want to mutate residue of second and fourth position to His and Gly, respectively, it should be set to {1:'H', 3:'G'}|Molmodeler.changeresidue|
|writepdb|The final protein structure can be saved with this function.|`outputfile`: string, the saved path for the protein structure.|IntrctMol.writepdb|

<table>
  <thead>
    <tr>
      <th>Header
      <th>Another Header
  </thead>
  <tr>
    <td>field 1
    <td>value one
</table>

First of all, we need to build a changeresidues object with `ChangeResidues(pdbfile)`
```
pdbfile='/home/zhanglu/workspace/new/pybind11/backbone/test/3nir_pre.pdb'
changeresidues=ChangeResidues(pdbfile)
```


main process
```
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
```