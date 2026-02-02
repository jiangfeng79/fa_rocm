# Build navi flash_attention python wheel package In Windows
### flash_attn guild guide with rocm7.x for AMD Navi series GPU
Install Git  
Install VS 2022  
Install Windows AMD HIP 7.1.1  
Download roc_wmma headers and unzip the headers to HIP directory: https://github.com/ROCm/rocWMMA/releases/tag/rocm-7.1.1  
Install pytorch, and torch rocm 7.2, from therock build: https://rocm.nightlies.amd.com/v2/gfx110X-all/  
Make a junction, supposingly you have miniconda to manage your python venv, the the venv name is __rocm__:    
```mklink /J "C:\Users\USER_NAME\AppData\Local\miniconda3\envs\rocm\bin" "C:\Users\USER_NAME\AppData\Local\miniconda3\envs\rocm\Scripts"```

Launch a developer powershell for VS 2022, then set the following env var, __DO NOT USE CMD.EXE__:  
> ```powershell
> # ROCm Core Paths
> $env:HIP_PATH = "C:\PROGRA~1\AMD\ROCm\7.1"
> $env:HIP_CLANG_PATH = "C:\PROGRA~1\AMD\ROCm\7.1\bin"
> $env:DEVICE_LIB_PATH = "C:\PROGRA~1\AMD\ROCm\7.1\lib"
> # Update PATH (Prepending)
> $env:PATH = "$env:HIP_CLANG_PATH;$env:PATH"
> # CPath and Architecture
> $env:CPATH = "C:\PROGRA~1\AMD\ROCm\7.1\include;C:\PROGRA~1\AMD\ROCm\7.1\include\rocwmma;$env:CPATH"
> $env:PYTORCH_ROCM_ARCH = "gfx1100"
> # Build Configuration
> $env:DISTUTILS_USE_SDK = "1"
> $env:FLASH_ATTENTION_TRITON_AMD_ENABLE = "FALSE"
> # MSVC & Windows SDK Setup
> $env:MSVC_ROOT = "C:\PROGRA~1\MICROS~2\2022\COMMUN~1\VC\Tools\MSVC\14.44.35207"
> $env:WIN_SDK = "C:\PROGRA~2\WI3CF2~1\10"
> $env:SDK_VER = "10.0.26100.0"
> # Include and Library Paths
> $env:INCLUDE = "$env:MSVC_ROOT\include;$env:MSVC_ROOT\atlmfc\include;$env:WIN_SDK\include\$env:SDK_VER\ucrt;$env:WIN_SDK\include\$env:SDK_VER\um;$env:WIN_SDK\include\$env:SDK_VER\shared"
> $env:LIB = "C:\PROGRA~1\AMD\ROCm\7.1\lib;$env:MSVC_ROOT\lib\x64;$env:WIN_SDK\lib\$env:SDK_VER\ucrt\x64;$env:WIN_SDK\lib\$env:SDK_VER\um\x64"
> ```

Build:  
> ```powershell
> git clone https://github.com/FeepingCreature/flash-attention-gfx11.git
> cd flash-attention-gfx11
> git checkout gel-crabs-headdim512
> git submodule update --init --recursive
> ```  
Edit setup.py, force __WMMA__ flag, and gfx1100 arch:
> ```powershell
> -    cc_flag = ""
> +    cc_flag = "-D__WMMA__"
> -    cc_flag = [f"--offload-arch={arch}" for arch in archs]
> +    cc_flag = [f"--offload-arch=gfx1100" for arch in archs]
> ```  
build the wheel:  
> ```powershell
> .\YOUR_VENV\python.exe setup.py bdist_wheel
> ```  
Find your wheel package in dist folder
