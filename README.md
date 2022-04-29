# Non-linear tension-compression asymmetric anisotropic constitutive model

Abaqus/Explicit implementation of a constitutive model capable of describing non-linear anisotropic fibre-reinforced materials with different responses in tension and compression.

The model is presented in the article that is currently under consideration for publication.

## Shared library usage manual
The developed constitutive model is implemented in Abaqus/Explicit via VUMAT subroutines. The subroutine is contained in compiled shared library `explicitU-D.dll`. *For the use of shared libraries, Fortran compilers are not needed.* For shared library usage, follow the next steps:
1. Copy shared library `explicitU-D.dll` in current working directory.
2. Copy `abaqus_v6.env` file either in the working directory or home directory (for Windows that is `C:\users\username\`)
   
   **Note:** abaqus_v6.env is located in `[abaqus_install_dir]/[version]/SMA/site/`
3. Insert the following 2 lines in copied `abaqus_v6.env` file:
   ```
   import os
   usub_lib_dir=os.getcwd()
   ```
4. Run Abaqus/Explicit using following command:

   `abaqus job=[job_name] double=both`

   **Note:** Abaqus solver will automatically include shared library located in the current working directory. Therefore `user` argument should not be used. Only a double-precision version of the shared library is included, therefore option `double=both` is mandatory. 

## Constitutive model usage manual

To use the consitutive model provided in `explicitU-D.dll`, define user matirial in abaqus input file using following command:
```
*Material, name=[arbitrary_name]
*Density
 [density_value],
*User Material, constants=38
 [p1], [p2], [p3], ... , [p38]
```
Parameters 1-9 represents following terms in linear tensile compliance matrix 

**Note:** Because the model is implemented in Abaqus/Explicit, the density of the material has to be defined.

         [p1  p4  p5  0   0   0  ]       [p10 p13 p14  0   0   0  ]
         [p4  p2  p6  0   0   0  ]       [p13 p11 p15  0   0   0  ]
    St = [p5  p6  p3  0   0   0  ]  Sc = [p14 p15 p12  0   0   0  ]
         [0   0   0   p7  0   0  ]       [0   0   0    p16 0   0  ]
         [0   0   0   0   p8  0  ]       [0   0   0    0   p17 0  ]
         [0   0   0   0   0   p9 ]       [0   0   0    0   0   p18]
         
         [p19  p4  p5  0   0   0  ]       [p10 p13 p14  0   0   0  ]
         [p4  p2  p6  0   0   0  ]       [p13 p11 p15  0   0   0  ]
    St = [p5  p6  p3  0   0   0  ]  Sc = [p14 p15 p12  0   0   0  ]
         [0   0   0   p7  0   0  ]       [0   0   0    p16 0   0  ]
         [0   0   0   0   p8  0  ]       [0   0   0    0   p17 0  ]
         [0   0   0   0   0   p9 ]       [0   0   0    0   0   p18]
