# Non-linear tension-compression asymmetric anisotropic constitutive model

Abaqus/Explicit implementation of a constitutive model capable of describing non-linear anisotropic fibre-reinforced materials with different responses in tension and compression.

The model is presented in the published article [Non-linear elastic tension–compression asymmetric anisotropic model for fibre-reinforced composite materials](https://doi.org/10.1016/j.ijengsci.2023.103829). When using the model please cite this article.

## Shared library usage manual (Abaqus 6.12)
The developed constitutive model is implemented in Abaqus/Explicit via VUMAT subroutines. Shared library `explicitU-D.dll` is used on Windows machines and `libexplicitU-D.so` is used on Linux machines. **For the use of shared library, Fortran compiler is not needed.** For shared library usage, follow the steps below:

1. Copy shared library `explicitU-D.dll` or `libexplicitU-D.so` in the current working directory.
2. Copy `abaqus_v6.env` file either in the working directory or home directory (for Windows that is `C:\users\username\`)
   
   **Note:** abaqus_v6.env is located in `[abaqus_install_dir]/[version]/SMA/site/`
3. Insert the following 2 lines in copied `abaqus_v6.env` file:
   ```
   import os
   usub_lib_dir=os.getcwd()
   ```
4. Run Abaqus/Explicit using the following command:
   ```
   abaqus job=[job_name] double=both
   ```
   
   **Note:** Abaqus solver will automatically include a shared library located in the current working directory. Therefore `user` argument should not be used.

   **Note:** Only a double-precision version of the shared library is included, therefore option `double=both` is mandatory. 

## Object file usage manual (other Abaqus versions)
The constitutive model can be used with other Abaqus versions (including version 6.12) with object files. Object file `user-xplD.obj` is used on Windows machines and `user-xplD.o` is used on Linux machines. **For the use of object files, Fortran compiler is needed.** For object file usage, follow the steps below:

1. Copy object file `user-xplD.obj` or `user-xplD.o` in the current working directory.

2. Run Abaqus/Explicit using the following command:
   ```
   abaqus job=[job_name] double=both user=user-xplD
   ```

**Note:** Only a double-precision version of the shared library is included, therefore option `double=both` is mandatory.

## Constitutive model usage manual

To use the constitutive model provided in `explicitU-D.dll`, define user material in Abaqus input file using the following command:
```
*Material, name=[arbitrary_name]
*Density
 [density_value],
*User Material, constants=38
 [p1], [p2], [p3], ... , [p38]
```
Parameters `p1`-`p18` represents terms in the linear tension (`St`) and linear compression (`Sc`) compliance tensors as indicated below: 
```
     [p1  p4  p5  0   0   0  ]       [p10 p13 p14  0   0   0  ]
     [p4  p2  p6  0   0   0  ]       [p13 p11 p15  0   0   0  ]
St = [p5  p6  p3  0   0   0  ]  Sc = [p14 p15 p12  0   0   0  ]
     [0   0   0   p7  0   0  ]       [0   0   0    p16 0   0  ]
     [0   0   0   0   p8  0  ]       [0   0   0    0   p17 0  ]
     [0   0   0   0   0   p9 ]       [0   0   0    0   0   p18]
```
Parameters `p19`-`p36` represents terms in the non-linear tension (`Nt`) and non-linear compression (`Nc`) compliance tensors as indicated below: 
```
     [p19 p22 p23 0   0   0  ]       [p28 p31 p32  0   0   0  ]
     [p22 p20 p24 0   0   0  ]       [p31 p29 p33  0   0   0  ]
Nt = [p23 p24 p21 0   0   0  ]  Nc = [p32 p33 p30  0   0   0  ]
     [0   0   0   p25 0   0  ]       [0   0   0    p34 0   0  ]
     [0   0   0   0   p26 0  ]       [0   0   0    0   p35 0  ]
     [0   0   0   0   0   p27]       [0   0   0    0   0   p36]
```
Parameters `p37` and `p38` represents tensile (`nt`) and compressive (`nc`) exponents, respectively. For more information please refer to article [Non-linear elastic tension–compression asymmetric anisotropic model for fibre-reinforced composite materials](https://doi.org/10.1016/j.ijengsci.2023.103829).

**Note:** Because the model is implemented in Abaqus/Explicit, the density of the material has to be defined.
