![](WENOLogo.png)


# WENO framework

Weighted essentially non-oscillatory library for the framework of OpenFOAM.
Detailed information about the theoretical background and the implementation can 
be found in:

 * [Development of a Finite Volume Solver for Two-phase Incompressible Flows using a Level Set Method](Martin_Development_of_a_Finite_Volume_Solver_for_Two-phase_Incompressible_Flows_using_a_Level_Set_Method.pdf)
 * [Solving the Level Set Equation using High-order Non-oscillatory Reconstruction](Martin_Solving_the_Level_Set_Equation_using_High-order_Non-oscillatory_Reconstruction.pdf)
 * [Presentation: WENOExt](./WENOExt-Presentation.pdf)

**Versions:**

Different versions of the code are structured through tags:

 * [1.0] Version for OpenFOAM 5.x, 6 and 7.
             Uses improved parallising and C++11 features 
 * [0.1] Version for OpenFOAM 2.3.x to OpenFOAM 5.x 

## Authors

 * Tobias Martin <tobimartin2@googlemail.com>
 * Jan Wilhelm Gärtner <jan.gaertner@outlook.de>


## Installation

1. Clone the directory with
    `git clone https://github.com/WENO-OF/WENOEXT.git`

2. Execute `Allwmake` to build the library


### Note to GNU compiler:

GNU compiler version must be higher than 7. For g++ < v7 an error is reported for 
the specialisation template syntax. 
The syntax in the code is according to C++11 standard which is available for g++ v7 and higher. 
 

## Usage

To use the WENO scheme you have to add the library to your controlDict by editing `system/controlDict`

    libs("libWENOEXT.so")

Within your `system/fvSchemes` file,

    divSchemes
    {
    	div(phi,U) 	Gauss WENOUpwindFit 2 1;
    }

Here the first index '2' represents the order of the WENO scheme and the second index can be either
'1' for bounded or '0' for unbounded.

### Expert Parameters

You can select some expert parameters in the file system/WENODict
```
/*--------------------------------*- C++ -*----------------------------------*\
| =========                 |                                                 |
| \\      /  F ield         | OpenFOAM: The Open Source CFD Toolbox           |
|  \\    /   O peration     | Version:  2.3.0                                 |
|   \\  /    A nd           | Web:      www.OpenFOAM.org                      |
|    \\/     M anipulation  |                                                 |
\*---------------------------------------------------------------------------*/
FoamFile
{
    version     2.0;
    format      ascii;
    class       dictionary;
    location    "system";
    object      WENODict;
}
// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //

// This dict contains expert parameters, which modify the standard WENO scheme.

  //- Stencil extension ratio:
  //- < 2.5:  decreased computational effort. May also decrease stability 
	//			and accuracy 
	//- > 2.5 : higher stability. May influence the accuracy of the SVD
	extendRatio		2.5;
	
	//- WENO stencil weighting parameters:
	p				4.0;
	dm				1000.0;

  //- Calculate best conditioned matrix
  //  This can save memory especially for high order WENO scheme 
  //  Increases the calculation time! Default is off
  bestConditioned true;
  
  // Define the tolerance for matrix databank, default is 1E-9
  tol 1E-9;
  
// ************************************************************************* //
```

## Tutorials

The code contains two tutorials from the standard cavity test. 
To run the tutorials execute `./Allrun` in the tutorial/ directory.

## Tests

For some general functions of the solver a unit test file is created, however testing is still incomplete.

## Execute Tests

Testing is performed with the CATCH2 framework. You can compile and execute the tests
by executing `./runTest` in the test directory. Further instructions are found in [here](tests/TestInstructions.md) 

