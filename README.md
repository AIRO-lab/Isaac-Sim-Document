# Isaac-Sim-Document
Isaac Sim에서의 설치 및 튜토리얼 내용

## 설치
### 1. Isaac Sim zip 파일 다운로드
https://docs.isaacsim.omniverse.nvidia.com/5.1.0/installation/download.html
<img width="781" height="607" alt="image" src="https://github.com/user-attachments/assets/a17c6764-ce54-46ae-99b7-e250f6683268" />

| Name | Version | Release Date | Links |
|------|---------|--------------|-------|
| Isaac Sim | 5.1.0 | October 2025 | [Linux (x86_64)](https://download.isaacsim.omniverse.nvidia.com/isaac-sim-standalone-5.1.0-linux-x86_64.zip)) |



### 2. 디렉토리 생성 및 압축 풀기
디렉토리 생성
```bash
mkdir -p ~/isaac-sim
unzip ~/Downloads/isaac-sim-standalone-5.1.0-linux-x86_64.zip -d ~/isaac-sim
```
압축 풀기
```bash
./post_install.sh
./isaac-sim.selector.sh
```
위 명령어를 실행하면 아래와 같은 오류 팝업이 나온다.

<img width="1487" height="213" alt="image" src="https://github.com/user-attachments/assets/3b8b92ed-c6ad-4311-9fb2-5542f2e1cb79" />
IOMMU가 켜져 있으면, Linux에서 CUDA랑 NVIDIA 드라이버가 GPU 간 메모리 복사를 제대로 못 해서 오류, 깨짐, 크래스가 날 수 있다

따라서, BIOS에서 IOMMU를 비활성화를 해야 한다.
