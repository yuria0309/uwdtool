* 한국어
* [English](https://github.com/yuria0309/uwdtool/blob/master/README.md)

UnityWebData 파일을 풀거나 다시 합치는 도구.
단순히 포함하고 있는 파일 정보를 확인할 수도 있어요.

## UnityWebData 파일이란
UnityWebData 파일은 WebGL게임에서 WebAssembly 파일과 함께 로드되어 사용되는 파일로, 주로 에셋, 리소스 및 메타데이터 파일을 모두 묶은 하나의 파일입니다.

## UnityWebData 파일의 구조
여기서는 UnityWebData1.0을 기준으로 바이너리 파일의 구조를 설명합니다.
int값은 리틀 엔디안으로 읽어야합니다.

### 파일 일반 헤더
| 명칭 | 크기(byte) | 자료형 | 설명 |
| :------------: | :------------: | :------------: | :------------: |
| 파일 시그니처 | 16 | String | "UnityWebData1.0\0" |
| 파일 시작 위치 | 4 | int | 나열된 파일 전체에서의 시작 위치로, 첫번째 파일의 위치와 같다 |

### 파일 정보 헤더
이후 파일 시작 위치 전까지 아래 묶음이 각 파일별로 반복됩니다.

| 명칭 | 크기(byte) | 자료형 | 설명 |
| :------------: | :------------: | :------------: | :------------: |
| 파일 위치 | 4 | int | 파일의 시작 위치 |
| 파일 크기 | 4 | int | 파일의 크기 |
| 파일 이름 길이 | 4 | int | 파일 이름의 길이 |
| 파일 이름 | n | String | 파일 이름 |

### 파일 영역
헤더 직후에는 각 파일들이 나열되어있습니다.
어떤 파일을 읽고자 한다면 헤더에서 해당 파일의 위치를 가져와서 그 위치부터 헤더의 파일 크기만큼 읽으면 됩니다.

## 파일 구조 이미지
![img_format](https://raw.githubusercontent.com/yuria0309/uwdtool/master/img/unitywebdata_format.png)

## 사용법

### CLI
```
python UWDTool.py <작업옵션> [-i 입력경로] [-o 출력경로]
```

#### 작업옵션
* -p --pack: 입력경로의 파일들을 UnityWebData 파일로 만들어 출력경로에 저장합니다.
이 때 입력경로는 패킹할 파일들을 포함하고 있는 폴더의 경로입니다.
* -u --unpack: 입력경로의 UnityWebData 파일을 언패킹하여 출력경로에 저장합니다.
이 때 입력경로는 언패킹할 파일의 경로이며, 출력경로는 파일들이 저장될 폴더의 경로입니다.
* -isp --inspect: 입력경로의 UnityWebData파일이 포함하고 있는 파일들의 정보를 출력합니다.
여기에는 파일의 이름과 크기가 표시됩니다. 이 경우 출력경로는 필요하지 않습니다.
* -h --help: 도움말 및 프로그램의 정보를 출력합니다.

### Python
```
from uwdtool import UWDTool


UWDTool.Packer().pack(args.ARG_INPUT, args.ARG_OUTPUT)  # packing

UWDTool.UnPacker().unpack(args.ARG_INPUT, args.ARG_OUTPUT)  # unpacking

UWDTool.Inspector().inspect(args.ARG_INPUT)  # inspector
```