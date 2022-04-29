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
