# 운영체제

## Objectives

1. 운영체제는 하드웨어 위에서 돌아가므로 컴퓨터 시스템에 대해서 먼저 작성하겠다.

2. 최근에는 multiprocessor(CPU가 여러개) multicore(한 CPU안에 CPU core가 여러개) multicomputer(컴퓨터가 여러개)처럼 성능을 높이기 위한 병렬 처리 구조가 많다.
하지만 이 글에서는 single CPU, single machine을 위주로 작성하겠다.

3. user mode : 일반 application에서 돌아가는 instruction
kernel mode : 자원을 사용하는 민감한 부분 previlleged instruction 

4. computer environment가 어떤 것이 있는지

5. 오픈소스

## computer system structure

HardWare - Operation System - Application programs

Operation System과 Application programs 사이에 MiddleWare가 존재한다. 
M/W + OS -> System software 라고도 한다. 

M/W에는 compilers, web browsers, database system 등이 있고
Application processors에는 word processors, video games 등이 있다.

## what operating system do

프로세스 관리 : 운영체제에서 작동하는 응용 프로그램 관리  
저장 장치 관리 : 메인 메모리, 하드 디스크 등을 관리  
사용자 관리 : 여러 사용자가 공평하게 사용할 수 있도록 resource를 관리  
시스템 하드웨어 관리 : 프로그램을 컨트롤  

OS는 Users가 컴퓨터를 편하게 사용할 수 있도록 UI제공한다.
kernel은 운영체제에서 아주 핵심적인 부분을 맡고있다.

## computer system organization 

I/O Devices, CPU는 서로 개별적으로 움직인다. 
CPU는 I/O Devices보다 빠르다.
device driver : device controller를 제어 

## common function of interrupts 

interrupts : 정상적인 일을 하는데 어떤 이벤트가 발생해서 그 이벤트가 우선적으로 처리되어야 함을 CPU에게 알리는 것  
interrupt vector : interrupt를 구별하는 값  
service routine : interrupt가 왔을 때 실행해야하는 program  

CPU는 일을 시키고 I/O는 일을 다하면 interrupt를 건다. interrupt를 걸면 그 데이터를 처리.
CPU는 기다리지 않고 다른 일을 하다가 interrupt가 오면 처리한다.
program counter : interrupt를 걸기 전에 CPU가 일을 어디까지 했었는지 알려준다.
resume : save 한 것을 가지고 자기 일을 이어서 함

## Storage Hierarchy

process : 프로그램 수행, 프로그램을 메모리 상에서 실행 중인 작업  
thread : 프로그램 수행, 프로세스 안에서 실행되는 여러 흐름 단위  
DMA : 직접 데이터 이동

Multiprocessors(CPU가 여러개) : parrellel systems, tightly-coupled systems
Asymmetric multiprocessors, Symmetric multiprocessors

Clustered Systems (컴퓨터가 여러 개)
동시에 충돌나는 것을 막아야 한다. (동기화)
