# PySCUBA_ChangeResidues
## Description
SCUBAChangeResidues is aimed at the amino acid mutations of protein.


## Parameters
## Class
### ChangeResidues
`ChangeResidues` will be initialized with a path containing a protein structure.

main process
```
#initialization
pdbfile='/home/zhanglu/workspace/new/pybind11/backbone/test/3nir_pre.pdb'
cr=ChangeResidues(pdbfile)

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