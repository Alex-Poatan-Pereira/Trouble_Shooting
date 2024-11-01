### 가상환경에 transformers를 설치하는데 자꾸 에러가 뜬다
````
(AI_1) t2024-m0198@t2024-m0198ui-MacBookAir transformers % pip install transformers
Collecting transformers
  Using cached transformers-4.46.1-py3-none-any.whl.metadata (44 kB)
Collecting filelock (from transformers)
  Using cached filelock-3.16.1-py3-none-any.whl.metadata (2.9 kB)
Collecting huggingface-hub<1.0,>=0.23.2 (from transformers)
  Using cached huggingface_hub-0.26.2-py3-none-any.whl.metadata (13 kB)
Collecting numpy>=1.17 (from transformers)
  Using cached numpy-2.1.2-cp313-cp313-macosx_14_0_arm64.whl.metadata (60 kB)
Collecting packaging>=20.0 (from transformers)
  Using cached packaging-24.1-py3-none-any.whl.metadata (3.2 kB)
Collecting pyyaml>=5.1 (from transformers)
  Using cached PyYAML-6.0.2-cp313-cp313-macosx_11_0_arm64.whl.metadata (2.1 kB)
Collecting regex!=2019.12.17 (from transformers)
  Using cached regex-2024.9.11-cp313-cp313-macosx_11_0_arm64.whl.metadata (40 kB)
Collecting requests (from transformers)
  Using cached requests-2.32.3-py3-none-any.whl.metadata (4.6 kB)
Collecting safetensors>=0.4.1 (from transformers)
  Using cached safetensors-0.4.5-cp313-cp313-macosx_11_0_arm64.whl.metadata (3.8 kB)
Collecting tokenizers<0.21,>=0.20 (from transformers)
  Using cached tokenizers-0.20.1.tar.gz (339 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... error
  error: subprocess-exited-with-error
  
  × Preparing metadata (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> [6 lines of output]
      
      Cargo, the Rust package manager, is not installed or is not on PATH.
      This package requires Rust and Cargo to compile extensions. Install it through
      the system's package manager or via https://rustup.rs/
      
      Checking for Rust toolchain....
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
error: metadata-generation-failed

× Encountered error while generating package metadata.
╰─> See above for output.

note: This is an issue with the package mentioned above, not pip.
hint: See above for details.

````
----

AI_1 이라는 가상환경을 생성한 후 import 시 에러가 발생
>
원인으로는
- 누락되거나 오래된 종속성 : 설치하려는 패키지는 설치되지 않았거나 오래된 다른 패키지에 종속되어 있을 수 있다
- 호환되지 않는 python 버전
- 손상되었거나 setuptools의 손상
- 네트워크 문제
- C컴파일러 문제
>
해결 방법
- `pip install --upgrade pip setuptools wheel`
pip, setuptddls, wheel의 최신상태로 업그레이드 해주기
나의 경우는 이 업그레이드를 통해 바로 해결되었다.
>
다른 방법으로는
`pip install transformers=="3.1.0" --no-dependencies ``
no dependencies 옵션을 넣어주니깐 설치가 됨 하지만 여전히 주피터노트북에서는 improt 되지 않는다.
>
#### 주피터 노트북에서 `!pip import transformers` 로 다시 설치하니깐 에러 안뜨고 제대로 작동!