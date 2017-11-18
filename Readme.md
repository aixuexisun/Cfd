# A computional fluid dynamics (CFD) module for FreeCAD

by Qingfeng Xia, 2015 <http://www.iesensor.com/HTML5pptCV>

the team from CSIR South Africa, 2016
+ Oliver Oxtoby <http://www.linkedin.com/in/oliver-oxtoby-05616835>
+ Alfred Bogears <http://www.csir.co.za/dr-alfred-bogaers>
+ Johan Heyns  <http://www.linkedin.com/in/johan-heyns-54b75511>

CSIR team has forked this repo into <https://github.com/jaheyns/Cfd>, focusing on usability for new users and OpenFOAM. Meanwhile, this repo still targets at preparing real-world case files from FreeCAD goemetry for advanced users, which also means user needs to tweak the OpenFOAM case files in production environment. Features from CSIR fork will be picked up regularly in the future. 

changelog and roadmap at [Roadmap.md](./Roadmap.md)

## LGPL license as FreeCAD

Use only with FreeCAD-daily version since Dec 2016. Currently, only OpenFOAM (official 3.0 +) solver is implemented, tested on Ubuntu 16.04

This module aims to accelerate CFD case build up. Limited by the long solving time and mesh quality sensitivity of CFD problem, this module will not as good as FEM module. For engineering application, please seek support from other commercial CFD software.


## Features and limitation

Currently, only *OpenFOAM<https://www.openfoam.com/>* laminar and turbulent solver are supported; while *Fenics<https://fenicsproject.org/>* solver will be added before 2018. 

Before the release of FreeCAD 0.18, the author will focus on implementing infrastructure for CFDworkbench (FoamCaseBuilder), commit to FemWorkbench of FreeCAD directly.


