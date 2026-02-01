\# fa\_rocm In Windows

\### flash\_attn guild guide for rocm7.x

Install Git  

Install VS 2022  

Install Windows AMD HIP 7.1.1  

Install pytorch, and torch rocm 7.2  

Make a junction:  

```mklink /J "C:\\Users\\USER\_NAME\\AppData\\Local\\miniconda3\\envs\\rocm\\bin" "C:\\Users\\USER\_NAME\\AppData\\Local\\miniconda3\\envs\\rocm\\Scripts"```



Launch a developer powershell for VS 2022, then set the following env var, DO NOT USE CMD.EXE:  

> ```powershell

> # ROCm Core Paths

> $env:HIP\_PATH = "C:\\PROGRA~1\\AMD\\ROCm\\7.1"

> $env:HIP\_CLANG\_PATH = "C:\\PROGRA~1\\AMD\\ROCm\\7.1\\bin"

> $env:DEVICE\_LIB\_PATH = "C:\\PROGRA~1\\AMD\\ROCm\\7.1\\lib"

> # Update PATH (Prepending)

> $env:PATH = "$env:HIP\_CLANG\_PATH;$env:PATH"

> # CPath and Architecture

> $env:CPATH = "C:\\PROGRA~1\\AMD\\ROCm\\7.1\\include;C:\\PROGRA~1\\AMD\\ROCm\\7.1\\include\\rocwmma;$env:CPATH"

> $env:PYTORCH\_ROCM\_ARCH = "gfx1100"

> # Build Configuration

> $env:DISTUTILS\_USE\_SDK = "1"

> $env:FLASH\_ATTENTION\_TRITON\_AMD\_ENABLE = "FALSE"

> # MSVC \& Windows SDK Setup

> $env:MSVC\_ROOT = "C:\\PROGRA~1\\MICROS~2\\2022\\COMMUN~1\\VC\\Tools\\MSVC\\14.44.35207"

> $env:WIN\_SDK = "C:\\PROGRA~2\\WI3CF2~1\\10"

> $env:SDK\_VER = "10.0.26100.0"

> # Include and Library Paths

> $env:INCLUDE = "$env:MSVC\_ROOT\\include;$env:MSVC\_ROOT\\atlmfc\\include;$env:WIN\_SDK\\include\\$env:SDK\_VER\\ucrt;$env:WIN\_SDK\\include\\$env:SDK\_VER\\um;$env:WIN\_SDK\\include\\$env:SDK\_VER\\shared"

> $env:LIB = "C:\\PROGRA~1\\AMD\\ROCm\\7.1\\lib;$env:MSVC\_ROOT\\lib\\x64;$env:WIN\_SDK\\lib\\$env:SDK\_VER\\ucrt\\x64;$env:WIN\_SDK\\lib\\$env:SDK\_VER\\um\\x64"

> ```



Build:  

> ```powershell

> git clone https://github.com/FeepingCreature/flash-attention-gfx11.git

> cd flash-attention-gfx11

> git checkout gel-crabs-headdim512

> git submodule update --init --recursive

> ```  

Edit setup.py, force \_\_WMMA\_\_ flag, and gfx1100 arch:

> ```powershell

> -    cc\_flag = ""

> +    cc\_flag = "-D\_\_WMMA\_\_"

> -    cc\_flag = \[f"--offload-arch={arch}" for arch in archs]

> +    cc\_flag = \[f"--offload-arch=gfx1100" for arch in archs]

> ```  

build the wheel:  

> ```powershell

> .\\YOUR\_VENV\\python.exe setup.py bdist\_wheel

> ```  

Find your wheel package in dist folder

