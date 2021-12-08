# Activity-prediction-for-chemical-compounds
Activity prediction for chemical compounds using various learning algorithms


Data Science Project: Activity prediction for chemical compounds
Chemical compounds can be represented by text strings using Simplified Molecular Input Line Entry Specification (SMILES) - for a brief introduction, see here (Links to an external site.).

In this assignment, your task is to develop a predictive model, using a training set with 148 565 chemical compounds represented by SMILES together with their activity label (0.0 = inactive, 1.0 =active) w.r.t. some biological activity, on the following form:

INDEX,SMILES,ACTIVE
1,C=C(C)c1cccc(C(C)(C)NC(=O)Nc2cc(C)ccc2OC)c1,0.0
2,CCCN(CCC)C(=O)CC(c1ccccc1)c1ccc(C)cc1O,0.0
3,O=C(CC1NCCNC1=O)Nc1ccccc1,0.0...

and apply it to 49 522 instances in a test set on the form:

INDEX,SMILES
148566,Cc1cc(N2CCCCC2)nc(N2CCCCC2)n1
148567,O=C(NCc1ccc(Cl)cc1Cl)C(=O)N1CCCCC1
148568,CCO.Cc1cc(C)n(CCc2nc(-c3ccc(-c4ccccc4)cc3)cs2)n1
...

In order to generate features from SMILES, you may employ the open source toolkit for cheminformatics RDKit, see here (Links to an external site.) and here (Links to an external site.).

Assuming that you have Anaconda (Links to an external site.) already installed, you may install RDKit by

conda install -c conda-forge rdkit

Then you may import and work with the package from Python, e.g.,

>>> from rdkit import Chem

>>> m = Chem.MolFromSmiles('Cc1ccccc1')

'Cc1ccccc1' is a SMILES string and m will be assigned an object representing the corresponding chemical compound, for which various properties may be derived, e.g.,

>>> m.GetNumAtoms()
7

RDKit contains several subpackages that may be used to generate features. Examples of these include:

rdMolDescriptors (see here (Links to an external site.) for details)

>>> import rdkit.Chem.rdMolDescriptors as d
>>> d.CalcExactMolWt(m)
92.062600256

Fragments (see here (Links to an external site.) for details)

>>> import rdkit.Chem.Fragments as f
>>> f.fr_Al_COO(m)
0

Lipinski (see here (Links to an external site.) for details)

>>> import rdkit.Chem.Lipinski as l
>>> l.HeavyAtomCount(m)
7

Fingerprints

A special class of features is the so-called fingerprints, which represent presence or absence of substructures. They can be derived in many different ways. One of these that is included in RDKit is the so-called Morgan fingerprints, which can be generated as follows:

>>> from rdkit.Chem import AllChem

>>> fp = AllChem.GetMorganFingerprintAsBitVect(m,2,nBits=124)
>>> np.array(fp)
array([0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0,0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0,0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0])

Here, the second argument corresponds to the size of the substructures and the third argument corresponds to how many dimensions to map the substructures to (length of the bit vector).

The task

The task is to develop a predictive model, using a suitable representation (feature set) and learning algorithm, in order to maximize the area under ROC curve (AUC) on the test set (for which you are not given the labels). The task also requires that an estimate is provided on what AUC is expected on the test set.
