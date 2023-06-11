---
category: 운영체제
title: 커널
---
# 커널
[커널](https://ko.wikipedia.org/wiki/커널_(컴퓨팅))(kernel)은 [운영체제](https://ko.wikipedia.org/wiki/운영체제)의 핵심 프로그램으로 일반적으로 시스템 전체에 대한 제어권을 가진다. [메모리](ko.Memory.md)에 항상 상주하여 하드웨어와 소프트웨어 간 상호작용을 가능케 하여, 운영체제의 아래 역할을 담당한다.

* [장치 드라이버](ko.Driver.md#장치-드라이버)를 통한 하드웨어 리소스 제어: 메모리, 입출력, 암호화 등
* [프로세스](ko.Process.md) 간 위의 하드웨어 리소스 할당에 대한 갈등 중재
* 공통 리소스 활용의 최적화: [CPU](ko.Processor.md) 및 캐시 사용률, 파일 시스템, 네트워크 소켓 등

## 아키텍처
운영체제 설계에 따라 커널은 크게 세 가지의 아키텍처로 설계될 수 있다.

![커널 설계에 따른 아키텍처](https://upload.wikimedia.org/wikipedia/commons/d/d0/OS-structure2.svg)

본 부문에서 자주 인용될 "서비스"란, 호출 가능한 커널 루틴 혹은 그 집합을 의미한다.

### 모놀리식 커널
[모놀리식 커널](https://ko.wikipedia.org/wiki/모놀리식_커널)(monolithic kernel)은 모든 서비스가 단일 프로그램으로 빌드되어 [커널 공간](ko.Processor.md#보호-링)에서 처리하는 구조이다. 단조로운 구조에 관리가 매우 편하고, 단일 프로그램에서 모든 커널 작업 및 서비스가 수행되니 성능 속도가 매우 빠르다. 단, 커널 공간의 특성상 사소한 오류가 시스템 전체에 영향을 줄 수 있는 위험이 항상 존재한다. 운영체제 런타임 도중에 장치 드라이버를 언제든지 불러올 수 있는 모듈성(modularity)을 지원한다.

마이크로소프트의 [MS-DOS](https://ko.wikipedia.org/wiki/MS-DOS) 및 [윈도우 9x](https://ko.wikipedia.org/wiki/윈도우_9x) 시리즈가 모놀리식 커널를 사용하였다.

### 마이크로커널
[마이크로커널](https://ko.wikipedia.org/wiki/마이크로커널)(microkernel)은 운영체제 구동에서 기초적이지만 필연적인 저급 커널 서비스만을 제외한 나머지를 [사용자 공간](ko.Processor.md#보호-링)으로 분리시킨 구조이다: [스케줄링](ko.Processor.md#스케줄링), 메모리 관리, 기초적인 [IPC](ko.Process.md#프로세스-간-통신)가 최소한으로 마련되어야 할 서비스이다. 한편, 사용자 모드에서 고급 커널 서비스를 제공하는 프로그램들을 서버(server)라고 부른다.

> 마이크로커널의 기초적인 IPC 서비스는 사용자 모드에 위치한 서버 혹은 장치 드라이버 간 통신을 위해 반드시 필요한 기능이다.

모놀리식 커널의 단점인 방대한 커널 이미지로 인한 코드 관리의 어려움 및 장치 드라이버 충돌에 의한 커널 전체에 미치는 악영향을 해소하고자 고안되었다. 커널 서비스의 서버화는 커널 개발 및 업데이트 시간을 단축시키지만, 사용자 모드에 위치하여 하드웨어 접근성 효율이 떨어진다. IPC 통신으로 인한 성능 저하 및 커널 동작 절차의 복잡성도 단점으로 함께 지적되었다.

### 하이브리드 커널
[하이브리드 커널](https://ko.wikipedia.org/wiki/하이브리드_커널)(hybrid kernel)은 마이크로커널처럼 서비스에 따라 서버가 나뉘어져 있으나, 모놀리식 커널처럼 전부 (혹은 대부분) 커널 공간에 존재한다. 결국 시스템 안전성 취약점은 여전히 존재하지만, 마이크로커널에 비해 사용자 및 커널 모드 전환이 불필요하여 커널 서비스의 성능 [오버헤드](https://ko.wikipedia.org/wiki/오버헤드)가 없다.

마이크로소프트의 [윈도우 NT](ko.Windows.md)가 하이브리드 커널의 영향을 받은 대표적인 운영체제이다.

# NT 커널
[윈도우 NT](ko.Windows.md) 운영체제의 커널 이미지 `ntoskrnl.exe`는 아래와 같이 구성된다.

<table style="width: 95%; margin: auto;">
<caption style="caption-side: top;">윈도우 커널 이미지의 구성</caption>
<colgroup><col style="width: 15%;"/><col style="width: 15%;"/><col/><col/><col/><col/><col/><col/><col/><col/><col/><col/></colgroup>
<thead><tr><th style="text-align: center;">이미지</th><th style="text-align: center;">계층</th><th colspan="10" style="text-align: center;">구성</th></tr></thead>
<tbody><tr><td rowspan="3" style="text-align: center;"><a href="https://ko.wikipedia.org/wiki/Ntoskrnl.exe"><code>ntoskrnl.exe</code></a><br/>(모듈명: <code>nt</code>)</td><td rowspan="2" style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Executive">Executive</td><td colspan="10" style="text-align: center;">시스템 서비스 담당자</td></tr><tr><td style="text-align: center;">입출력 관리자</td><td style="text-align: center;">캐시 관리자</td><td style="text-align: center;">보안 참조 모니터</td><td style="text-align: center;">전력 관리자</td><td style="text-align: center;">PnP 관리자</td><td style="text-align: center;">메모리 관리자</td><td style="text-align: center;">프로세스 관리자</td><td style="text-align: center;">객체 관리자</td><td style="text-align: center;">구성 관리자</td><td style="text-align: center;">ALPC</td></tr>
<tr><td style="text-align: center;"><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Kernel">Kernel</a></td><td colspan="10" style="text-align: center;">스케줄링, 동기화, 인터럽트 등 기초적인 핵심 함수 제공</td></tr></tbody>
</table>

Executive는 특정 작업을 수행하는 여러 구성원들로 이루어진 `ntoskrnl.exe`의 상위 계층이다. 한편, Kernel 계층은 모듈에서 필요로 하는 기초적인 커널 함수들을 제공하는 하위 계층이며 [마이크로커널](#마이크로커널)의 역할을 담당한다. 이러한 구조의 정립으로 Kernel은 단순히 OS 매커니즘을 구현하고, Executive는 이를 활용하여 실질적인 정책 결정에 기여한다.

아래는 `nt` 모듈의 함수 접두사가 각각 어떤 목적으로 사용되는 지 식별하는 도표이다. 일부 함수는 접두사에 `p`가 추가된 경우가 있는데, 이는 해당 목적 혹은 구성원에서만 사용되는 내부 전용 함수를 의미한다.

<table style="width: 60%; margin: auto;">
<caption style="caption-side: top;">NT 함수 접두사</caption>
<colgroup><col style="width: 15%;"/><col style="width: 85%;"/></colgroup>
<thead><tr><th style="text-align: center;">접두사</th><th style="text-align: center;">의미</th></tr></thead>
<tbody>
<tr><td style="text-align: center;"><code>Cc</code></td><td>파일 시스템 캐시</td></tr>
<tr><td style="text-align: center;"><code>Cm</code></td><td><a href="#구성-관리자">구성 관리자</a>, 커널 모드의 윈도우 레지스트리</td></tr>
<tr><td style="text-align: center;"><code>Csr</code></td><td>Csrss.exe <a href="ko.Subsystem.md#윈도우-서브시스템">윈도우 서브시스템</a> 프로세스와 통신하는 함수</td></tr>
<tr><td style="text-align: center;"><code>Dbg</code></td><td>디버깅 보조 함수: 소프트웨어로 구현된 중단점 등</td></tr>
<tr><td style="text-align: center;"><code>Ex</code></td><td><a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Executive">Executive</a></td></tr>
<tr><td style="text-align: center;"><code>FsRtl</code></td><td>파일 시스템 런타임 라이브러리</td></tr>
<tr><td style="text-align: center;"><code>Hal</code></td><td><a href="#하드웨어-추상-계층">하드웨어 추상 계층</a>(Hardware Abstraction Layer)</td></tr>
<tr><td style="text-align: center;"><code>Io</code></td><td><a href="#입출력-관리자">입출력 관리자</a></td></tr>
<tr><td style="text-align: center;"><code>Ke</code></td><td>핵심 <a href="https://en.wikipedia.org/wiki/Architecture_of_Windows_NT#Kernel">Kernel</a> 루틴</td></tr>
<tr><td style="text-align: center;"><code>Ki</code></td><td>Kernel 내부 전용</td></tr>
<tr><td style="text-align: center;"><code>Ks</code></td><td>커널 스트리밍</td></tr>
<tr><td style="text-align: center;"><code>Kx</code></td><td>인터럽트 처리, 세마포어, 스핀락, 멀티스레딩 및 문맥 교환 함수</td></tr>
<tr><td style="text-align: center;"><code>Ky</code></td><td>트랩 프레임을 생성하고 <code>Kx</code> 접두함수를 호출하는 내부 및 부분 함수</td></tr>
<tr><td style="text-align: center;"><code>Ldr</code></td><td><a href="https://ko.wikipedia.org/wiki/PE_포맷">PE 포맷</a> 로더</td></tr>
<tr><td style="text-align: center;"><code>Lpc</code></td><td><a href="https://ko.wikipedia.org/wiki/로컬_프로시저_호출">로컬 프로시저 호출</a>(LPC)</td></tr>
<tr><td style="text-align: center;"><code>Lsa</code></td><td><a href="https://ko.wikipedia.org/wiki/로컬_보안_인증_하위_시스템_서비스">로컬 보안 인증</a>(LSASS)</td></tr>
<tr><td style="text-align: center;"><code>Mm</code></td><td><a href="#메모리-관리자">메모리 관리자</a></td></tr>
<tr><td style="text-align: center;"><code>Mi</code></td><td>메모리 관리자 내부 전용</td></tr>
<tr><td style="text-align: center;"><code>Nls</code></td><td>네이티브 언어 지원(Native Language Support)</td></tr>
<tr><td style="text-align: center;"><code>Ob</code></td><td><a href="#객체-관리자">객체 관리자</a></td></tr>
<tr><td style="text-align: center;"><code>Pfx</code></td><td>접두어 관리</td></tr>
<tr><td style="text-align: center;"><code>Po</code></td><td><a href="#PnP-관리자">PnP</a> 및 <a href="#전력-관리자">전력 관리</a></td></tr>
<tr><td style="text-align: center;"><code>Ps</code></td><td><a href="ko.Process.md">프로세스</a> 및 <a href="ko.Process.md#스레드">스레드</a> 관리</td></tr>
<tr><td style="text-align: center;"><code>Rtl</code></td><td><a href="https://ko.wikipedia.org/wiki/런타임_라이브러리">런타임 라이브러리</a>; 커널에 직접적으로 가담하지 않지만 네이티브 어플리케이션에서 사용할 수 있는 다양한 유틸리티 함수를 제공한다.</td></tr>
<tr><td style="text-align: center;"><code>Se</code></td><td>보안 관리자, 그리고 Win32 API의 <a href="https://en.wikipedia.org/wiki/Access_token">접근 토큰</a></td></tr>
<tr><td style="text-align: center;"><code>Tp</code></td><td><a href="https://en.wikipedia.org/wiki/Thread_pool">스레드 풀</a> 관리</a></td></tr>
<tr><td style="text-align: center;"><code>Vf</code></td><td><a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/driver-verifier">드라이버 검증 도구</a>(Driver Verifier)</td></tr>
<tr><td style="text-align: center;"><code>Vi</code></td><td>드라이버 검증 도구 내부 전용</td></tr>
<tr><td style="text-align: center;"><a href="ko.WinAPI.md#nt와-zw-접두사-시스템-서비스-비교"><code>Nt</code></a>/<a href="ko.WinAPI.md#nt와-zw-접두사-시스템-서비스-비교"><code>Zw</code></a></td><td>네이티브 <a href="ko.WinAPI.md#시스템-서비스">시스템 서비스</a> API 함수; 두 접두사의 함수는 유사한 (혹은 동일한) 작업을 하지만 커널 모드에서 추가 검증 여부 차이가 존재한다.</td></tr>
</tbody>
</table>