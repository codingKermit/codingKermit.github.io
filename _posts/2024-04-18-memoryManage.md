---
title : 메모리 관리 기법
date : 2024-04-12 19:30
categories : [자격증, 정보처리기사]
tags : [정보처리기사]
---

안녕하세요 개발구리입니다🐸.

메모리 관리 방법을 글로써 공부하면 개념을 따로따로 배우듯이 공부하게 되어 이해가 잘 되지 않습니다.

사실 메모리 관리의 네가지 방법은 하나하나를 따로 암기할게 아니라 전체가 하나의 프로세스로 동작되는 것인데 단순이 암기만 하려고 하니 더욱 어려웠습니다.

그래서 메모리 관리가 어떤 구조로 되어있는지, 어떤 프로세스로 동작하는지를 이해하기 위해 다시 정리를 하고자 합니다.

<h2>메모리 관리의 개념</h2>
프로그램의 실행이 종료될 때 까지 메모리를 사용 가능한 상태로 유지 및 관리하는 방법
<h3>메모리 관리 기법</h3>
<ol>
    <li>
        <!-- <a hef="#fetchConcept"> -->
            반입 기법
        <!-- </a> -->
    </li>
    <li>
        <!-- <a href="#replacementConcept"> -->
            배치 기법
        <!-- </a> -->
    </li>
    <li>
        <!-- <a href="#allocationConcept"> -->
            할당 기법
        <!-- </a> -->
    </li>
    <li>
        <!-- <a href="#replacementConcept"> -->
            교체 기법
        <!-- </a> -->
    </li>
</ol>
<h2 id="fetchConcept">반입 기법의 개념</h2>
보조 기억장치의 프로그램이나 데이터를 주 기억장치에 언제 적재할 것인지를 결정하는 방법
<ol>
    <li>요구 반입</li>
    <li>예상 반입</li>
</ol>
<h2 id="placementConcept">배치 기법의의 개념</h2>
디스크에 있는 프로세스를 주기억장치의 어느 위치에 저장할 것인지 결정하는 방법
<h3>배치 기법의 종류</h3>
<ol>
    <li>최초 적합 (First Fit)</li>
    <li>최적 적합 (Best Fit)</li>
    <li>최악 적합 (Worst Fit)</li>
</ol>
<h2 id="allocationConcept">할당 기법의 개념</h2>
프로세스를 실행시키기 위해 주기억장치에 어떻게 할당할 것인지에 대한 내용
<h3>할당 기법의 종류</h3>
<ol>
    <li>연속 할당 기법</li>
        <ul>
            <li>단일 분할 할당 기법</li>
            <li>다중 분할 할당 기법</li>
        </ul>
    <li>분산 할당 기법</li>
        <ul>
            <li>페이징 기법(Paging)</li>
            <li>세그먼테이션 기법(Segmentation)</li>
            <li>페이징/세그먼테이션 혼용기법</li>
        </ul>
</ol>
<h2 id="replacementConcept">교체 기법의 개념</h2>
주기억 장치에 있는 프로세스 중 어떤 프로세스를 제거할 것인지를 결정하는 기법
<h3>교체 기법의 종류</h3>
<ol>
    <li>FIFO (Fist In First Out)</li>
    <li>LRU (Least Recently Used)</li>
    <li>LFU (Least Frequently Used)</li>
    <li>OPT (OPTimal Replacement)</li>
    <li>NUR (Not Used Recently)</li>
    <li>SCR (Second Change Replacement)</li>
</ol>
<h2>정리</h2>
메모리 관리의 기법이라는 워딩을 사용해서 개별의 기법들이 독자적으로 수행된다 착각할 수 있지만 전체가 하나의 프로세스로 수행<br>
반입 기법 -> 배치 기법 -> 할당 기법 -> 교체 기법<br>
위의 순서로 수행됨<br>
ex) 프로세스가 실행될 때 이를 주기억장치에 적재하는 기법이 반입 기법.<br>
프로세스를 주기억장치에 적당한 위치에 배치하는 기법이 배치 기법<br>
주기억장치에 적재할 프로세스를 어떻게 할당할 것인지 결정하는 것이 할당 기법<br>
주기억장치에 있는 프로세스 중에 뭘 제거할 것인지를 결정하는 것이 교체 기법<br>
=>프로세스를 메모리에 언제, 어디에, 어떻게 올리고 뭘 지울 것인지를 결정하는 하나의 프로세스.
<ol>
    <li>
        반입 기법
        <ul>
            <li>요구 반입 기법</li>
            <li>예상 반입 기법</li>
        </ul>
    </li>
    <li>
        배치 기법
        <ul>
            <li>최초 배치 기법 (First Fit)</li>
            <li>최적 배치 기법 (Best Fit)</li>
            <li>최악 배치 기법 (Worst Fit)</li>
        </ul>
    </li>
    <li>
        할당 기법
        <ul>
            <li>
                연속 할당 기법
                <ul>
                    <li>단일 분할 할당 기법</li>
                    <li>다중 분할 할당 기법</li>
                </ul>
            </li>
            <li>
                분산 할당 기법
                <ul>
                    <li>페이징 기법 (Paging)</li>
                    <li>세그먼테이션 기법 (Segmentation)</li>
                    <li>페이징/세그먼테이션 혼용 기법</li>
                </ul>
            </li>
        </ul>
    </li>
    <li>
        교체 기법
        <ul>
            <li>FIFO (Fist In First Out)</li>
            <li>LRU (Least Recently Used)</li>
            <li>LFU (Least Frequently Used)</li>
            <li>OPT (OPTimal Replacement)</li>
            <li>NUR (Not Used Recently)</li>
            <li>SCR (Second Change Replacement)</li>
        </ul>
    </li>
</ol>