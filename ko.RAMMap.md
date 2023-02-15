---
category: 마이크로소프트
title: RAMMap
alias: null
visible: true
icon: sysinternals.png
---
# RAMMap
[RAMMap](https://learn.microsoft.com/en-us/sysinternals/downloads/rammap)는 시스템이 [물리 메모리](ko.Memory), 즉 [RAM](https://ko.wikipedia.org/wiki/랜덤_액세스_메모리)을 어떻게 활용하고 있는지 분석하는 [Sysinternals](ko.Sysinternals) 유틸리티 프로그램이다. 일반 사용자들에게는 흔히 "RAM 정리 프로그램"으로 알려져 있으나, 이는 RAMMap에서 제공하는 기능 중 하나인 Empty Working Set으로부터 비롯된 오해이다.

![RAMMap 유틸리티 프로그램](./images/sysinternals_rammap.png)

> 본 프로그램을 이해하려면 [윈도우 NT](ko.WindowsNT) 운영체제를 [컴퓨터 구조론](https://ko.wikipedia.org/wiki/컴퓨터_구조) 관점에서의 개념이 확립되어야 한다.

RAMMap 유틸리티로부터 사용자는 윈도우 운영체제가 메모리를 관리하는 방식을 이해, 어플리케이션의 메모리 점유 분석, 그리고 RAM 할당에 대한 의문점을 답해 줄 수 있다. 아래는 RAMMap에서 표시하는 각 메모리 용도들을 나열하고 설명한다:

* **페이징 풀(Paged Pool)**

    운영체제 또는 [장치 드라이버](ko.WindowsNT#장치-드라이버)에서 관리되는 커널 힙 메모리이며, 페이징 파일로 이동될 수 있는 성질을 지닌다. 제조사에 의해 할당 당시에 태그가 지정되는데, 이를 통해 해당 메모리를 할당한 드라이버 및 목적을 파악할 수 있다. 태그 목록은 [`pooltags.txt`](./references/pooltag.txt) 파일에서 찾아볼 수 있다.

* **비페이징 풀(Nonpaged Pool)**

    운영체제 또는 장치 드라이버에서 관리되는 커널 힙 메모리이며, 페이징 파일로 이동될 수 없는 성질을 지닌다. 제조사에 의해 할당 당시에 태그가 지정되는데, 이를 통해 해당 메모리를 할당한 드라이버 및 목적을 파악할 수 있다. 태그 목록은 `pooltags.txt` 파일에서 찾아볼 수 있다.

* **드라이버 잠금(Driver Locked)**

    [운영체제 구성요소](ko.WindowsNT#운영체제-구성요소) 또는 장치 드라이버에서 직접적으로 관리되는 페이징될 수 없는 메모리 영역이다. 문맥상 비페이징 풀과 유사하지만, 중요한 데이터의 빠른 접근성을 위해 장치 드라이버에 특별히 최적화되어 상대적으로 빠른 대신 더 많은 리소스가 요구된다. [하이퍼-V](ko.HyperV) 또는 [VMware](https://www.vmware.com/)와 같은 하이퍼바이저의 가상 머신에 배정한 RAM 메모리가 호스트 머신에서는 드라이버 잠금 메모리로 할당된다.