![FreeCAD CFDworkbench screenshot](https://github.com/qingfengxia/qingfengxia.github.io/blob/master/images/FreeCAD_CFDworkbench_screenshot.png)

### Highlight for this initial commit:

1. Python code to run a basic laminar flow simulation is added into CfdWorkbench
2. An independent python module, FoamCaseBuilder (LGPL), can work with and without FreeCAD to build up case for OpenFOAM
3. A general FemSolverControlTaskPanel is proposed for any FemSolver.
4. VTK mesh and result IO, commit to FemWorkbench
5. result can only be viewed in paraview. but possible to export result to VTK format then load back to FreeCAD. (it is implemented in Oct 2016)
6. FenicsSolver is integrate and basic case writer is reaby, but waiting for meshing export

### Limitation:

1. turbulence model case setup is usable, but only laminar flow with dedicate solver and boundary setup can converge
2. OpenFOAM thermal sovler is under development

### Platform support status
- install on Linux:
        Ubuntu 16.04 as a baseline implementation

- install on Windows 10 with Bash on Windows support (yet done):
        Official OpenFOAM  (Ubuntu deb) can be installed and run on windows via Bash on Windows,
        but it is tricky to run windows python script to call the command with python via subprocess module

- install on MAC (not tested):
        As a POSIX system, it is possible to run OpenFOAM and this moduel, assuming openfoam/etc/bashrc has been sourced for bash
      
=============================================
  
## Installation guide

### Prerequisites OpenFOAM related software

- OpenFOAM (3.0+)  `sudo apt-get install openfoam` once repo is added/imported

> see more [OpenFOAM official installation guide](http://openfoamwiki.net/index.php/Installation), make sure openfoam/etc/bashrc is sourced into ~/.bashrc

- PyFoam (0.6.6+) `sudo pip install PyFoam`

- gnuplot.py/gnuplot-py (plot by matplotlib is also under testing)

- paraFoam (paraview for Openfoam, usually installed with OpenFoam)

- gmsh (requested by FemWorkbench)


Debian/Ubuntu: see more details of Prerequisites installation in *Readme.md* in *FoamCaseBuilder* folder

RHEL/SL/Centos/Fedora: Installation tutorial/guide is welcomed from testers

### Install freecad-daily and FEM
After Oct 2016, Cfd boundary condition C++ code (FemConstraintFluidBoundary) has been merged into official master

Make sure netgen and Gmsh function is enabled and installed 
        
### Install Cfd workbemch
from github using
`git clone https://github.com/qingfengxia/Cfd.git`
        
symbolic link or copy the folder into `<freecad installation folder>/Mod`, 
e.g, on POSIX system: 

`sudo ln -s (path_to_CFD)  (path_to_freecad)/Mod`
        

ALTERNATIVELY, use FreeCAD-Addon-Installer macro from <https://github.com/FreeCAD/FreeCAD-addons>

========================================

## Testing

### Test FoamCaseBuider

There is a test script to test installation of this FoamCaseBuilder, copy the file FoamCaseBuilder/TestBuilder.py to somewhere writtable and run 

`python2 pathtoFoamCaseBuilder/TestBuilder.py` 

This script will download a mesh file and build up a case without FreeCAD.

### tutorial to build up case in FreeCAD

Similar with FemWorkbench

+ make a simple part  in PartWorkbench or complex shape in Partdesign workbench

+ select the part and click "makeCfdAnalysis" in CfdWorkbench
> which creats a CfdAnalysis object, FemMesh object, and default materail

+ config the solver setting in property editor data tab on the left combi panel, by single click sovler object

+ double click mesh object to refine mesh

+ hide the mesh and show hte part, so part surface can be select in creatation of boundary condition

+ add boundary conditions by click the toolbar item, and link surface and bondary value

+ double click solver object to bring up the SolverControl task panel
> select working directory, write up case, further edit the case setting up
  then run the case (currently, copy the solver command in message box and run it in new console)

### Test with prebuilt case

A simple example of a pipe with one inlet and one outlet is included in this repo (test_files/TestCase.fcstd). The liquid viscosity is 1000 times higher than water, to make it laminar flow. 
[The video tutorial is here](https://www.iesensor.com/download/FreeCAD_CfdWorkbench_openfoam_tutorial.webm)


Turbulence flow case setup is also usable, see (test_files/TestCaseKE.fcstd), but Reynolds number is set to be 1000, otherwise, calculation will diverge due to bad tetragen mesh. Better CFD meshing is planned for cfmesh, perhaps, snappyHexMesh.


Johan has built a case, see attachment [test procedure on freecad forum](http://forum.freecadweb.org/viewtopic.php?f=18&t=17322)


========================================

## Roadmap

### see external [Roadmap.md](./Roadmap.md)


=======================================

## How to contribute this module

You can fork this module to add new CFD solver or fix bugs for this OpenFOAM solver.

There is a ebook "Module developer's guide on FreeCAD source code", there are two chapters describing how Fem and Cfd modules are designed and implemented.

<https://github.com/qingfengxia/FreeCAD_Mod_Dev_Guide.git> where updated PDF could be found on this git repo

This is an outdated version for early preview:
<https://www.iesensor.com/download/FreeCAD_Mod_Dev_Guide__20161224.pdf>

## Collaboration strategy

Cfd is still under testing and dev, it will not be ready to be included into official in the next 6 m.

Currently route I can imagine:
CFD workbench new developers fork official FreeCAD and my Cfd githttps://github.com/qingfengxia/Cfd.git.

Cfd workbench depends on Fem for meshing and boundary(FemConstraint) and post-processing, most of them are coded in C++ so it is hard to split from Fem. If you need add feature on above, you branch FreeCAD official, not mime (but do let me know, pull request will only accepted if it is fully discussed on forum and reviewed by other dev like Bernd, me). see my ebook on how to pull request. Any other cfd python code, do pull request me (my Cfd.git) e.g. I developed vtk mesh and result import and export feature, git directly to official Fem workbench.

User can install freecad-daily, and git update/install Cfd.git so all got updated code without pain for installation.


